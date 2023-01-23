# [TIL & Issue Note] 20220324



# 블록체인 원리(7) - 채굴과 51% 공격

## 1. 채굴과 블록 생성
### 블록을 만드는 것 = 채굴
- 보상금 획득 하는것이 금 획득하는 것과 비슷
- **채굴** 단어의 유래 - Nick Szabo
	- 금이 가치를 갖는 것은 채굴이 어렵기 때문
	- **어떤 문제가 매우 어렵다면 -> 문제의 정답 자체가 어떤 가치를 가지지 않을까?**
- 블록 만듦 -> 해시 퍼즐 풀이 : 어려운 문제이기 때문에 정답 자체가 가치 보유
- 금을 캐려면 -> 금광에서 채굴 : 힘든 작업이기 때문에 금이 가치 보유
- 해시 퍼즐 풀이 ->(비유) 금광에서 채굴
- 채굴 == 블록을 만들다.

### 블록을 왜 만들었을까?
- 블럭을 생성해야 거래내역을 기록할 수 있다.
- 해시 퍼즐 및 연쇄 -> 비가역성 보유
- 비가역성 거래내역을 기록하기 위해


### 해시퍼즐
- 무작위로 해시함수 계산, 특정 패턴이 나올 때까지 무한 반복
- 풀이에 많은 에너지 필요
- 보조금 삭감 -> 시세 하락 -> 채굴업자에게 영향
- 채굴 시 소모 에너지 > 채굴 보상금 -> 채굴이 완전히 멈추는 현상 발생


### 채굴업자가 더 이상 채굴하지 않을 경우
- 거래내역을 담을 수 없게 됨 -> **더 이상 블록체인이 움직이지 않음**
- 채굴의 목적 -> 경제적 수익
- 채굴 비용 > 채굴 수익 -> 블록체인 거래가 불가능하게 됨

### 채굴이 갑자기 멈추지 않는 이유
- 이미 채굴한 양이 많기 때문
- 한계점이 이상을 지나면 멈추는 시점이 찾아옴

### 발행량 반감 -> 시세 하락 -> 블록체인 안정도와 직접 영향


## 2. 51% 공격
### 51% 공격
- 누군가 엄청난 하드웨어를 소유했다면?
- 단 한명이 나머지 사람들의 하드웨어보다 더 많은 자원을 가지는 현상
- 자원 및 계산량이 많으므로 -> 블록을 만들 가능성이 높아짐
- 블록을 만들 확률 51%
- 블록을 만들지 못할 확률 49%
- **특정한 노드가 높은 확률로 계속 블록을 만드는 현상 발생**

### 51% 시스템에 미치는 영향
- 거래내역 기록 -> 블록 생성자가 결정
- 높은 수수료 거래내역 먼저 처리
- **자기 임의대로 거래내역 작성**
- **시스템 교란 목적으로 거래내역 미작성** -> 시스템 마비
- 누군가가 소유하고 있는 계산력 -> 해시파워

### 해시파워
- 해시 퍼즐을 풀 수 있는 능력
- 시스템 계산량 51% = 해시 파워 51% 독점
- 52%의 해시 파워로 다른 노드 보다 항상 더 높은 확률로 블록 생성

### 블록 생산권 독점 시 악의적 상황
#### 거래내역 미작성 
- 서비스 자체가 방해되어 블록체인 서비스 비활성화
- 자신이 원하는 거래내역만 기록

#### 수수료 인상
- 트랜잭션 처리 선택은 블록 생산자에게 달림
- 특정 수수료 지급 전까지 트랜잭션 미처리
- **블록 생산자가 원하는 수수료를 지불**할 수 밖에 없음

#### 이중 사용
- 동일한 비트코인을 두 번 이상 사용하려는 시도
- 상충된 지급내역이 브로드캐스팅 되어 네트워크에 확산
- 독점권 소유자가 **자신에게 되돌아오는 거래내역만 처리**
	- 이중사용 항상 성공
- 자기 자신이 블록을 결정

#### 긴 블록 생성 후 기존 블록 퇴출
- 긴 블록체인 생성 후 기존 블록 퇴출
- 시스템 자체의 안전성을 저해

### 51% 공격
- **특정 노드가 해시 파워 독점, 시스템 전체를 교란시키는 공격** 
- 과반수 이상을 차지한 상징적 경우 의미
- 이클립스 공격, 이기적인 채굴자 공격 -> 과반수 해시 파워 없이 시스템 교란 가능
- 25% 시스템 독점 시 시스템 교란 가능 입증

### 51% 공격, 실제로 일어날까?
- 상위 10개 채굴 업체가 보유 -> 전체 해시 파워 55% 장악
- 언제든 51% 공격 가능
- **공격 확률은 낮다 -> 경제적 이득이 적음**

### 51% 공격자가 이득을 얻는 경우

- 디지털 서명, 비대칭 암호화 기법, 해시 함수 -> 원천적으로 불가능
- **타인의 비트코인 약탈 불가능**
- 이득을 얻는 유일한 방법은 이중사용

#### 이중 사용
- 물건을 건내 준 사람이 무한정 기다리면 무조건 실패
- 타인이 비트코인이 아닌 자신의 비트코인 두 번 사용
	- 타인의 비트코인 약탈 불가능
- **51% 공격을 감행해서 가치를 떨어뜨리는 것보다 가치를 유지하면서 더 많은 보상금을 획득하는 것이 나음**


### 현재 블록체인 구조
- 정상적 블록 생성이 경제적으로 더 이득
- 51% 공격 발생 확률 높지 않음
- 랜덤화를 통해 투명성을 이끌었던 블록체인의 기본정신 훼손
- 상위 3개 업체가 과반 이상의 블록 생성
- 10% 업체가 전체 90% 블록 생산 
- 99.9999% ->의존 0.0001%
- 블록체인 존립에 심각한 위협 요소
- 단 10개의 노드가 채굴 중지 시 더 이상 거래 불가능

### 비트코인 블록체인
- 채굴자원의 독점 심화, 심각한 위협

----
## 채굴과 블록 생성
- 채굴은 블록 만들기의 다른 이름임
- 블록을 만든다는 의미는 거래내역을 보관한다는 의미임


## 51% 공격
- 51% 공격이란 특정 노드가 시스템 해시자원 51% 이상을 독점한 상황을 상징적으로 나타내는 말을 뜻함
- 시스템 자원을 51% 독점하게 되면 블록 생산권을 독점하게 되고, 이에 따라 다양한 형태로 시스템 공격이 가능함
- 51% 공격은 경제적 이득보다는 주로 시스템 교란에 의한 서비스 단절이 주된 위협임

# 중개인이 없는 거래 - 스마트 계약 개념 및 이더리움 구현 방식

## 1. 중개인이 없는 거래
### 상거래에서의 제 3자 이용
- 은행이라는 제 3자를 두고 계좌이체하는 것
- 이때 은행은 중앙 서버역할 수행 및 수수료 획득

### 제 3자를 활용한 거래의 단점
- 임의 기록 수정
- 악의적 의도 기록 변경
- 불필요한 수수료 발생

### 중개인이 없는 거래가 가능하다면
- 불필요한 수수료 절감

### 블록체인 거래
- **중앙 서버 없이도 안심하고 거래할 수 있는 환경 구축**
- 해시파워를 제일 먼저 해결한 노드가 기록함 -> 다만, 모든 참여자의 검증
- 중앙서버에 비해 높은 안정성 보유
- 별도 수수료 지급하지 않음
- **직거래 시스템 구축 가능**
	- 응용분야 : 중고 물품 거래, 중고차 거래
	- 기대 : 수수료 절감

### 중개인이 없는 거래의 이면
- 항상 좋은 것일까?
- 비트코인을 사용하다가 비밀번호를 잊어버리면 ?
- 비트코인 키를 저장한 장치를 훼손하게 되면?
- 잘못된 주소로 비트코인을 전송하게 되면?

### 금융거래 중개인
- 은행의 비밀번호를 잊어버리면?
- 은행 공인인증서를 훼손하게 되면다면?
	- 은행에 중재 요청하여 해결 가능

### 중개인이 없는 거래 수수료가 절감 되는가?
- 채굴업자에게 지급하는 수수료가 더 적은지는 알 수 없음


### 절대 익명의 거래
- 많은 문제점 발생, 중재자 필요

#### 중고차를 익명으로 거래한 경우
- 이득 -> 수수료 절감
- **단점 -> 중고차를 구매인은 하자 발생 시 중재자 없음**

#### 블록체인을 통한 중고차 직거래?
- 아직은 희망사항일 뿐, 해결해야하는 문제 다수 존재
- 개인정보 보호 문제 해결

#### 브로드캐스팅
- 모든 노드가 블록체인 데이터 확인 가능
- 개인정보가 담기게 된다면 -> 모든 노드가 개인정보 확인 가능
- 브로드캐스팅을 사용하는 한 개인정보를 담아서는 안 됨

#### 블록체인을 이용한 거래
- 개인 정보 불필요한 거래 성사 전까지 해결 요소 많음


## 2. 스마트 계약

### 스마트 계약
- 법률가 Nick Szabo -> 1990년대 자신의 아이디어 
- 제 3자의 개입 없이도 스스로 계약을 집행할 수 없을까?
- 디지털 환경을 잘 꾸미면 제 3자 개입 없이 계약을 집행할 수 있을 거라 생각

- **제 3가 개입하지 않아도 계약 집행 가능**

### 대부분의 계약
- 공증을 위한 변리사, 법무사, 변호사

### Nick Szabo 스마트 계약
- **스스로 디지털화 되어 집행하는 계약**
- 블록체인의 직접 거래와 같은 맥락

### 비트코인 블록체인에 기록할 수 있는 것
- 정적인 내용
- 비트코인 거래 기록 외 다른 기록을 하기 위해서는 프로그램 자체를 수정해야 한다.
- **프로그램을 수정하지 않고 다른 용도로 블록체인을 사용할 방법이 없을까 -> 동적인 데이터를 담으면 어떨까?**

### 블록에 담긴 내용이 컴퓨터 프로그래밍이라면?
- 프로그램을 통해 명시한 조건에 따라 다른 결과 도출
- 디지털 환경을 통한 스마트 계약 -> Nick Szabo 아이디어 동일
- 프로그램을 호출하여 계약을 집행
- 프로그램 -> 계약내용 , 저장장치 -> 프로그램을 불러오는 장치
- 닉 사보의 스마트 계약이 실행 될지도 모름

### Smart contract
- 정적인 거래내역 이외에 거래내역 자체를 컴퓨터 프로그램으로 기술하는 방식
-  **호출이 가능하여 호출 때마다 실행**
- 메세지를 통한 매개 변수에 따라 서로 다른 작용
- 동적으로 여러가지 다른 용도 사용 가능

### 비트코인 블록체인의 스마트 컨트랙트
- 아주 기초적으로 구현되어 있음
- 사카시 나카모토도 스마트 컨트랙트를 구현해둠 -> 실행을 위한 도구는 미제공

### 이더리움
- 스마트 컨트랙트 모습을 완전히 갖춘 최초의 블럭체인

### 계약과 컨트랙트(Contract)
- 계약 : 금융, 결제에 국한. 국한적 의미
- 컨트랙트 : 금융, 결제 외 광범위한 의미. 범용적 의미
- 거래내역, 계약, 컨트랙트는 기본적으로 같은 의미

## 3. 이더리움에서의 스마트 계약

### 이더리움에 구현된 스마트 계약
- 스마트 계약 : 제3자의 개입없이 스스로 집행하는 계약의 구상

#### 스마트 계약 구현 방법
- 정적인 기록이 아닌 프로그램 기록
- **이더리움에서는 프로그램화 된 내용을 블록에 쉽게 담을 수 있는 환경을 제공** -> IDE(Integrated development Environment) 
- 원하는 프로그램 작성 -> 컴파일 -> 이더리움에 불러오도록 저장
- 기존비트코인(정적인 기록 저장) <-> IDE(프로그램 저장)

- 사용자가 작성 프로그램 결정 -> 그 방식에 따라 무한가지 용도

#### 솔리더티 
- 이더리움에 사용되는 언어
-  솔리더티를 사용하여 프로그램 -> 컴파일 -> 코드저장
- 언제든 호출 가능하고 호출되면 프로그램 실행, 결과는 블록체인에 저장
- 같은 기록이라도 실행될 때의 매개 변수, 컨디션에 따라 다른 겨로가 도출

### 프로그램을 통한 암호화폐 전달 기능
#### 비트코인을 통해 축구 내기를 한 경우
- 제3자가 필요 
- 승자에게 비트코인 전달이 제대로 이루어졌는지 중재해야함

#### 이더리움을 통해 축구 내기를 한 경우
- 축구 내기 프로그래밍
- 당사자가 만날 필요 없음
- 승자에게 자동으로 이더리움 전송

1. 내기 프로그래밍 하기
2. 스마트 컨트랙트 호출, 내기 참여
3. 결과에 따라 이더리움 전송 기다리기

### 컴퓨터를 범용기계라고 하는 이유
- 게임 소프트웨어 설치 : 게임기
- 스프레드 시트 사용 : 회계용도
- 어떤 프로그램을 구동하느냐에 따라 용도 변경

#### 비트코인
- 하나의 용도만 가진 기계

#### 이더리움
- 최초의 범용 블록체인
- 용도 무한대 가능

### 스마트계약 해결애햐할 문제
- 이더리움을 이용한 스마트 계약 -> 여전히 해결해야 할 문제 다수

----

## 중개인이 없는 거래
- 제 3자의 개입 없이 블록체인 거래
- 필요 없는 수수료 절감과 함께, 한 번 기록되면 변경되지 않는 플랫폼을 구축하는 것을 목표로 함

## 스마트 계약
- 스마트 컨트랙트는 제 3자의 개입 없이도 계약을 실행할 수 있는 디지털 환경에 대한 개념
- 이더리움에 의해 블록체인에 구현

## 이더리움에서의 스마트 계약
- 이더리움에서는 솔리더티 등의 언어로 프로그램을 작성할 수 있는 통합 환경 제공
- 프로그램을 통해 여러 조건을 정의 할 수 있음
- 프로그램은 컴파일 된 후 블록에 저장되고 호출되어 실행 됨
- 