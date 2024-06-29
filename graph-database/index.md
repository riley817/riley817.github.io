# Graph Database 리서치 - Follow, Tag 기능을 고도화 해보자! (1)


## 개요
현재 회사에서는 SNS 서비스를 개발하고 있다. 인스타그램 처럼 특정 사용자를 팔로우 할 수 있는 기능이 있는데 관계형 데이터베이스인 `postgresql` 데이터베이스로 유저 간 팔로우 관계를 관리하고 있다.

유저에게 알 수도 있는 친구를 추천하기 위한 기능이 구현하게 되었는데 팔로우 관계를 나타내는 테이블이 하나로 구성되어 있다 보니 유저의 팔로워, 팔로잉 조회를 위해 `with` 절, `union` 등과 같은 문법을 사용했고 나의 팔로우의 팔로우 중 나와 팔로우 관계가 없는 친구만 불러오기 위해 복잡하게 서브 쿼리로 작성하게 되었다.

{{<image src="/posts/images/dbms/graph_database.png" caption="뇌절 쿼리...⚡️">}}

ORM을 사용하고 있는데 도저히 저 쿼리는 ORM으로 작성할 엄두가 나지 않아 네이티브로 SQL을 직접 작성하여 구현하게 되었다. 그러다보니 결과가 제대로 나오지 않을 때 어떠한 문제가 있는지 이슈 파악이 쉽지가 않았다. 이런 이슈를 개선할 필요가 있었고 추가 요구사항이 발생했을 때 어떻게 더 쿼리를 확장시켜 나갈지 감이 오지 않았다. 마침 AWS 자격증 스터디를 하면서 AWS 서비스 중 `AWS Neptue` 사용 사례에서 소셜 네트워크 관계망 서비스에 활용할 수 있다고 보았고 현재 시스템에 적용해 보면 좋을 것 같다는 생각이 들었다.

## Graph Database
그래프 데이터베이스는 데이터 관계를 그래프를 사용하여 저장, 탐색, 질의하는 그래프 기반의  **NoSQL** 타입의 데이터베이스이다. 엔티티와 엔티티간 관계를 그래프(노드, 에지)로 나타내고 저장한다. 그래프 데이터베이스는 소셜 네트워크, 추천 시스템, 이상 감지 시스템과 같은 대규모로 연결된 데이터를 저장하고 질의하는데 유용하다. 가장 많이 사용 되고 있는 그래프 데이터 베이스로는 `Neo4J`, `OrientDB`, `ArangoDB`가 있다.

### Graph Database 예시
그래프 데이터베이스는 데이터를 `node`로 표현한다. 노드에는 `key-value` 데이터를 통해 노드의 속성`property`으로 사용할 수 있다. `node` 는 하나 이상의 `label`을 가질 수 있으며 `label`에는 `node`의 유형을 정의할 수 있다. 노드와 다른 노드간의 관계는 아래 화살표 처럼 `edge`를 통해 연결 할 수 있다.

{{<image src="/posts/images/dbms/graph_database_node_relation.png">}}

관계형 데이터베이스로 비교하면 아래와 같다.

|관계형 데이터베이스|Graph DB|
|---|---|
|Table|Label   |
|Row   |Node   |
|Column / Data|Property / Value|


### Graph Database 사용 사례

1. **Social networks :** 소셜 네트워크의 사용자 프로필, 피드 활동, 팔로우 관계와 같이 서로 연결된 대용량 데이터를 저장하고 질의하는데 적합하다.
2. **Digital Asset Management :** Netflix 에서는 사용자가 이미 시청하였거나, 시청자연령에 따라 컨텐츠(asset) 접근 관리를 그래프 데이터베이스를 통해 관리하고 있다.
3. **추천 시스템** : 사용자의 관심사, 구매 이력 등 개인화를 통한 추천 데이터를 저장하는데 사용할 수 있다.
4. **이상 감지** : 금융 거래, 계정 정보, IP 등의 데이터 관계를 분석하여 이상 활동 분석 및 패턴 식별을 하는데 사용할 수 있다.
5. 그 외  **네트워크 분석, Master Data Management, Semantic Search 활용 할 수 있다.**
    - https://www.profium.com/en/blog/graph-database-use-cases/

### AWS Neptune vs Neo4j
AWS Neptune과 Neo4j 둘다 인기있는 그래프 데이터 베이스지만 사용 사례에 따라 각각 다른 장점을 가지고 있다.

#### AWS Neptune

- AWS 에서 제공되는 **완전관리형 그래프 데이터베이스 서비스**
- AWS 에서 서비스 제공을 위한 모든 인프라를 제공 하므로 설치, 모니터링, 이벤트 알림, 데이터 백업 등 배포 및 관리가 쉽고 편리하다.
- `Gremlin` 와 `SPARQL` 등의 query language 를 지원
- Neptune는 `property graph` 모델을 지원하며 `Gremlin` query language는 `property grapsh`를 빠르게 순회하는 방법을 제공하며 그 외 `SPARQL`언어를 지원한다.

#### Neo4j
- 오픈 소스이며 `on-premises` 또는 클라우드로 서비스 할 수 있다.
- 좀 더 유연하며 사용자 정의 옵션을 제공하고 `Cypher` query 언어를 지원한다.
- 커뮤니티가 활성화 되어있으며 다양한 third-party 플러그인을 통합하여 사용할 수 있다.

### 사용 사례에 따른 Database 선택

- `AWS Neptune` : 이미 서비스에서 AWS 사용하는 경우, `pay-as-you-go` 가격 모델을 통한 이점을 얻을 수 있다. 그리고 `Gremlin` 쿼리 언어를 사용하는게 더 낫다면  `Neptune` 사용을 추천한다.
- `Neo4j` : 좀 더 유연하고 사용자 정의 기능이 필요하거나 특정 클라우드 서비스에 `locked-in` 되는 것을 원하지 않을 경우, `Cyper` 쿼리 언어를 사용하는게 더 나은 경우 그리고 오픈 소스 커뮤니티를 통한 정보 공유 및 확장 플러그인이 필요한 경우에 추천한다.

## 참조
- [Graph database use cases (10 examples)](https://www.profium.com/en/blog/graph-database-use-cases/)
- [AWS NEPTUNE 적용기](https://jiwonny.github.io/projects/aws-neptune-1/#amazon-neptune)

