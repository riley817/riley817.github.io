# Graph Database 리서치 - Follow, Tag 기능을 고도화 해보자! (2)


기존 Amazon Aurora(postgreSQL) 의 데이터를 필요한 데이터만 추출하여 옮겨 보자. go 언어를 이용하여 `user`, `follow`  의 필요한 부분을 추출하여 `AWS Neptune` 으로 옮기는 코드를 작성한다.

## 1. 라이브러리 세팅
`gremlin-go` 라이브러리를 사용하였다.
- [https://github.com/apache/tinkerpop/tree/master/gremlin-go](https://github.com/apache/tinkerpop/tree/master/gremlin-go)

## 2. Neptune 연결 설정
```go
// 연결 설정
remoteConn, err = gremlingo.NewDriverRemoteConnection(fmt.Sprintf("wss://%s:8182/gremlin", neptuneEndpoint),
		func(settings *gremlingo.DriverRemoteConnectionSettings) {
			settings.TraversalSource = "g"
			settings.TlsConfig = &tls.Config{InsecureSkipVerify: true}
		}
)

// 그래프 탐색을 위한 리모트 연결
g = gremlingo.Traversal_().WithRemote(remoteConn)
```
{{< admonition tip "Note" true >}}
AWS Neptune TLS 인증서 형식은 Go 1.18 버전 이상부터는 지원되지 않으므로 `DriverRemoteConnection` 에 TLS 설정을 추가해야 한다.
{{< /admonition >}}

## 3. 메소드 정의하기

### Vertex 삽입하기

먼저 회원 테이블을 조회하여 vertex로 생성한다. vertex의 회원아이디는 기존 데이터베이스의 회원 테이블에서 사용하는 아이디(PK) 일치 시킨다. vertex의 속성값으로는 닉네임과 생년월일을 문자열 형태로 저장한다.

먼저 이미 등록되어있는 vertex 인지 체크한다. (나중에 알았지만 ID 가 같으면 그냥 덮어쓰는 것 같다.)

```go
func existsUserVertex(vertexId string) (bool, error) {
	_, err := g.V().HasLabel(userLabel).HasId(vertexId).Next()

  // 특정 에러값을 구분하고 싶은데 private 값이라 임의로 하드코딩하여 비교했다... ^.^ 
	if err != nil && err.Error() != GremlinErrNextNoResultsLeftError {
		return false, err
	}
  
	if err != nil && err.Error() == GremlinErrNextNoResultsLeftError {
		return false, nil
	}

	return true, nil
}
```

```go
// addUserVertex 유저 vertex 를 생성한다.
func addUserVertex(userInfo *User) error {

  // 이미 존재하는 vertex 여부를 체크한다.
	exists, err := existsUserVertex(userInfo.userId)
	if err != nil {
		return err
	}

	if exists {
		return nil
	}

	nickname, _ := userInfo.nickname.Value()
	birth, _ := userInfo.birth.Value()

  // 그래프에 vertex 추가
	_, createdErr := g.AddV(userLabel).Property(gremlingo.T.Id, userInfo.userId).
		Property("nickname", nickname).
		Property("birth", birth).Next()

	if createdErr != nil {
		return createdErr
	}
	return nil
}
```
일단 트랜잭션까지… 고려는 무시 

```go
// 회원 데이터베이스에서 현재 유효한 유저정보 조회 (아이디, 닉네임, 생년월일)
rows, err := db.Query(`select user_id, nickname, to_char(birth, 'YYYY-MM-DD') as birth from users u where u.deleted_at is null`)
	if err != nil {
		panic(err)
	}

  // row 수 만큼 vertex 생성!
	for rows.Next() {
		u := &User{}
		if err := rows.Scan(&u.userId, &u.nickname, &u.birth); err != nil {
			panic(err)
		}

		if err := addUserVertex(u); err != nil {
			panic(err)
		}
	}
```

### vertex 간 edge 설정

follow 같은 경우 follower, following 처럼 양방향성을 가지고 있다. 모델을 보는 관점에 따라 다르겠지만 서로 상호관계를 갖고 있기 때문에 아래처럼 양방향성 혹은 방향성이 없다고도 말할 수 있다.

{{<image src="/posts/images/dbms/graphdatabase/Untitled.png" caption="">}}

하지만 위처럼 방향성이 없는 관계를 나타낼 수 없기 때문에 아래처럼 불필요한 관계를 더 만들어 낼 수도 있지만 되도록이면 아래처럼 임의의 방향으로 관계를 설정하도록 하는것이 좋다고 한다. (그래프 탐색 때문 성능때문에 임의의 단방향으로 설정 하는 것 같은데 데이터를 방향성으로 탐색할때 어떻게 해야하는지 모르겠다… 어렵네?!

{{<image src="/posts/images/dbms/graphdatabase/neptune1.png" caption="">}}

일단 나는 follow라는 레이블을 갖는 edge를 만들고 현재 follow DB 구조와 동일하게 user_id → target_id 방향으로 follow edge를 생성했다. 만약 맞팔로우 관계일경우 edge 속성에 서로 팔로우 여부를 true로 저장하게 끔 구현했다.

### edge 추가

회원 아이디를 가지고 vertex 의 객체를 가져온다.

```go
func getUserVertex(vertexId string) (*gremlingo.Result, error) {
	res, err := g.V().HasLabel(userLabel).HasId(vertexId).Next()
	if err != nil {
		if err.Error() == GremlinErrNextNoResultsLeftError {
			return nil, ErrDataNotFound
		}
		return nil, err
	}
	return res, err
}
```

from vertex 에 edge가 존재하는지 탐색한다. 

```go
// findFollowEdge
func findFollowEdge(fromVertexId, toVertexId string, toObj interface{}) (string, error) {
   res, err := g.V().HasLabel(userLabel).HasId(fromVertexId).BothE(followEdgeLabel).Next()

   if err != nil && err.Error() != GremlinErrNextNoResultsLeftError {
      return "", err
   }

   if res == nil {
      return "", ErrDataNotFound
   }

   inV, _ := res.GetVertex()
   outV, _ := res.GetVertex()

   if inV.Id == toVertexId || outV.Id == toVertexId {

   } else {
      return "", ErrDataNotFound
   }

   edgeRes, err := res.GetEdge()
   if err != nil {
      return "", err
   }
   return edgeRes.Id.(string), err
}
```

from → to 각각 vertex를 조회 후 follow edge가 없으면 from→to 방향으로 follow edge를 추가 있는 경우는 edge의 속성의 f4f 값을 true로 변경한다.

```go
func addFollowEdge(fromId, toId string) error {
   // 1. find from vertex
   fromObj, err := getUserVertex(fromId)
   if err != nil {
      log.Printf("[skip] skip %s", fromId)
      return nil
   }

   // 2. find to vertex
   toObj, err := getUserVertex(toId)
   if err != nil {
      log.Printf("[skip] skip %s", toId)
      return nil
   }

   // 3. follow edge 가 존재 하는지 체크
   edgeId, err := findFollowEdge(fromId, toId, toObj)
   if err != nil && !errors.Is(err, ErrDataNotFound) {
      return err
   }

   // 3.1 edge 가 없는 경우
   if errors.Is(err, ErrDataNotFound) {
      _, err = g.V(fromObj.Data).AddE(followEdgeLabel).From(fromObj.Data).To(toObj.Data).
         Property(f4fEdgeProp, false).Next()

			// 완료 나가리
      return nil
   }

   if err != nil {
      panic(err)
   }

   // 3.2 edge 가 있는 경우
   _, err = g.E().HasLabel(followEdgeLabel).HasId(edgeId).Property(f4fEdgeProp, true).Next()
   if err != nil {
      return err
   }
   // 완료 나가리 
   return nil
}
```

`**follow` 테이블을 조회하여 vertex 간 edge 정보를 추가한다.**

```go
// follow 테이블에서 edge 관계를 생성

	followRows, findFollowErr := db.Query(`select f.user_id as from_id, f.target_id as to_id from follow f where f.deleted_at is null order by f.created_at asc`)
	if findFollowErr != nil {
		panic(findFollowErr)
	}

	for followRows.Next() {
		var fromId, toId string
		if err := followRows.Scan(&fromId, &toId); err != nil {
			panic(err)
		}

		log.Printf("[addFollow] %s -> %s", fromId, toId)

		if err := addFollowEdge(fromId, toId); err != nil {
			panic(err)
		}
	}
```

## 결과
{{<image src="/posts/images/dbms/graphdatabase/neptune3.png" caption="">}}
테스트 때문에 한 계정에 팔로우로 몰아주었는데 비쥬얼라이징 했을때 다소 징그러운 모습이 되어버렸다… 

## 마치며

데이터는 성공적으로 마이그레이션 했으나, 내가 원하는 결과 (사용자의 팔로우의 팔로우 중 사용자와 아직 팔로우 관계가 아닌 유저를 조회) 를 정확하게는 뽑아내지는 못하였다. `gremlin` 언어가 사용하기 어려운 부분도 있었지만 아직 그래프 데이터베이스에 대해 잘 이해하고 있지 못해서 그런 것 같다. 또한 팔로우 관계처럼 단방향, 양방향을 구분해야 할 경우 어떻게 설계해야할지 어려운 부분이 있었다. 실제 시스템에 적용하기 위해서는 더 많은 공부가 필요할 것 같다. 😇😇😇😇

인스턴스는 삭제 했다. 빠이

{{<image src="/posts/images/dbms/graphdatabase/neptune4.png" caption="">}}
