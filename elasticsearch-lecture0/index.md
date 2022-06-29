# Elasticsearch 시작하기


## 1. Elasticsearch
+ `Elasticsearch` 는 `Apache Lucene` 기반의 Full-Text 검색엔진 이며 분석엔진 이다.
+ `고가용성(High Availability)` 의 확장 가능한 `오픈 소스`이다.
+ HTTP 웹 인터페이스와 스키마에서 자유로운 JSON 문서로 제공된다.

### 1.1 검색 엔진으로서의 Elasticsearch
Elasticsearch 는 루씬 기반의 대중적인 엔터프라이즈 검색엔진으로, Apach 2.0 License 에 의거 오픈 소스로 출시 되었다. 또한 **대부분의 루씬이 제공하는 기능들을 Eleasticsearch 에서도 제공**한다. `HTTP Web Interface`와 `Schema`에 자유로운 **JSON** 형태의 도규먼트를 지원하는 준 실시간 분산형 검색엔진이다.

#### Apache Lucene
* `정보검색(Information Retrieval, IR)` 소프트웨어 검색 라이브러리
* Apahce Software 재단의 검색엔진 상위 프로젝트
* **자바 언어로 개발**되어 있다.
* 오픈소스
* 주요기능
    - 사용자 위치 정보 이용 가능
    - 다국어 검색 지원
    - 자동 완성 지원
    - 미리 보기 지원
    - 철자 수정 기능 지원

### 1.2 분석 엔진으로서의 Elasticsearch
Elasticsearch 는 검색엔진으로 단독으로 서비스 하지만, 몇 가지 솔루션을 추가하여 분석엔진으로서도 활용이 가능하다.

#### 관련 솔루션

{{< figure src="/posts/images/elastic/beats-platform.png" >}}
##### Kibana
- Elasticsearch로 수집된 데이터를 통계 및 집계하여 시각화

##### Beats
- 데이터 수집기. 서버 혹은 단말에 Agent 형태로 로그나 데이터 원본을 Elasticsearch로 전달

##### Logstash
- 직접 로그를 전달하거나 Beats 에서 데이터를 전달받아 파싱 혹은 필터링 하여 Elasticsearch 로 전달

## 2. Elasticsearch의 용어 및 개념 정리

### Document
+ 도큐먼트는 `JSON (Javascript Object Notation)` 형태의 Elasticsearch 의 기본 저장단위 이다.
+ 관계형 데이터 베이스의 `Row` 와 비슷한 개념으로 볼 수 있다.
+ 도큐먼트는 데이터에 적재될 때 `Document ID` 를 갖는다.
+ **Document ID 는 지정하지 않으면 랜덤하게 생성** 되며, 사용자가 정의한 값으로도 생성 가능하다.
+ Document ID 는 데이터를 찾아가는 Metadata 로 볼 수 있다.

### Index
+ Index 는 비슷한 형질을 가지는 도큐먼트 간의 집합이다.
+ 관계형 데이터 베이스의 `Database` 와 비슷한 개념으로 볼 수 있다.
+ 여러 도큐멘트들이 하나의 인덱스에 적재된다.
+ 인덱스 이름은 문서에 대한 인덱싱/검색/갱신/삭제 등을 수행할 때 참조값으로 사용된다.
+ 인덱스는 사전에 정의되어야 할 데이터 타입이나, 특정한 구조가 필요하지 않다면 최초 데이터가 인입될 때 생성 된다.
+ ex) 고객정보, 제품카탈로그, 주문정보 등...

### Type
+ 관계형 데이터 베이스의 `Table` 과 비슷한 개념으로 볼 수 있다.
+ 타입은 인덱스의 파티션으로 사용 된다.
+ 하나의 인덱스에 도큐먼트를 저장할때 타입을 분리해서 인덱싱이 가능하다.

