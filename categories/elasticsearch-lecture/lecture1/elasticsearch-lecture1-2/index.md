# 1.2 개념 및 용어 정리 - Shard


# Shard
Shard 란 **인덱스의 데이터를 나누는 단위**이다. 인덱스는 무한한 양의 데이터가 저장이 가능하다. 
그래서 인덱스 데이터가 단일 노드의 *하드웨어의 용량을 초과하여 더이상 데이터를 저장할 수 없거나* 혹은 **CPU, Memory 의 자원 초과로 인덱싱이나 검색의 성능 저하**를 발생시킬 수 있다. 이러한 문제를 해결하기 위하여 인덱스를 수 많은 조각으로 나누어 관리한다.

### 샤딩을 함으로써

- 콘텐츠 볼륨의 **수평(Horizontal) 분할 및 확장**이 가능하다. 관계형 데이터베이스처럼 컬럼별로 나누는 것이 아닌 횡별로 나누어 샤드에 저장.
- 여러 샤드에 분산 배치하여 병렬화 함으로써 성능 및 처리량을 늘릴 수 있다.

### Shard 의 종류
#### Primary Shard
`Primary Shard` 는 Indexing 되어 들어온 Document 의 **원본 Shard** 를 의미한다.

+ Primary Shard 는 각 인덱스 별로 **최소 1개 이상 존재** 해야 한다.
+ ElasticSearch 에 Document 가 인덱싱 될 때 가장 처음에 생성되는 샤드이다.
+ 샤드 개수를 지정하지 않는다면 기본으로 `5 개`로 지정된다.

#### Replica Shard
Network / Cloud 환경의 샤드 노드가 오프라인 상태가 되거나 혹은 사라지게 될 경우를 대비하여 인덱스 샤드에 대해 하나 이상의 복사본을 생성할 수 있다. 이를 `Replica Shard` 라고 한다.

+ ElasticSearch 에서 Primary Shard 가 인덱싱 된 후, Primary Shard 가 저장된 데이터 노드와는 다른 곳에 복제된다.
+ Replica Shard 에도 넘버링을 하며, 어떤 Primary Shard 의 복제본인지 식별이 가능하다.
+ replica 의 `기본값은 1`
+ 인덱싱 시 Primary Shard 의 복제를 하는 과정이 추가된다.

### 샤딩의 단점
+ I/O 가 두배로 발생하여 인덱싱 성능이 저하된다.
+ 디스크 볼륨 또한 실제 도큐먼트 용량의 두배가 필요하다.

![shard](/categories/images/eshaed.png)
[그림 1] ES 플러그인 인 Head 로 각 노드에 샤드 dashboard를 확인 할 수 있다. ( 진한테두리가 Primary Shard )
<p class="image-area"></p>

>1. Elasticsearch 의 샤드는` Lucene 의 인덱스` 이며, 단일 Lucene 인덱스가 포함할 수 있는 도큐먼트의 최대 개수는 **2,147,483,519** 개 이다
>2. Replica Shard 가 있기 때문에 샤드/노드 오류가 발생하더라도 Elasticsearch 클러스터의 고가용성이 유지된다.
>3. 모든 Replica Shard 에서 병렬 방식으로 검색을 실행할 수 있으므로 검색 처리량 확장 가능하다.

## 참고
+ [Elasticsearch Reference](https://www.elastic.co/guide/en/elasticsearch/guide/current/index.html)
