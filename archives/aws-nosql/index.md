# 

author::riley
title:: 대규모 서비스를 위한 AWS의 대표적인 NoSQL Database 서비스 알아보기
date::2022-01-20 10:12:47
tags:: #AWS #NoSQL #데이터베이스

## 모던 애플리케이션과 데이터베이스
- db-engines.com 
- 인기순이 top 10 NoSQL 
- JSON NoSQL데이터베이스의 확장성
- 트랜잭션과 보조 인덱스를 흡수하여 단점을 제거 하고 있다 - 릴레이션쉽
- 오픈 소스 데이터베이스를 더 많이 사용하고 있다.

### 모던 애플리케이션
- 사용자 글로벌, 
- 다양한 디바이스와 데이터양이 많아졌다.
- 빠른 성능요구
- 빠르게, 무제한으로 확장이 가능
- Amazon Prime Day 

### 마이크로 서비스 아키텍처
- 하나의 모노리틱 서비스가  -> 마이크로 서비스 구조에서는 각 데이터베이스
- DevOps 형태로 각 마이크로 서비스 별로 2피자 팀구성
-

각 데이터베스별로 잘하는 것과 잘 못하는 것이 있다. -> 내 워크로드에 맞는 ㅇ
-> 확장성, 성능 , 가용성 


SQL or NoSQL

- SQL & NoSQL
- 내 서비스에 워크로드에 맞는 데이터베이스를 골라 사용해야 한다.
- 최근 모던 애플케이션은 혼용하여 사용


대용량 데이터 처리
- SQL 튜닝
- 메인디비 scale up
- read replica로 분산
* cache layer - redis -> 동일한 결과 값이 나오는 경우
* cahce layer cluster 
* shard node -> 여전히 어려움. 운영 어려움

 DynamoDB 인프라 갖고 있고 이를 관리하는 것은 간단
PUT, GET QUERY 명령어를 사용해서 세부적인


서비스 
- Amazon DynamoDB, Amazon ElasticCache Redis

Redis
Blazing fast
다양한 자료구조

읽기 부하 분산을 위한 캐시
- 고정값을 중심으로 히트률이 높은 데이터를 인메모리 기반에 Redis 기반에 두고 사용
- string redis사용할 수 있다.

세션스토어
- 저절한 TTL 시간과 함께 Redis 사용
- 별도의 세션스토에 Stateless한 상태로 만들어야 사용자 경험에는 변함이 없다
- hash Redis
-

리더보드
- 실시간 정보
- Redis Sorted Set 쉽게 구현

지리정보 앱
- 특정 거리 안에 있는 오브젝트 간 거리를 구할 수 있다.
- Redis Geo

