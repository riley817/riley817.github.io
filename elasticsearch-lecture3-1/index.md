# [Elasticsearch 검색 엔진 구축 강의] 3.1 Elasticsearch 구성하기


Elasticsearch 는 정적으로 설정을 구성할 수 있을 뿐만 아니라 클러스터 운영중에도 클러스터 세팅 업데이트 API 를 통하여 동적인 설정 구성이 가능하다.

## Static settings
Elasticsearch는 노드별로 설정파일을 구성할 수 있다. Elasticsearch 에는 세 개의 구성 파일이 있으며, 이 설정 파일들의 위치는 아카이브 배포판 설치시에는 `$ES_HOME/config`, 패키지 배포시에는 (RPM 설치 등) `/etc/elasticsearch` 에 위치한다.

- `elasticsearch.yml` : Elasticsearch 의 핵심 설정
- `jvm.options` : JVM 옵션 설정 ( heapsize 설정 )
- `log4j2.properties` : Elasticsearch 의 logging 설정 

## Dynamic settings
클러스터에 REST API 로 호출하여 클러스터를 운영중에도 구성 설정을 변경 할 수 있다.

## 참고
+ https://www.elastic.co/guide/en/elasticsearch/reference/current/settings.html#settings


