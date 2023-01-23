# [TIL & Issue Note] 20220307


# 블록체인과 중앙집중 시스템과의 차이점 (1)

## 1. 중앙화 시스템과 분산 시스템
### 분산 시스템의 정의
- 각각의 꼭지점 = 하나의 서버
- 모드 꼭짓점 = 노드
- **직접적으로 연결된 노드** = 피어

{{< figure src="/posts/images/TIL/20220308110403.png#center" >}}

### 중앙집중 시스템과 분산시스템
#### 중앙집중 시스템
- 대표적인 예시 은행 -> 은행이 운영하는 웹 서버에 연결
- 은행 이용자가 모두 동일한 서버를 이용
- 사용자의 수와 상관없이 사용자가 사용하는 시스템은 동일

##### 특징
- 설계와 운영이 간편한 중앙집중 시스템 -> 기능 추가 시 별도의 절차 없이 모든 사용자에게 적용.
- 서버에 이상이 생길 경우 모든 사용자가 이용 불가


#### 분산 시스템
- 여러 개의 서버가 일을 나누어 처리
- 여러 서버가 일을 분담하는 분산 시스템

##### 분산시스템의 장점
- 일의 효울성이 높다.
- 처리 속도가 신속함
- 중앙시스템이 고장이 나면 모든 서비스가 중단되지만 분산 시스템은 그렇지 않다.

##### 중앙시스템과 비교
- 중앙시스템의 경우 하나의 서버 공격으로 자원 탈취나 서비스 마비 가능성이 높다 -> 외부 공격에 매우 취약
- 여러 개의 서버가 마비 된 서버를 대신하여 서비스를 처리 -> 일이 많아 분산될 수록 해커의 공격으로 안전

##### 분산시스템의 단점
- 단일 서버에 비해 구성 비용이 상당하다.
- 특정 순간에 데이터 간의 내용이 상이하다.
- 여러 개의 서버의 저장 데이터를 동기화 하는 과정이 필요함 -> 상이한 내용이 같아지도록 데이터를 복사하는 과정 필요

#### 중앙집중 시스템 vs 분산시스템
- 장/단점에 따라 구성을 결정
- 경우에 따라서는 혼합할 수 있다. -> 특정한 기능은 중앙서버에서 처리하고 나머지는 분산 서비스로 처리 = **하이브리드시스템**

## 2. 분산 시스템과 블록체인 시스템
### P2P(Peer-to-Peer)
- 대표적인 것 -> 파일이나 노래 파일 공유
- 네트워크의 모든 노드
	1. 서버의 역할
	2. 요청자 역할
-> 모든 노드가 동등하게 역할을 수행한다.


### 분산서버로써 블록체인 시스템의 특징



- 모든 노드가 공유해서 들여다볼 수 있도록 설계되어 있다. 
- 블록체인 시스템은 기본적으로 반복적으로 수행 -> 반복 검증으로 정확성, 투명성 획득

- 데이터 자체는 전혀 보호되지 않고 모든 노드가 공유하는 구조
- 데이터 위조/변조/서비스 무력화에 안전
- 블록체인을 이용하는 것은 효율성 저하 => 시스템의 목적이 상이
- 블록체인 시스템은 기본적으로 반복적으로 수행 -> 반복 검증으로 정확성, 투명성 획득
- 데이터 변경 시 모든 노드가 검증
- 효율성 X
- 투명성, 안전성


#### 블록체인 안전의 양면성
1. 데이터 자체 보호에 무력
2. 다만 데이터의 변형과 서비스 무력화에 저항


### 블록체인 시스템의 잘못된 상식
- 분산 시스템을 이용하므로 효율적이다 ?
- 현재의 블록체인 시스템으로 효율성을 논하는 것은 불가능
-> 중앙화 시스템과 분산 시스템의 차이 -> 일의 분산을 통해서 효율성 확보
-> 일반적 분산 시스템과 블록체인 시스템 차이 -> 일의 중복을 통해서 투명성과 기록 불변성을 확보


## 3. 블록체인의 특성 종합
#### 블록체인 시스템 
- 모든 노드가 동등함을 의미 -> 같은 역할과 의무가 부여됨
- 해커의 목적 분산
	- 최근 블록체인은 특정 노드가 특정 역할을 수행
	- 속도 등의 성능 향상 -> 안전성의 상대적 취약
- 비트코인 블록체인 정의 -> 모든 노드는 상호 동등
- 같은 역할과 의무가 부여 -> 각 노드가 모든 역할을 반복하여 비효율적
- 시스템의 변경에는 항상 장/단점이 존재

모든 노드가 동등하여 해커로부터 안전하나 -> 모든 일을 반복함으로 비효율적인 시스템
- 현업에서 원하는 투명성 정도
- 희생해도 되는 효율성 정도-> 특정 노드의 역할을 결정
- Denial of Service Dos 공격으로부터 가장 안전한 시스템
- 블록체인 시스템은 효율을 위한 시스템.