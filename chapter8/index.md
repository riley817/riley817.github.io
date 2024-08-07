# 가상 면접 사례로 배우는 대규모 시스템 설계 기초 Study [8장] URL 단축기 설계


{{< admonition type=tip title="Note" open=true >}}
팀 내에서 진행하는 Study 정리 입니다.
{{< /admonition >}} 

{{< admonition type=question title="함께 논의하고 싶은 주제" open=true >}}
- 내용이 매우 유익했습니다. 책에 설계된 대로 구현을 해볼 생각입니다!
- 단축 URL을 만들기 위한 접근법 (해시 후 충돌 해소, base-62) 이 외에 다른 접근법이 있을까 서칭과 chatGPT 쿤에게 문의 해보았지만 좋은 방법은 찾지 못하였는데 혹시 다른 아이디어가 있을까요?
{{< /admonition >}}

## 1. 문제 이해 및 설계 범위 확정

### 1.1 기능
1. URL 단축 : 주어진 긴 URL을 짧게 줄인다.
2. URL 리다이렉션 : 축약된 URL로 HTTP 요청이 오면 원래 URL로 안내
3. 높은 가용성 규모 확장성, 그리고 장애 감내가 요구

### 1.2 개략적 추정
- 쓰기 연산 : 매일 1억 개의 단축 URL을 생성
- 초당 쓰기 연산 : 1억(100million)/24/3600 = 1160
- 읽기 연산 : 읽기 연산과 쓰기 연산의 비율을 10:1 로 가정. 그 경우 읽기 연산은 초당 11,600회 발생한다. (1160 * 10 = 11,600)
- 10년간 운영한다고 가정 : 1억(100million) * 365 * 10 = 3,650억(365billion) 개의 레코드를 보관
- 축약 전 URL의 평균 길이는 100
- 10년동안 필요한 저장 용량은 3650억(365billion) * 100 byte = 36.5 TB

## 2. 개략적 설계안 제시 및 동의 구하기
### 2.1 API 엔드 포인트

1. URL 단축 엔드포인트 : 새 단축 URL을 생성하고자 할 때 호출한다. 
```
POST /api/v1/data/shorten

- 인자 : {longUrl:longURLString}
- 반환 : 단축 URL
```


2. URL 리다이렉션용 엔드포인트 : 단축 URL에 대한 원래 URL로 보내주기 위한 용도
```
GET /api/v1/shortUrl

- 반환 : HTTP 리다이렉션 목적지가 될 원래 URL
```
|301 `Permanently Moved`|302 `Found`|
|---|---|
|응답에 대한 HTTP 요청의 처리 책임이 영구적으로 Location 헤더에 반환된 URL로 이전|주어진 URL로 요청이 일시적으로 Location 헤더가 지정하는 URL에 의해 처리되어야 한다.|
|영구적으로 이전되었으므로 브라우저는 이 응답을 캐시함|단축 URL에 먼저 보내진 후 원래 URL로 리다이렉션되어야 함|
|첫 번째 요청만 전송되므로 서버 부하를 줄일 수 있다.|트래픽 분석이 가능하다.|
> - 원래 URL = hashTable.get(단축 URL)
> - 301 또는 302 응답으로 Location 헤더에 원래 URL을 넣은 후 전송

### 2.2 URL 단축
#### 해시 함수의 요구 사항
- 입력으로 주어진 긴 URL이 다른 값이면 해시 값도 달라야한다.
- 계산된 해시 값은 원래 입력으로 주어졌던 긴 URL로 복원될 수 있어야 한다.

## 3. 상세 설계
### 3.1 데이터 모델
- `<단축 URL, 원래 URL>` 순서쌍을 관계형 데이터베이스에 저장
- 단순하게 설계 테이블은 id, shortURL, longURL 세 개의 컬럼으로 구성할 수 있다.

### 3.2 해시 함수
#### 해시 값 길이
- hashValue는 `0-9, a-z, A-Z` 의 문자들로 구성 문자의 개수는 `10 + 26 + 26 = 62`개 이다.
- 길이를 정하기 위해서는 `62^n >= 3,650억(365billion)`의 n의 최소값은 7이다. (`62^7 = 3.5조`)

#### 해시 후 충돌 해소
- CRC32, MD5, SHA-1과 같은 해시함수를 이용하여 처음 7 글자만 사용하기
- 충돌이 해소될 때 까지 데이터베이스를 질의한다. 
- 데이터베이스 대신 블룸 필더를 사용하여 성능을 높일 수 있다.

#### base-62 인코딩
- 진법 변환은 URL 단축기를 구현할 때 흔히 사용되는 접근법
- 62진법을 사용하는 이유는 hashValue에 사용할 수 있는 문자 개수가 62개 이기 때문

#### 두 접근법 비교하기
|해시 후 충돌 해소 전략|base-62 변환|
|---|---|
|단축 URL의 길이가 고정됨|단축 URL의 길이가 가변적. ID 값이 커지면 같이 길어짐|
|유일성이 보장되는 ID 생성기가 필요하지 않음|유일성 보장 ID 생성기가 필요|
|충돌이 가능해서 해소 전략 필요|ID 유일성이 보장된 후에야 적용 가능한 전략이라 충돌 아예 불가능|
|ID로 부터 단축 UR을 계산하는 방식이 아니라 다음에 쓸 수 있는 URL을 알아내는 것이 불가능|ID가 1씩 증가하는 값이라고 가정하면 다음에 쓸 수 있는 단축 URL이 무엇인지 쉽게 알아내어 보안상 문제가 될 소지 있음|

### 3.3 URL 단축키 상세 설계
{{<figure src="/posts/images/system-design-interview/system-design-figure-8-7.png#center">}}

1. 입력으로 긴 URL을 받는다.
2. 데이터베이스에 해당 URL이 있는 지 검사한다.
3. 데이터베이스에 있는 경우 해당 단축 URL을 가져와서 클라이언트에게 반환
4. 데이터베이스에 없는 경우 유일한 ID 생성 - 이 유일한 ID는 데이터베이스 기본 키로 사용
5. 62진법 변환하여 ID를 단축 URL로 변환
6. ID, 단축URL, 원래 URL레코드에 저장 후 단축 URL을 클라이언트에게 반환

### 3.4 URL 리다이렉션 상세 설계
{{<figure src="/posts/images/system-design-interview/system-design-figure-8-8.png#center">}}

1. 사용자가 단축 URL을 클릭
2. 로드 밸런서가 해당 요청을 웹 서버로 전달
3. 단축 URL이 이미 캐시되어 있는 경우 원래 URL을 바로 꺼내어 클라이언트에게 전달
4. 캐시에 해당 단축 URL 없는 경우 데이터베이스에서 가져온다.
  - 없는 경우는 사용자가 잘못된 단축 URL을 입력한 경우
5. 데이터베이스에 꺼낸 URL을 캐시에 넣은 후 사용자에게 반환

## 4. 마무리
설계를 마친 후 다음과 같은 것을 더 생각해보자


**처리율 제한 장치**
- 엄청난 URL 요청이 밀려들 경우 무력화될 수 있다는 잠재적 결함이 있음
- IP 주소를 비롯한 필터링 규칙을 통해 요청을 거름

**웹 서버의 규모 확장**
- 본 설계의 웹 계층은 무상태`stateless` 계층이므로 웹 서버를 자유로이 증설 및 삭제 가능

**데이터베이스의 규모 확장**
- 데이터베이스를 다중화 혹은 샤딩`sharding`하여 규모 확장성을 달성

**데이터 분석 솔루션**
- URL 단축기와 데이터 분석 솔루션을 통합하여 링크에 대한 분석

**가용성, 데이터 일관성, 안정성**
- 대규모 시스템이 성공적으로 운영되기 위해 반드시 갖추어야 할 속성들

