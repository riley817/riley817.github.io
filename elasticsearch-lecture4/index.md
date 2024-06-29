# [Elasticsearch 검색 엔진 구축 강의] Elasticsearch 기본 동작



## 1. 인덱스 생성하기
- 인덱스는 문서`document`의 모음이다.

{{< admonition type=tip title="인덱스를 생성하는 방법" open=true >}}
- **Index Settings를 정의한다.**
- Index Mappings를 정의한다. 
- **사용자 정의된 도큐먼트를 인덱싱한다.** 
{{< /admonition >}}

## 2. 인덱스의 settings

### Static Index Settings
- `index.number_of_shards` : 인덱스가 가져야 하는 Primary 샤드의 개수 설정. 기본값 5

### Dynamic Index Settings
- `index.number_of_replicas` : 각 기본 샤드의 복제본 (Replica 샤드 개수 설정). 기본값은 1
- `index.refresh_interval` : 검색 commit point 를 만드는 refresh interval 설정 (새로 고침 작업수행 빈도). -1 비활성화. 기본값 1s
- `index.routing.allocation.enable` : 인덱스의 샤드들의 라우팅 허용 설정

### Other Settings
* Analysis, Mapping, Slowlog ... 

## 3. 인덱스 생성

### 인덱스 Settings로 인덱싱 하기
```bash
# PUT 메소드를 이용한다.
curl -X PUT "localhost:9200/twitter" -H 'Content-Type: application/json' -d
{
    "settings" : {
        "index" : {
            "number_of_shards" : 3, 
            "number_of_replicas" : 2 
        }
    }
}
```

## 4. 인덱스 삭제하기
DELETE 메소드를 사용한다. 인덱스 삭제시는 조심해서 삭제해야 한다. 주로 nginx와 같은 웹서버를 앞단에 두어 특정 IP 에서만 DELETE Method를 요청할 수 있도록 설정하거나 혹은 Index의 Read only 설정을 사용하여 아예 삭제 할 수 없도록 설정하는 것이 좋다.

```bash
curl -X DELETE -H 'Content-Type: application/json' http://{ES_URL}:9200/{index}
```

### Read only로 삭제 방지, true/null로 설정/해제
```
PUT twitter/_settings 
{
    "index.blocks.read_only_allow_delete": true 
}
```

## 5. 인덱스 조회

### 인덱스 세팅 확인하기
```bash
GET twitter/_settings
```

### 인덱스 상태 확인
- 인덱스의 사이즈, Document 개수, 실행된 명령 정보들 확인하기
```bash
GET twitter/_stats
```

### 인덱스 샤드 및 세그먼트 정보 확인
```bash
GET twitter/_segments
```

### 인덱스 요약 정보
```bash
GET _cat/indices?v
GET _cat/indices/twitter?v
```

### 문서 색인 및 조회
- 인덱스는 미리 정의된 샤드 개수에 의해 나누어짐
- **한 번 설정한 샤드 개수는 변경 불가**
- 문서는 인덱싱 할 때 랜덤한 string을 Document ID로 할당받거나 사용자가 정의한 Document ID로 생성된다.
- 사용자는 생성된 Document ID로 Document를 조회 할 수 있다.

{{< admonition type=info title="샤드 할당 알고리즘" open=true >}}
- shard = hash(routing) % number_of_primary_shards
{{< /admonition >}}

### 인덱싱의 필수 조건
- primary shard가 항상 제일 먼저 `writing`되어야 한다.
- primary shard가 `writing`이 전부 완료된 이후에 replica shard로 복제한다.


## 참고
+ [Elasticsearch Reference - index modules](https://www.elastic.co/guide/en/elasticsearch/reference/current/index-modules.html#index-modules-settings)

