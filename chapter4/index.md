# 가상 면접 사례로 배우는 대규모 시스템 설계 기초 Study [4장] 처리율 제한 장치의 설계


{{< admonition type=tip title="Note" open=true >}}
팀 내에서 진행하는 Study 정리 입니다.
{{< /admonition >}} 

{{< admonition type=question title="함께 논의하고 싶은 주제" open=true >}}
- 점점 내용이 어려워지는 것 같다...
- 우리 서비스 경우도 사용자 인증을 위해 여러 MSA 서버에서 하나의 인증 서비스를 의존하고 있습니다. 미들웨어를 도입하면 좋을 것 같아요. 이전에 API 게이트 서비스를 개발해두었던 것 같은데 ...
- 우리 서비스에는 어떠한 처리율 제한 장치가 있을까요? (인프라 적으로 모든) 깊게 물어본적이 없는 것 같아 반성해 봅니다...
- 토큰 버킷 알고리즘에서 IP 주소별로 처리율 제한이 필요하면 IP 주소마다 버킷을 하나씩 할당하게 되면 버킷이 엄청 많아질텐데...
- 누출 버킷 알고리즘 단점중에서 윈도 경계 부근에 순간적으로 많은 트래픽이 몰릴 경우 윈도에 할당된 양보다 더 많은 요청이 처리되어 문제라는데 다른 윈도우에 있기때문에 상관없지 않을까합니다 ? 다들 어떻게 이해하셨나요?
{{<figure src="/posts/images/system-design-interview/system-design-figuare-4-9.png#center">}}
{{< /admonition >}}

## 처리율 제한 장치 `rate limiter`
- 클라이언트 또는 서비스가 보내는 트래픽 처리율`rate`을 제어하기 위한 장치
- API 요청 횟수가 제한 장치에 정의된 임계치`threadhold`를 넘어서면 추가로 도달한 모든 호출은 처리가 중단`block`된다.

### API 처리율 제한 장치를 두면 좋은점
- **DoS(Denial of Service)** 공격에 의한 자원 고갈`resource starvation`을 방지
- 비용을 절감 : 과금이 횟수에 따라 이루어진다면, 그 횟수를 제한할 수 있어야 비용을 절감
- 서버의 과부하를 막는다

## 1. 문제 이해 및 설계 범위 확정
- 어떤 종류의 처리율 제한 장치를 설계해야 하는가? 클라이언트 측 혹은 서버 측 제한 장치인가?
- 어떤 기준(IP, 사용자 ID)으로 API 호출을 제어해야 하는가?
- 시스템의 규모는 어떠한가
- 분산 환경에서도 동작하는가
- 독립된 서비스? 애플리케이션 코드에 포함될 수 있는가?
- 사용자에게 제한 장치에 대한 알림을 주어야 하는가

### 1.1 요구사항
- 설정된 처리율을 초과하는 요청은 정확하게 제한해야한다.
- 낮은 응답시간 : 이 처리율 제한 장치는 HTTP 응답시간에 나쁜 영향을 주어서는 안된다.
- 가능한 한 적은 메모리를 사용
- 분산형 처리율 제한 `distributed rate limiting` : 하나의 처리율 제한 장치를 여러 서버나 프로세스에 공유할 수 있어야 한다.
- 예외처리 : 요청이 제한되었을 때 그 사실을 사용자에게 분명하게 보여주어야 한다.
- 높은 결함 감내성 `fault tolerance` : 제한 장치에 장애가 생기더라도 전체 시스템에 영향을 주어서는 안 된다. 

## 2. 개략적 설계안 제시 및 동의 구하기

### 2.1 처리율 제한장치의 위치
- 클라이언트 요청은 쉽게 위변조가 가능하고 구현을 통제하는 것도 어려울 수 있다.
- **처리율 제한 장치를 API 서버에 두는 대신 처리율 제한 미들웨어`middleware`를 만들어 통제한다.**
- 클라우드 마이크로서비스의 경우, API 게이트웨이`gateway`라 불리는 컴포넌트에 보통 구현
- **회사의 현재 기술 스택`technology stack`이나 엔지니어링 인력, 우선순위, 목표에 따라 처리율 제한 장치는 달라질 수 있다.**

{{< admonition type=note title="API 게이트 웨이" open=true >}}
API 게이트웨이는 처리율 제한, SSL 종단`termination`, 사용자 인증`authentication` IP 허용 목록`whitelist` 관리 등을 지원하는 완전 위탁관리형 서비스 `fully managed`이다. 
{{< /admonition >}} 

### 2.2 처리율 제한장치 적용을 위한 지침
- 기술 스택 점검 고려 하기 : 현재 사용하는 프로그래밍 언어가 서버 측 구현을 지원하기 충분할 정도로 효율이 높은지 확인하기
- 알맞은 처리율 제한 알고리즘 선택 하기
- 이미 API 게이트웨이를 사용한다면 게이트웨이에 포함시켜야 할 수도 있다.
- 상용 API 게이트 웨이를 쓰는 것이 바람직 할 수도 

### 2.3 처리율 제한 알고리즘
- 토큰 버킷`token bucket`
- 누출 버킷`leaky bucket`
- 고정 윈도 카운터`fixed window counter`
- 이동 윈도 로그`sliding window log`
- 이동 윈도 카운터`sliding window counter`

#### 2.3.1 토큰 버킷 알고리즘
- 처리율 제한에 폭넓게 이용되고 있음
- 간단하고 알고리즘에 대한 세간의 이해도도 높음 


##### 동작 원리
1. 토큰 버킷은 지정된 용량을 갖는 컨테이너이며 사전에 설정된 양의 토큰 공급기`refiller`에 의해 토큰이 주기적으로 채워진다. 버킷이 가득차면 토큰은 버려진다.`overflow`
2. 요청이 도착하면 버킷에 충분한 토큰이 있는지 검사
  - 충분한 토큰이 있는 경우 : 버킷에서 토큰 하나를 꺼낸 후 요청을 시스템에 전달
  - 충분한 토큰이 없는 경우 : 해당 요청은 버러짐`dropped`
  
##### 인자 
- **버킷 크기** : 버킷에 담을 수 있는 토큰의 최대 개수
- **토큰 공급률`refill rate`**: 초당 몇 개의 토큰이 버킷에 공급되는가 

##### 사용 사례
- 통상적으로 API 엔드포인트마다 별도의 버킷을 둔다.
- IP 주소별로 처리율 제한이 필요하면 IP 주소마다 버킷을 하나씩 할 당해야한다.

##### 장점
- 구현이 쉽다
- 메모리 사용 측면에서 효율적
- 짧은 시간에 트래픽`burst of traffic`도 처리 가능

##### 단점
- 버킷 크기와 토큰 공급률의 인자를 적절하게 튜닝하는 것이 까다로움

##### 토큰 버킷 알고리즘 go 코드
<script src="https://gist.github.com/riley817/669530003558a6fe0de134b1b96c3a06.js"></script>

#### 2.3.2 누출 버킷 알고리즘
- 누출 버킷`leaky bucket` 알고리즘은 토큰 버킷 알고리즘과 비슷하지만 요청 처리율이 고정되어있다는 점에 차이가 있다.
- `FIFO(First-In-First-Out)` 큐로 구현한다.

##### 동작 원리
1. 요청이 도착하면 큐가 가득 차 있는지 본다. 빈자리가 있는 경우 큐에 요청을 추가한다.
2. 큐가 가득 차 있는 경우 새 요청은 버린다.
3. 지정된 시간마다 큐에 요청을 꺼내어 처리한다.

##### 인자
- **버킷 크기** : 큐 사이즈
- **처리율`outflow rate`** : 지정된 시간당 몇 개의 항목을 처리할지 지정하는 값. 초 단위로 표현한다.

##### 장점
- 큐의 크기가 제한되어 있어 메모리 사용량 측면에서 효율적
- 고정된 처리율을 갖고 있기 때문에 안정적 출력`stable outflow rate`이 필요한 경우 적합

##### 단점
- 단 시간에 트래픽이 몰리는 경우 큐에 오래된 요청이 쌓이고 최신 요청들은 버려짐
- 두 개의 인자를 튜닝하기 까다로움 

##### 누출 버킷 알고리즘 Go 코드

#### 2.3.3 고정 윈도 카운터 알고리즘

##### 동작 원리
1. 타임라인을 고정된 간격인 윈도`window`로 나누고 각 윈도마다 카운터`counter`를 붙인다.
2. 요청이 접수될 때마다 이 카운터의 값은 1씩 증가한다.
3. 카운터의 값이 사전에 설정된 임계치`threshold`에 도달하면 새로운 요청은 새 윈도가 열릴때까지 버려진다.

- 시스템은 초당 3개까지 요청을 허용한다.
- 매초마다 열리는 윈도에 3개 이상의 요청이 밀려오면 초과분은 그림 4-8 에 보이는대로 버려진다.
{{<figure src="/posts/images/system-design-interview/system-design-figuare-4-8.png#center">}}

##### 장점
- 메모리 효율이 좋다
- 이해하기 쉽다
- 윈도가 닫히는 시점에 카운터를 초기화 하는 방식은 특정한 트래픽 패턴을 처리하기 적합하다.

##### 단점
- 윈도 경계 부근에 일시적으로 많은 트래픽이 몰려드는 경우 기대했던 시스템의 처리 한도보다 많은 요청의 양을 처리하게 된다.

#### 2.3.4 이동 윈도 로깅 알고리즘
- 고정 윈도 카운터 알고리즘의 단점을 해결하는 알고리즘이다

##### 동작원리
1. 요청의 타임스탬프를 추적한다. 타임스탬프는 레디스, 정렬집합`sorted set`과 같은 캐시에 보관한다.
2. 만료된 타임스탬프는 그 값이 현재 윈도의 시작 시점보다 오래된 타임스탬프를 말한다.
3. 타임스탬프를 로그`log`에 추가한다.
4. 로그의 크기가 허용치 보다 같거나 작으면 요청을 시스템에 전달한다.

##### 예시
아래의 처리율 제한기는 분당 최대 2회의 요청만을 처리하도록 설정되어있다.
{{<figure src="/posts/images/system-design-interview/system-design-figuare-4-10.png#center">}}

1. 요청이 1:00:01에 도착했을 때 로그는 비어 있는 상태이므로 요청이 허용된다.
2. 새로운 요청이 1:00:30에 도착한다. 타임스탬프가 로그에 추가된다. 로그크기는 2이므로 허용 한도보다 크지 않다.
3. 새로운 요청이 1:00:50에 도착한다. 타임스탬프가 로그에 추가된다. 로그크기는 3이므로 허용 한도보다 크다. 따라서 요청은 거부된다.
4. 새로운 요청이 1:01:40에 도착한다. 범위 안에 요청이 1분 윈도 안의 요청이지만 이전의 스탬프들이 전부 만료이므로 로그에서 삭제되면 크기는 2이다. 따라서 신규 요청은 전달된다.

##### 장점
- 정교하다. 허용되는 요청의 개수는 시스템의 처리율 한도를 넘지 않는다.

##### 단점
- 거부된 요청의 타임스탬프도 로그로 남기므로 다량의 메모리를 사용

#### 2.3.5 이동 윈도 카운터 알고리즘
- 고정 윈도 카운터 알고리즘과 이동 윈도 로깅 알고리즘을 결합

##### 예시
- 아래 처리율 제한기는 분당 7개 요청으로 설정
- 이전 1분 동안 5개의 요청이 그리고 현재 1분 동안 3개의 요청이 왔다
{{<figure src="/posts/images/system-design-interview/system-design-figure-4-11.png#center">}}

{{< admonition type=tip title="Note" open=true >}}
- 현재 1분간 요청의 수 + 직전 1분간 요청의 수 * 이동 윈도와 직접 1분이 겹치는 비율
- 3 + 5 * 70% = 6.5 
{{< /admonition >}} 

- 현재 1분의 30% 시점에 도달한 신규 요청은 시스템으로 전달되나 그 직후에는 한도에 도달하였으므로 더 이상 요청은 받을 수 없다.

##### 장점
- 이전 시간대의 평균 처리율에 따라 현재 윈도 상태를 계산하므로 짧은 시간에 몰리는 트래픽도 잘 대응한다.
- 메모리 효율이 좋다.

##### 단점
- 직전 시간대의 요청이 균등하게 분포되어 있다고 가정한 상태에서 추정치를 계산하기 때문에 다소 느슨

#### 2.3.6 개략적인 아키텍처
- 얼마나 많은 요청이 접수되었는지 추적할 수 있는 카운터를 추적 대상별로 두고(사용자 별, IP 별, endpoint 별, 서비스 단위 별)이 카운터의 값이 어떤 한도를 넘어서면 한도를 넘어 도착한 요청은 거부하는 것

- 카운터의 보관 장소는 메모리상 동작하는 캐시가 바람직하다. 
  - 속도가 빠르고 시간에 기반한 만료 정책을 지원하기 때문
  
{{< admonition type=tip title="Redis" open=true >}}
  - 레디스는 처리율 제한 장치를 구현할 때 자주 사용되는 저장장치이다.
  - `INCR` : 메모리에 저장된 카운터의 값을 1 만큼 증가 
  - `EXPIRE` : 카운터에 타임아웃 값을 설정. 시간이 지나면 카운터는 자동으로 삭제됨
{{< /admonition >}} 


##### 동작원리
1. 클라이언트가 처리율 제한 미들웨어`rate limiting middleware`에게 요청을 전달
2. 처리율 제한 미들웨어는 레디스의 지정 버킷에서 카운터를 가져와서 한도 도달여부 검사 
  - 한도에 도달했으면 요청은 거부됨
  - 한도에 도달하지 않았다면 요청은 API 서버로 전달
3. 미들웨어는 카운터의 값을 증가시킨 후 다시 레디스에 저장

## 3. 상세 설계

### 3.1 처리율 제한 규칙
처리율 제한 규칙들은 보통 설정파일 형태로 디스크에 보관된다.

### 3.2 처리율 한도 초과 트래픽의 처리
- 한도 제한에 걸리면 API는 **HTTP 429 응답**`too many requests`를 클라이언트에게 보냄

#### 3.2.1 처리율 제한 장치가 HTTP 응답값
- 클라이언트는 HTTP 응답 헤더를 통해 요청이 처리율 제한에 걸리고 있는지를 감지할 수 있다.
- 사용자가 너무 많은 요청을 보내면 429 too many requests 오류를 X-Ratelimit-Retry-After 헤더와 함께 반환하자.
- `X-Ratelimit-Remaining` : 윈도 내에 남은 처리 가능 요청의 수
- `X-Ratelimit-Limit` : 매 윈도마다 클라이언트가 전송할 수 있는 요청의 수
- `X-Ratelimit-Retry-After` : 한도 제한에 걸리지 않으려면 몇 초 뒤에 요청을 다시 보내야하는지 알림

### 3.3 상세 설계
{{<figure src="/posts/images/system-design-interview/system-design-figure-4-13.png#center">}}

1. 처리율 제한 규칙은 디스크에 보관하며 작업 프로세스`worker`는 수시로 규칙을 디스크에 읽어 캐시에 저장
2. 클라이언트의 요청은 제일 먼저 처리율 제한 미들웨어에 도달
3. 처리율 제한 미들웨어는 제한 규칙을 캐시에서 가져오고 마지막 요청의 타임스탬프는 레디스 캐시에서 가져옴
  - 해당 요청이 처리율 제한에 걸리지 않으면 API
  - 해당 요청이 처리율 제한에 걸리면 429 too many requests 에러를 클라이언트로 보냄

### 3.4 분산 환경에서의 처리율 제한 장치의 구현
다중 서버에서 병렬 스레드를 지원하도록 확장하는 것은 다음과 같은 문제를 해결해야한다.
- 경쟁 조건`race condition`
- 동기화`synchronization`

#### 3.4.1 경쟁조건
- 경쟁 조건을 해결하기 위한 방법은 락`Lock`이 있다. 락은 시스템의 성능을 떨어뜨리는 문제가 있다.
- 락 이외에 루아 스크립트`Lua Sript`와 정렬집합`sorted set`이라 불리는 레디스 자료구조를 사용할 수 있다.

#### 3.4.2 동기화 이슈
- 처리율 제한 장치를 여러 대 두게 되면 동기화가 필요해진다.
- 해결방안으로는 고정 세션을 활용하여 클라이언트로부터의 요청은 항상 같은 처리율 제한 장치로 보내게 한다.
  - 비추천하며, 규모면에서 확장 가능하지도 않고 유연하지 않다.
- 레디스와 같은 중앙 집중형 데이터 저장소를 쓴다.

#### 3.4.3 성능 최적화
1. 여러 데이터 센터를 지원하여 사용자 트래픽을 가장 가까운 에지 서버로 전달하여 지연시간을 줄인다.
2. 제한 장치 간 데이터를 동기화할 때 최종 일관성 모델을 사용

#### 3.4.4 모니터링
모니터링을 통해 확인하려는 것
- 채택된 처리율 제한 알고리즘이 효과적인가
- 정의한 처리율 제한 규칙이 효과적이다.

## 4. 마무리
추가로 더 언급할 사항들

- 경성`hard` 또는 연성`soft` 처리율 제한
  - 경성 처리율 제한 : 요청 개수는 임계치를 절대 넘어설 수 없다.
  - 연성 처리율 제한 : 요청 개수는 잠시 동안은 임계치를 넘어설 수 있다.

- 다양한 계층에서 처리율 제한
  - 앞서 소개한 애플리케이션 계층 말고도 다른 계층에서도 처리율 제한이 가능하다. 
  - 예시로 `Iptables`를 사용하여 네트워크 계층에서 처리율 제한을 적용하는 것이 가능

- 처리율 제한을 회피하기 위한 클라이언트 설계 방법
  - 클라이언트의 캐시를 사용하여 API 호출 횟수를 줄인다.
  - 처리율 제한의 임계치를 이해하고 짧은 시간동안 너무 많은 메세지를 보내지 않는다.
  - 예외나 에러를 처리하는 코드를 도입 클라이언트가 예외적 상황으로부터 우아하게 복구
  - 재시도 로직을 구현할 때는 충분한 백오프 시간을 둔다.



