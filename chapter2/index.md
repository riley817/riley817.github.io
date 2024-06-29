# 가상 면접 사례로 배우는 대규모 시스템 설계 기초 Study [2장] 개략적인 규모 추정


{{< admonition type=tip title="Note" open=true >}}
팀 내에서 진행하는 Study 정리 입니다.
{{< /admonition >}} 

{{< admonition type=question title="함께 논의하고 싶은 주제" open=true >}}
- SNS 서비스인 우리 서비스는 DAU(Daily active users)를 산출하고 싶습니다. 우리 회사의 서비스의 활성유저는 어떻게 정의 할 수있을까요?
{{< /admonition >}}

## 2의 제곱수
볼륨의 단위를 2의 제곱수로 표현하면 어떻게 되는지 알아야 한다. 흔히 쓰이는 데이터 볼륨 단위

|2의제곱|근사치|이름|축약형|
|---	|---	|---	|---
|10|	1천만(thousand)|	1킬로바이트(Kilobyte)|	1KB
|20|	1백만(milion)|	1메가바이트(Megabyte)|	1MB
|30|	10억(bilion)|	1기가바이트(Gigabyte)|	1GB
|40|	1조(trilion)|	1테라바이트(Terabyte)|	1TB
|50|	1000조(quadrilion)|	1페타바이트(Petabyte)|	1PB

## 모든 프로그래머가 알아야 하는 응답 지연 값
이들 가운데 몇몇은 더 빠른 컴퓨터가 등장하면서 유효하지 않게 되었지만 아직도 이 수치들은 컴퓨터 연산들의 처리 속도가 어느 정도인지 짐작할 수 있도록 해준다.

- [Google Pro Tip: Use Back-Of-The-Envelope-Calculations To Choose The Best Design](http://highscalability.com/blog/2011/1/26/google-pro-tip-use-back-of-the-envelope-calculations-to-choo.html)

- L1 cache reference 0.5 ns
- Branch mispredict 5 ns
- L2 cache reference 7 ns
- Mutex lock/unlock 100 ns
- Main memory reference 100 ns
- Compress 1K bytes with Zippy 10,000 ns
- Send 2K bytes over 1 Gbps network 20,000 ns
- Read 1 MB sequentially from memory 250,000 ns
- Round trip within same datacenter 500,000 ns
- Disk seek 10,000,000 ns
- Read 1 MB sequentially from network 10,000,000 ns
- Read 1 MB sequentially from disk 30,000,000 ns
- Send packet CA->Netherlands->CA 150,000,000 ns 

### 주의사항
- 메모리는 빠르다 디스크는 아직도 느리다.
- 디스크 탐색`seek`은 피하라
- 단순한 압축 알고리즘은 빠르다
- 데이터를 인터넷으로 전송하기 전 압축하라
- 데이터 센터는 여러 지역에 분산되어 있고 센터들 간 데이터를 주고 받는데에 시간이 걸린다.
- 쓰기 연산은 읽기보다 40배 비싸다

## 가용성에 관계된 수치들
- 고가용성`high availability`은 시스템이 오랜 시간 동안 지속적으로 중단없이 운영될 수 있는 능력을 지청
- 퍼센트로 표현한다. (100%는 한번도 중단된 적이 없음을 의미한다.)
- 아마존, 구글, 마이크로소프트와 같은 사업자는 99% 이상의 SLA를 제공

### 트위터 QPS와 저장소 요구량 추청

#### 가정
- 월간 능동 사용자(Montly active user)는 3억(300million) 명이다.
- 50%의 사용자가 트위터를 매일 사용한다.
- 평균적으로 각 사용자는 매일 2건의 트윗을 올린다.
- 미디어를 포함하는 트윗은 10% 정도이다.
- 데이터는 5년간 보관한다.

#### 추정
QPS(Query per Second) 추정치
- DAU(3million) * 50% = 150 million
- QPS = 1.5 * 2 트윗/24시간/3600초 = 약 3500
- 최대 QPS(Peek QPS)=2 * QPS = 약 700

미디어 저장을 위한 저장소 요구량
- 평균 트윗 크니
  - tweet_id = 64Byte
  - 텍스트 = 140 byte
  - 미디어 = 1 MB

- 미디어 저장소 요구량 : 1.5억 * 2 * 10% * 1MB = 30TB/일
- 5년간 미디어로르 보관하기 위한 저장소 요구량 : 30TB * 365 * 5 = 55 TB



## 더 알아보기 

### 우리서비스의 규모 추정
- 서비스의 목적, 사용자 요구사항, 타켓 시장, 비즈니스 모델에 따라 규모를 추정할 수 있을 것 같다. (우리서비스는 이러한 사항들이 명확히 정해져 있지 않은 것 같다.)
- 지속적으로 접속할 사용자가 존재할지? 마케팅을 위한 이벤트가 생겼을 때 어떤식으로 할지
 

### 가용성의 우선 순위 
서비스의 가용성을 관리하고 평사하는 지표의 우선순위는 서비스의 목적, 사용자 요구사항, 비즈니스 우선 순위에 따라 다를 수 있지만 일반적으로는 SLA`Service Level Argreement`와 RTO`Recovery Time Objective` 가 있다.

1. **SLA (Service Level Agreement)** 
  - SLA는 서비스 제공자가 사용자와의 계약에서 정한 가용성 목표를 나타내는 지표이므로 사용자는 서비스가 SLA 수준의 가용성을 제공하는 것을 기대한다.
  - SLA를 유지하는 것은 신뢰와 계약을 유지하는데 매우 중요하다.

2. **RTO (Recovery Time Objective)**
 - RTO는 복구 시간 목표로, 시스템 장애 발생 시 복구되기를 목표로 하는 최대 시간을 나타낸다.

그 외에도 가용성과 관련된 다양한 지표가 존재한다.

### 고착도 `Stickness` 
- 서비스의 고착도`Stickness` 는 사용자가 서비스에 얼마나 자주 머무르고 상호 작용하는지를 나타내는 지표이다.
- 서비스의 고착도를 산출하기 위해서는 서비스의 목적과 특성, 사용자의 행동 패턴을 고려하여 위의 요소를 분석해야한다.

1. DAU/MAU 비율: DAU를 MAU로 나누어서 DAU 대비 활발한 월간 사용자의 비율을 계산한다.
2. 유저 리텐션(보존) 분석: 일정 기간(예: 30일) 동안 사용자를 추적하여 MAU로부터 특정 기간 동안 사용한 사용자 비율을 분석한다. 리텐션률이 높을 수록 고착도를 나타내는 지표가 된다.
3. DAU/MAU 세그먼트 분석: 사용자 그룹을 나누어서 DAU/MAU 비율을 세분화하여 분석한다. 이렇게 함으로써 특정 사용자 그룹이 서비스에 더 고착되어 있는지 파악할 수 있다.
4. 사용자 활동 패턴 분석: DAU와 MAU에서 얻은 데이터를 사용하여 사용자의 활동 패턴을 분석한다. 특정 사용자가 매일 특정 기능을 사용하거나 상호 작용하는 패턴을 찾는 것이 고착도에 도움을 줄 수 있다.


## 참고
- [페이스북 지표](https://backlinko.com/instagram-users)

