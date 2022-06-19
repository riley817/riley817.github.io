# Elasticsearch 강의 정리


![elasticsearch](/categories/images/elastic/9SyGqYLhMmYxEJyNUf.png)

## Elasticsearch
+ `Elasticsearch` 는 `Apache Lucene` 기반의 Full-Text 검색엔진 이며 분석엔진 이다.
+ `고가용성(High Availability)` 의 확장 가능한 `오픈 소스`이다.
+ HTTP 웹 인터페이스와 스키마에서 자유로운 JSON 문서로 제공된다.

### 검색 엔진으로서의 Elasticsearch
Elasticsearch 는 루씬 기반의 대중적인 엔터프라이즈 검색엔진으로, Apach 2.0 License 에 의거 오픈 소스로 출시 되었다. 또한 **대부분의 루씬이 제공하는 기능들을 Eleasticsearch 에서도 제공**한다.

### Apache Lucene
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

###  분석 엔진으로서의 Elasticsearch
Elasticsearch 는 검색엔진으로 단독으로 서비스 하지만, 몇 가지 솔루션을 추가하여 분석엔진으로서도 활용이 가능하다.

![image](/categories/images/elastic/beats-platform.png)
### 관련솔루션
#### Kibana
- Elasticsearch 에 존재하는 데이터를 실시간으로 분석 및 시각화.

#### Beats
- 데이터 수집기. 서버 혹은 단말에 Agent 형태로 Elasticsearch 나 Logstash 로 데이터를 전달

#### Logstash
- 직접 로그를 전달하거나 Beats 에서 데이터를 전달받아 파싱 혹은 필터링 하여 Elasticsearch 로 전달
