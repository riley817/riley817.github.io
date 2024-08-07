# 가상 면접 사례로 배우는 대규모 시스템 설계 기초 Study [9장] 웹 크롤러 설계


{{< admonition type=tip title="Note" open=true >}}
팀 내에서 진행하는 Study 정리 입니다.
{{< /admonition >}} 

{{< admonition type=question title="함께 논의하고 싶은 주제" open=true >}}
- 예의있는 크롤러, 스파이더, 스파이더 덫 등 용어들이 재밌는 것 같습니다.
- 최근에는 리액트, 뷰와 같은 CSR(Client Side Rendering) 웹 페이지들이 많을 것 같은데 그런것들은 클라이언트에서 동적으로 렌더링을 완료한 뒤 웹 페이지를 다운받는 형태일까요? 그럼 고전적인 방식의 HTML 처리할 때보다 훨씬 오래 걸릴 것 같습니다.
- 크롤러는 조금 어려울 것 같지만 책에 설계된 구조대로 한번 실습을 해봐야겠습니다!
{{< /admonition >}}

## 웹 크롤러 `web crawler`
- 웹 크롤러는 로봇(`robot`) 또는 스파이더(`spider`)라고도 부른다.
- 검색 엔진에서 널리 쓰는 기술로, 웹에 새로 올라오거나 갱신된 콘텐츠를 찾아내는 것이 주된 목적이다.

### 웹 크롤러의 용례
- **검색 엔진 인덱싱 `search engine indexing`** : 가장 보편적인 용례. 크롤러는 웹 페이지를 모아 검색 엔진을 위한 로컬 인덱스를 만든다.
- **웹 아카이빙 `web archiving`** : 나중에 사용할 목적으로 장기 보관하기 위해 웹에 정보를 모으는 절차
- **웹 마이닝 `web mining`** : 웹 마이닝을 통해 인터넷의 유용한 지식을 도출해 낼 수 있다.
- **웹 모니터링 `web monitoring`** : 크롤러를 사용하면 인터넷에서 저작권이나 상표권이 침해되는 사례를 모니터링 할 수 있다.

> 웹 크롤러의 복잡도는 웹 크롤러가 처리해야하는 데이터 규모에 따라 달라진다. 설계를 위해 웹 크롤러가 감당해야 하는 데이터의 규모와 기능들을 알아내야 한다.

## 1. 문제 이해 및 설계 범위 확정
### 1.1 웹 크롤러의 알고리즘
1. URL 집합 입력이 주어지면 해당 URL들이 가리키는 모든 웹 페이지를 다운로드 한다.
2. 다운받은 웹 페이지에서 URL들을 추출한다.
3. 추출된 URL들을 다운로드할 URL 목록에 추가하고 위의 과정을 처음부터 반복한다.

#### 요구사항 파악하기
- 크롤러의 용도 : 검색 엔진 인덱스 생성
- 매달 10억개(1billion)의 웹 페이지를 수집
- 새로 만들어진 웹페이지나 수정된 웹 페이지도 고려해야 함
- 수집한 웹 페이지는 5년간 저장
- 중복된 콘텐츠는 무시

#### 웹 크롤러가 만족시켜야 할 속성
- **규모 확장성** : 병행성(`parallelism`)을 활용하여 수십억 개의 페이지를 보다 효과적으로 웹 크롤링이 가능
- **안정성(robustness)** : 비정상적인 입력과 환경에 잘 대응(잘못 작성된 HTML, 아무 반응 없는 서버, 장애, 악성 코드가 붙어 있는 링크 등)
- **예절(politeness)** : 수집 대상 웹 사이트에 짧은 시간 동안 너무 많은 요청을 보내서는 안 된다.
- **확장성(extensibility)** : 새로운 형태의 콘텐츠를 지원하기가 쉬워야 한다.

### 1.2 개략적 규모 추정
> - QPS=10억(1,000,000,000) / 30일 / 24시간 / 3600초 = 대략 400페이지/초
> - 최대(peak) QPS = 2 * QPS = 800
> - 웹 페이지의 평균은 500k 라고 가정
> - 10억 페이지 * 500k = 500TB/월
> - 5년간 보관 : 500TB * 12 * 5 = 30PB

### 1.3 개략적 설계안 제시 및 동의 구하기
{{<figure src="/posts/images/system-design-interview/system-design-figure-9-2.png#center">}}

#### Seed URLs
- 시작 URL 집합은 웹 크롤러가 크롤링을 시작하는 출발점
- 크롤러가 가능한 많은 링크를 탐색할 수 있는 URL을 선택해야 한다.
- 일반적으로 전체 URL 공간을 작은 부분 집합으로 나누는 전략을 사용. (지역적 특색, 나라별 인기 웹사이트 등)
- 주제별로 다른 시작 URL을 사용

#### URL Frontier
- 웹 크롤러는 크롤링 상태를 1. 다운로드할 URL 2. 다운로드된 URL로 나눈다.
- 다운로드할 URL을 미수집 URL 저장소(URL frontier)라고 함
- FIFO 큐와 비슷

#### HTML Downloader
- 인터넷에서 웹 페이지를 다운로드하는 컴포넌트

#### DNS Resolver
- 웹 페이지를 다운받으려면 URL을 IP 주소로 변환하는 절차 필요
- DNS Resolver를 사용하여 URL 대응되는 IP 주소를 알아낸다

#### Content Parser
- 웹 페이지를 다운로드하면 파싱과 검증 절차를 거쳐야 한다.
- 이상한 웹 페이지는 문제를 일으킬 수 있다.
- 크롤링 서버 안에 콘텐츠 파서를 구현하면 크롤링 과정이 느려지게 될 수 있으므로 독립적 컴포넌트로 만든다.

#### Content Seen?
- 29% 웹 페이지 콘텐츠는 중복이다.
- 자료 구조를 도입하여 데이터 중복을 줄이고 데이터 처리에 소요되는 시간을 줄인다.
- 모든 문서의 내용을 비교할 수 없으므로 웹 페이지의 해시 값을 비교할 수 있다.

#### 콘텐츠 저장소
- HTML 문서를 보관하는 시스템
- 저장할 데이터 유형, 크기, 저장소 접근 빈도, 데이터의 유효 기간 등을 종합적으로 고려
- 디스크와 메모리를 동시에 사용할 저장소를 택한다.
  - 데이터 양이 너무 많으므로 대부분의 콘텐츠는 디스크에 저장
  - 인기 있는 콘텐츠는 메모리에 두어 접근 지연시간을 줄인다.

#### Link Extractor
- HTML 페이지를 파싱하여 링크들을 골라내는 역할을 한다.
- 상대 경로는 전부 절대 경로로 변환

#### URL Filter
- 특정한 콘텐츠 타입이나 파일 확장자를 갖는 URL, 접속 시 오류가 발생하는 URL, 접근 제외 목록에 포함된 URL은 크롤링 대상에서 배제

#### URL Seen? (이미 방문한 URL)
- 이미 방문한 적 있는 URL인지 추적하면 같은 URL을 여러번 처리하는 일을 방지
- 서버 부하를 줄이고 시스템이 무한 루프에 빠지는 일을 방지
- 블룸 필터나 해시 테이블과 같은 자료구조 사용

#### URL Storage
- URL 저장소는 이미 방문한 URL을 보관하는 장소다

### 1.4 웹 크롤러의 작업 흐름
{{<figure src="/posts/images/system-design-interview/system-design-figure-9-4.png#center">}}
1. 시작 URL들을 미수집 URL 저장소에 저장
2. HTML 다운로더는 미수집 URL 저장소에서 URL 목록을 가져옴
3. HTML 다운로더는 도메인 이름 변환기를 사용하여 URL의 IP 주소를 알아내고 해당 IP 주소에 접속하여 웹 페이지를 다운
4. 콘텐츠 파서는 다운된 HTML 페이지를 파싱하여 올바른 형식을 갖춘 페이지인지 검증
5. 콘텐츠 파싱과 검증이 끝나면 중복 콘텐츠인지 확인하는 절차를 개시
6. 중복 콘텐츠인지 확인하기 위해 해당 페이지가 이미 저장소에 있는지 본다
  - 이미 저장소에 있는 콘텐츠인 경우 처리하지 않고 버린다.
  - 저장소에 없는 콘텐츠인 경우 저장소에 저장한 뒤 URL 추출기로 전달
7. URL 추출기는 해당 HTML 페이지에서 링크를 골라낸다
8. 골라낸 링크를 URL 필터로 전달
9. 필터링이 끝나고 남은 URL만 중복 URL 판별 단계로 전달
10. 이미 처리한 URL 인지 확인하기 위해 URL 저장소에 보관된 URL인지 살핀다. 이미 저장소에 있는 URL은 버린다.
11. 저장소에 없는 URL은 URL 저장소에 저장할 뿐 아니라 미수집 URL 저장소에도 전달

## 3. 상세 설계
### 3.1 중요한 컴포넌트와 구현 기술
- DFS(Depth-First Search) vs BFS(Breadth-First Search)
- 미수집 URL 저장소
- HTML 다운로더
- 안정성 확보 전략
- 확장성 확보 전략
- 문제 있는 콘텐츠 감지 및 회피 전략

### 3.2 DFS vs BFS
- 웹은 유향 그래프(`directed graph`)
- 페이지는 노드, 하이퍼링크는 에지라고 보면 된다.
- 크롤링 프로세스는 유향 그래프를 에지를 따라 탐색하는 과정
- 깊이 우선 탐색법(`depth-first search`)는 좋은 선택이 아닐 수도 있다. 그래프 크기가 클 수록 어느정도 깊숙이가게 될지 가늠하기 어렵다
- 웹 크롤러는 보통 **너비 우선 탐색법(`breadth-first seach`)**을 사용한다. BFS는 큐를 사용하는 알고리즘이며 큐의 한쪽으로는 탐색할 URL을 집어 넣고 다른 한쪽으로는 꺼내기만 한다.

#### BFS 구현의 문제점

**하나의 웹페이지는 대부분 내부 링크(즉 동일한 서버에 다른 페이지를 참조하)가 상당수이다.**
- 크롤러는 같은 호스트에 속한 많은 링크를 다운받게 되어 병렬로 처리한다면 보통 예의없는 크롤러로 간주된다.

**표준적 BFS 알고리즘은 URL간에 우선순위를 두지 않는다.**
- 모든 웹 페이지이가 같은 수준의 품질, 같은 수준의 중요성을 갖지 않는다.
- 페이지 순위, 사용자 트래픽 양, 업데이트 빈도 등 여러가지 척도에 비추어 처리 우선순위를 구별해야 한다.

### 3.3 미수집 URL 저장소
- 미수집 URL 저장소를 활용하여 예의를 갖춘 크롤러, URL 사이의 우선순위와 신선도를 구별하는 크롤러를 구현할 수 있다.

#### 예의
- 수집 대상 서버로 많은 요청을 보내는 것은 무례한 일이며 때로는 `DoS(Denial-of-Service)` 공격으로 간주
- 동일 웹 사이트에 대해서는 한 번에 한 페이지만 요청
- 각 다운로드 스레드는 별도 FIFO 큐를 가지고 있어 해당 큐에서 꺼낸 URL만 다운로드 한다.

||설명|
|---|---|
|큐 라우터|같은 호스트에 속한 URL은 언제나 같은 큐로 가도록 보장|
|매핑 테이블|호스트 이름과 큐 사이의 관계를 보관하는 테이블|
|FIFO 큐|같은 호스트에 속한 URL은 언제나 같은 큐에 보관|
|큐 선택기|큐 선택기는 큐들을 순회하면서 큐에서 URL을 꺼내서 해당 큐에 나온 URL을 다운로드 하도록 지정된 작업 스레드에 전달하는 역할|
|작업 스레드|전달된 URL을 다운로드하는 작업을 수행. 전달된 URL은 순차적으로 처리되며 작업들 사이에는 일정한 지연시간을 둘 수 있다.|

#### 우선순위
- 유용성에 따라 URL 우선 순위를 나눌 때 페이지랭크, 트래픽 양, 갱신 빈도 등 다양한 척도 사용
- **순위결정장치 (prioritizer)** 는 URL 우선순위를 정하는 컴포넌트


{{<figure src="/posts/images/system-design-interview/system-design-figure-9-8.png#center">}}
- **순위결정장치 (prioritizer)** : URL을 입력받아 우선순위 계산
- **큐** : 우선순위별로 큐가 하나씩 할당. 우선순위가 높으면 선택 될 확률도 높음
- **큐 선택기** : 순위가 높은 큐에서 더 자주 꺼내도록 프로그램
- **전면 큐** : 우선순위 결정 과정을 처리
- **후면 큐** : 크롤러가 예의 바르게 동작하도록 보장

#### 신선도
- 데이터의 신선함을 유지하기 위해서는 이미 다운로드한 페이지라고 해도 주기적으로 재수집(recrawl)할 필요가 있다.

**재수집의 최적화 전략**
- 웹 페이지의 변경 이력 활용
- 우선순위를 활용하여 중요한 페이지는 좀 더 자주 재수집

#### 미수집 URL 저장소를 위한 지속성 저장장치
- 대부분의 URL은 디스크에 두지만 IO 비용을 줄이기 위해 메모리 버퍼에 큐를 두는 것
- 버퍼에 있는 데이터는 주기적으로 디스크에 기록

### 3.4 HTML 다운로더
- HTTP 프로토콜을 통해 웹 페이지를 내려 받는다

#### Robots.txt
- 로봇 제외 프로토콜(`Robot Exclusion Protocol`). 웹사이트와 크롤러가 소통하는 표준적 방법
- 크롤러가 수집해도 되는 페이지 목록이 들어 있다.
- Robots.txt 파일을 거푸 다운로드하는 것을 피하기 위해 이 파일은 주기적으로 다시 다운받아 캐시에 보관

#### 성능 최적화
1. 분산 크롤링
- 크롤링 작업을 위해 서버를 분산하는 기법
- 각 서버는 여러 스레드를 돌려 다운로드 작업을 처리
- URL 공간은 작은 단위로 분할하여 각 서버는 그중 일부의 다운로드를 담당

2. 도메인 이름 변환 결과 캐시
- DNS Resolver는 크롤러의 성능의 병목 중 하나 - DNS 요청을 보내고 결과를 받는 작업의 동기적 특성 때문
- DNS 요청 처리되는 데 보통 10ms ~ 200ms 소요되며 작업진행 동안 DNS 요청은 전부 블록 된다.
- 크론 잡을 통하여 도메인 이름과 IP 주소 사이의 관계를 캐시에 보관하여 갱신

3. 지역성
- 서버를 지역별로 분산
- 지역성을 활용하는 전략은 크롤서버, 캐시, 큐, 저장소 등 대부분 컴포넌트에 적용 가능

4. 짧은 타임아웃
- 대기 시간이 길어지면 좋지 않으므로 최대 얼마를 기다릴지 미리 정해두는 것
- 대기 시간까지 서버가 응답하지 않으면 크롤러는 다음 페이지로 넘어간다

#### 안정성
- 안정성을 위한 접근법
- **안정 해시** : 다운로드 서버들에 부하를 분산할 때 적용. 이 기술을 통해 서버를 쉽게 추가하고 삭제
- **크롤링 상태 및 수집 데이터 저장** : 장애가 발생한 경우에도 쉽게 복구할 수 있도록 크롤링 상태와 수집된 데이터를 지속적 저장장치에 저장
- **예외 처리** : 예외가 발생해도 전체 시스템이 중단되는 일 없이 그 작업을 우아하게 이어나갈 수 있어야 함
- **데이터 검증** : 시스템 오류를 방지하기 위한 중요 수단 가운데 하나

#### 확장성
- 새로운 모듈을 끼워 넣음으로써 새로운 형태의 콘텐츠를 지원할 수 있도록 했다.
{{<figure src="/posts/images/system-design-interview/system-design-figure-9-10.png#center">}}

#### 문제 있는 콘텐츠 감지 및 회피
1. 중복 콘텐츠
- 30% 가량은 중복이다 해시나 체크섬(checksum)을 사용하면 중복 콘텐츠를 보다 쉽게 탐지

2. 거미 덫
- 거미 덫(Spider trap)은 크롤러를 무한 루프에 빠뜨리도록 설계한 웹 페이지이다.
- 모든 종류의 덫을 피할 만능 해결책은 없다.
- 수작업으로 덫을 확인하고 찾아낸 후 덫이 있는 사이트를 크롤러 탐색 대상에 제외하거나 URL 필터 목록에 걸어둔다

3. 데이터 노이드
- 어떤 콘텐츠는 거의 가치가 없다. (광고, 스크립트 코드, 스팸 URL 등)
- 이런 콘텐츠는 크롤러에게 도움될 것이 없으므로 가능하다면 제외

## 4. 마무리
- **서버 측 렌더링** : 웹 페이지를 그냥 있는 그대로 다운받아 파싱할 경우 자바스크립트, AJAX 등 동적으로 생성되는 링크는 발경할 수 없다. 페이지를 파싱하기 전에 서버 측 렌더링(동적 렌더링)을 적용하면 해결할 수 있다.
- **원치 않는 페이지 필터링** : 스팸 방지 컴포넌트를 두어 품질이 조악하거나 스팸성인 페이지를 걸러내도록 해 두면 좋다.
- **데이터베이스 다중화 및 샤딩** : 다중화나 샤딩 같은 기법을 적용하면 데이터 계층의 가용성, 규모 확장성, 안전성이 향상
- **수평적 규모 확장성** : 수평적 규모 확장성을 달성하는 데 중요한 것은 서버가 상태정보를 유지하지 않도록 무상태 서버로 만드는 것이다.
- **가용성, 일관성, 안정성** 
- **데이터 분석 솔루션** : 데이터를 수집하고 분석하는 것은 어느 시스템에 중요하다.




 

