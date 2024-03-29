# [TIL & Issue Note] 20220317

# 블록체인 원리 - (4) 탈중앙화 합의 및 이중사용

## 1. 탈중앙화 합의 규칙의 개념

### 탈중앙화 합의 규칙
- **규칙 미준수 -> 폐기**
- **규칙 준수 -> 동의**
- ###### 탈중앙화 합의 규칙에는 규칙을 지켰을 때도 퇴출하는 방식이 필요

### 블록체인의 특징
- 구성상 P2P 혹은 피어투피어 방식으로 이루어짐
- 특정 서버가 존재하지 않음
- 브로드캐스팅을 통해서 자신의 피어를 통한 데이터 전송을 받고 있음
- **모든 노드는 현재 자신이 가지고 있는 데이터와 피어로부터 전달받은 데이터에 의존해서 모든것을 판단해야 함**

#### 중앙서버
- 중앙 통제 서버가 모든 지시를 내림 -> 노드는 지시에 따름
#### 블록체인
모든 노드가 동등한 권리와 의무를 가짐 -> 지시를 받을 수 없는 구조

#### 블록체인에서 데이터를 수집할 수 있는 방법
- 네트워크를 통해 정보 수집을 위해 피어를 통한 방식을 사용
- 현재 데이터와 피어가 준 데이터에 기초하여 모든 업무를 처리

#### 서로 다른 블록을 가지고 검증에 임하게 되는 경우 서로 다른 데이터로 구성된 블록체인이 생성됨
- 규칙을 위반한 적이 없음
- 서로의 노드가 각자 먼저 해시 퍼즐을 풀었다고 생각함
- 전파 속도가 늦어져 다른 내용을 만들게 됨

하나의 네트워크에 서로 다른 두 진실이 존재
하나의 동일한 진실만 기록해야 하므로 두 노드 중 하나를 퇴출

### 탈중앙화 합의 과정
- 서로 내용이 상이하여 하나의 값으로 통일하는 과정
- 더 긴 길이의 노드가 이기게 됨
- 블록체인의 데이터가 상이함을 발견하는 순간 두 블록체인의 길이를 비교함
- 동일한 규칙을 지켰음에도 서로 내용이 다르다면 길이를 비교하고 긴 것을 따라 가게 됨

#### 블록체인 데이터가 상이한 이유
- P2P 시스템인 블록체인에 전체 상황을 일괄적으로 통제해서 알려 줄 시스템이 없기 때문


#### 블록체인 데이터의 표현
- 난이도가 일정한 경우 -> 블록체인 길이로 표현 가능 (대부분의 경우)
- 난이도가 다른 경우 -> 블록체인 무게를 통해 표현 가능 

- 상충된 블록체인이 하나로 맞춰짐
- 언제든 동일한 데이터 유지
- 퇴출된 블록에 포함된 보상금 소멸

## 2. 이중사용
### 이중사용
- 동일한 암호화폐를 여러번 사용하려는 악의적 시도를 의미
- 브로드캐스팅을 통해 노드들이 거래내역이 정상인지 판단 -> 상충된다는 사실이 발견되면 폐기
- 블록을 생성하는 노드에 두 거래내역 중 하나만 전달 된 경우 -> 둘 중 하나만 먼저 처리
- 두 가지의 경우가 모두 처리되는 일은 없음 -> 시스템적으로 이중사용은 완벽히 해결

## 3. 이중사용의 상거래적 측면에서의 해석
### 이중사용의 상거래 측면의 해석 
- 시스템상으로 완전히 해결하였으나, 상거래 측면에서는 미해결
- 기록된 내용이 확인 되어야 한다.
	- 탈중앙화의 합의과정에 의해 거래가 취소 될 수도 있다.

### 비트코인을 안전하게 사용 방법
- 이론상 대체로 6개의 블록이 생성될 때까지 기다려야 한다.
- 거래의 즉시성을 해침
- 일반 상거래에서는 부적절 할 수 있다.
