# 가상 면접 사례로 배우는 대규모 시스템 설계 기초 Study [11장] 뉴스 피드 시스템 설계


{{< admonition type=tip title="Note" open=true >}}
팀 내에서 진행하는 Study 정리 입니다.
{{< /admonition >}} 

{{< admonition type=question title="함께 논의하고 싶은 주제" open=true >}}
- 이번 챕터도 굉장히 우리 서비스와 밀접한 내용이여서 매우 유익하고 좋았습니다.
- 대부분 설계에서 캐시를 많이 사용하고 있는 것 같습니다. 우리 서비스의 콘텐츠 쪽에는 도입하고 있는데 유저 쪽에도 도입을 해서 불필요한 서비스 의존관계를 없애면 좋을 것 같습니다.
{{< /admonition >}}

## 뉴스피드 (news feed)
뉴스 피드는 사용자의 홈 페이지 중앙에 지속적으로 업데이트 되는 스토리들로 사용자 상태 정보 업데이트, 사진, 비디오, 링크, 앱 활동과 팔로우, 페이지, 그룹으로부터 오는 좋아요 등을 포함

## 1. 문제 이해 및 설계 범위 확정

### 요구사항 파악하기
- 웹과 앱 둘다 지원
- 뉴스 피드 페이지를 새로운 스토리에 올릴 수 있고 친구들이 올리는 스토리를 볼 수 있어야 한다.
- 시간 흐름 역순으로 정렬
- 최대 5,000 명의 친구를 갖을 수 있다.
- 트래픽 규모 : 매일 천만명이 방문
- 이미지, 비디오 등 미디어 파일 포함

## 2. 개략적 설계안 제시 및 동의 구하기
### 2.1 뉴스 피드 API
- HTTP 프로토콜 기반
- API 피드발행, 피드 읽기 API

#### 피드 발행 API
- 새 스토리를 포스팅 한다.
- HTTP POST 형태로 요청
> POST /v1/me/feed
> - Body: 포스팅 내용
> - Authorization 헤더 : API 호출을 인증하기 위해 사용

#### 피드 읽기 API
- 뉴스 피드를 가져오는 API. 
> GET /v1/me/feed
> - Authorization 헤더 : API 호출을 인증하기 위해 사용

### 2.2 피드 발행
{{<figure src="/posts/images/system-design-interview/system-design-figure-11-2.png#center">}}

- **사용자** : 모바일 앱이나 브라우저에 새 포스팅을 올리는 주체 
- **로드밸런서** : 트래픽을 웹 서버들로 분산
- **웹 서버** : HTTP 요청을 내부 서비스로 중계하는 역할
- **포스팅 저장 서비스** : 새 포스팅을 데이터베이스와 캐시에 저장
- **포스팅 전송 서비스** : 새 포스팅을 친구의 뉴스 피드에 푸시한다. 뉴스 피드 데이터는 캐시에 보관하여 빠르게 읽어 갈 수 있도록 한다.
- **알림 서비스** : 친구들에게 새 포스팅이 올라왔음을 알리거나 푸시 알림을 전송

### 2.3 뉴스 피드 

{{<figure src="/posts/images/system-design-interview/system-design-figure-11-3.png#center">}}
- 사용자 : 뉴스 피드를 읽는 주체다
- 로드밸런서 : 트래픽을 웹 서버들로 분산
- 웹 서버 : 트래픽을 뉴스 피드 서버로 보냄
- 뉴스 피드 서비스 : 캐시에서 뉴스 피드를 가져오는 서비스
- 뉴스 피드 캐시 : 뉴스 피드를 렌더링 할 때 필요한 피드 ID 보관

## 3. 상세 설계

### 3.1 피드 발행 흐름 상세 설계
{{<figure src="/posts/images/system-design-interview/system-design-figure-11-4.png#center">}}

#### 웹서버
- 클라이언트 통신 뿐만 아니라 인증이나 처리율 제한 등의 기능도 수행
- 인증 토큰을 헤더에 넣고 API를 호출해야 포스팅 할 수 있게 해야 한다.
- 스팸을 막고 유해한 콘텐츠가 자주 올라오는 것을 방지하기 위해 특정 기간 동안 사용자가 올릴 수 있는 포스팅 수 제한

#### 포스팅 전송(팬아웃) 서비스
- 사용자가 새 포스팅을 그 사용자와 친구 관계에 있는 모든 사용자에게 전달하는 과정
- 쓰기 시점 팬아웃(푸시 모델), 읽기 시점 팬아웃(풀 모델)

**쓰기 시점에서 팬아웃 하는 모델** 
- 새로운 포스팅을 기록하는 시점에 뉴스 피드 갱신
- 포스팅이 완료되면 바로 해당 사용자의 캐시에 해당 포스팅 기록
- 장점
  - 실시간으로 갱신, 사용자에게 즉각 전송
  - 기록시 갱신되므로 뉴스 피드를 읽는 데 드는 시간이 짧아짐

- 단점
  - 친구가 많을 경우 목록에 있는 사용자에게 갱신시 많은 시간 소요 (핫키 문제)
  - 서비스를 자주 이용하지 않는 사용자의 피드도 갱신

**읽기 시점에서 팬아웃하는 모델** 
- 피드를 읽어야 하는 시점에 뉴스 피드 갱신
- 요청 기반(On-demand) ahepf
- 사용자는 본인 홈페이지에 타임 라인을 로딩하는 시점에 새로운 포스트를 가져옴

- 장점
  - 비활성화된 사용자, 또는 서비스에 거의 로그인하지 않는 사용자의 경우 이모델이 유리
  - 로그인하기까지 어떤 컴퓨터 자원도 소모하지 않음
  - 데이터를 친구 각각에게 푸시할 필요가 없으므로 핫키 문제도 생기지 않음
- 단점
  - 뉴스 피드를 읽는데 많은 시간이 소요

> 두 가지 결합하여 대부분의 사용자에 대해서는 푸시 모델을 사용하고 친구나 팔로어가 많은 사용자의 경우 팔로어로 하여금 해당 사용자의 포스팅을 필요할 때 가져가는 풀 모델을 사용하여 시스템 과부하를 방지

#### 팬 아웃 서비스
{{<figure src="/posts/images/system-design-interview/system-design-figure-11-5.png#center">}}

1. 그래프 데이터베이스에서 친구 ID 목록을 조회. 그래프 데이터베이스는 친구 관계나 친구 추천을 관리하기 적합
2. 사용자 정보 캐시에서 친구들의 정보를 조회. 사용자 설정에 따라 일부 친구 걸러넴
3. 친구 목록과 새 스토리 포스팅 ID를 메시지 큐에 저장
4. 팬아웃 작업서버가 메시지 큐에 데이터를 꺼내 뉴스 피드 데이터를 뉴스 피드 캐시에 저장
  - 뉴스 피드 캐시는 [포스팅 ID, 사용자 ID]의 순서쌍을 보관하는 매핑 테이블
  - 메모리 공간을 위해 아이디 값만 저장

#### 피드 읽기 흐름 상세 설계
- 미디어 같은 콘텐츠는 CDN에 저장
{{<figure src="/posts/images/system-design-interview/system-design-figure-11-7.png#center">}}

1. 사용자가 뉴스 피드를 읽으려는 요청 전송
2. 로드밸런서가 웹 서버 중 하나로 전달
3. 웹 서버는 피드를 가져오기 위해 뉴스 피드 서비스를 호출
4. 뉴스 피드 서비스는 뉴스 피드 캐시에서 포스팅 ID 목록을 가져옴
5. 뉴스 피드에 표시할 사용자 이름, 사용자 사진, 포스팅 콘텐츠, 이미지 등 사용자 캐시와 포스팅 캐시에서 가져와 완전한 뉴스 피드를 만듬
6. 생성된 뉴스 피드를 JSON 현태로 만드러 클라이언트에게 보냄

### 3.2 캐시 구조

{{<figure src="/posts/images/system-design-interview/system-design-figure-11-8.png#center">}}

- 뉴스 피드 : 뉴스 피드의 ID를 보관
- 콘텐츠 : 포스팅 데이터 보관, 인기 콘텐츠는 따로 보관
- 소셜 그래프 : 사용자 간 관계 정보 보관
- 행동 : 포스팅에 대한 사용자 행위에 관한 정보 보관. 좋아요, 답글, 등등
- 횟수 : 좋아요 횟수, 응답 수, 팔로어 수, 팔로잉 수 정보 보관

## 4. 마무리
회사마다 독특한 제약이나 요구조건이 있기 때문에 설계를 진행하고 기술을 선택할 때 그 배경에는 어떤 타협적 결정들(Trade-off)가 있었는지 잘 이해하고 설명할 수 있어야 한다.

### 데이터 베이스 규모 확장
- 수직적 규모 확장 vs 수평적 규모 확장
- SQL vs NoSQL
- 주/부 다중화
- 복제본에 대한 읽기 연산
- 일관성 모델
- 데이터베이스 샤딩

- 웹 계층을 무상태로 운영
- 가능한 한 많은 데이터를 캐시할 방법
- 여러 데이터를 센터를 지원할 방법
- 메세지 큐를 사용하여 컴포넌트 사이의 결합도 낮추기
- 핵심 메트릭에 대한 모니터링 : 트래픽이 몰리는 시간대의 QPS, 사용자가 뉴스 피드를 새로고침 할 때 지연 시간 등

