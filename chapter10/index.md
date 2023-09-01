# 가상 면접 사례로 배우는 대규모 시스템 설계 기초 Study [10장] 알림 시스템 설계


{{< admonition type=tip title="Note" open=true >}}
팀 내에서 진행하는 Study 정리 입니다.
{{< /admonition >}} 

# 함께 논의 하고 싶은 주제
---
- 우리 업무와 매우 밀접한 내용이라 매우 유용했습니다.
- 우리서비스에서도 알림을 발송할 때 단말 토큰을 가져오기 위해 서버에서 다른 서버로 통신하는데 그부분을 캐시로 대체하여 리소스를 줄이면 좋을 것 같다는 생각이 들었습니다.
- 발송에 실패했을 때 재시도 할 수 있는 재시도 큐를 추가해야 겠습니다.


# 요약
--
알림 시스템은 모바일 푸시 알림 뿐만 아니라 SMS 메세지, 이메일 세 가지로 분류 할 수 있다.

## 1. 문제 이해 및 설계 범위 확정

### 요구사항 파악하기
- 푸시 알림, SMS 메세지, 이메일 알림을 지원한다.
- 연성 실시간(soft real-time) 시스템이라고 가정한다. 알림은 가능한 빨리 전달어야 하나 시스템 부하가 걸렸을 땐 약간의 지연은 무방
- 지원 단말 : iOS, android, 랩톱/데스크톱
- 전달하는 알람을 만드는 주체 - 클라이언트 애플리케이션 또는 서버 스케쥴링
- 사용자는 알림을 받지 않도록 `opt-out` 설정 할 수 있다.
- 천만 건의 모바일 푸시, 백만 건의 SMS, 오백만 건의 이메일

## 2. 개략적 설계안 제시 및 동의 구하기

### 2.1 알림 유형별 지원 방안
#### iOS 푸시 알림

- **알림 제공자(Provider)** : 알림 요청을 만들어 애플 푸시 알림 서비스(APNS: Apple Push Notification Service)로 보내주는 주체. 
- 알림 요청을 위해 필요한 데이터 
  - 단말토큰(device token) : 알림 요청을 보내는 데 필요한 고유 식별자
  - 페이로드(payload) : 알림 내용을 담은 JSON 딕셔너리

- **APNS** : 애플이 제공하는 원격 서비스. 푸시 알림을 iOS 장치로 보내는 역할

- **iOS 단말(iOS Device)** : 푸시 알림을 수신하는 사용자 단말

#### 안드로이드 푸시 알림
- 안드로이드 푸시 알림도 비슷한 절차로 전송. APNS 대신 FCM(Firebase Cloud Messaging)을 사용

#### SMS 메세지
- SMS 메세지를 보낼 때는 보통 [트윌리오](https://www.twilio.com/en-us), [넥스모](https://www.vonage.com/communications-apis/) 같은 제3자 서비스를 이용한다. 대부분 상용서비스로 이용 요금을 지불한다.
   
#### 이메일
- 센드그리드, 메일침프 등 이메일 서버를 자체적으로 구축하는 대신 상용 서비스를 많이 이용한다.
- 전송 성공률도 높고, 데이터 분석서비스도 제공한다.

### 2.2 연락처 수집 절차
- 알림을 보내려면 모바일 단말 토큰, 전화번호, 이메일 주소 등의 정보가 필요
- API를 사용하여 사용자 정보를 서버에 저장

### 2.3 알림 전송 및 수신 절차
#### 개략적 설계안 (초안)
{{<figure src="/posts/images/system-design-interview/system-design-figure-10-9.png#center">}}

- **서비스(1~N)** : 각각의 마이크로서비스일 수도 있고 크론잡일 수도 이씨고 분산 시스템 컴포넌트 일 수도 있다.
- **알림 시스템** : 알림 전송/수신 처리의 핵심. 서비스를 위한 알림 API 제공과 제 3자 서비스에 전달할 알림 페이로드도 만들어 낼 수 있어야 한다.
- **제3자 서비스** : 사용자에게 알림을 실제로 전달. 확장성에 유의(쉽게 통합, 제거 가능해야함). 
- **iOS, 안드로이드, SMS, 이메일 단말** : 사용자는 자기 단말에서 알림 수신

#### 설계안의 문제점
- **SPOF(Single-Point-Of-Failure)** : 알림 서비스에 서버가 하나밖에 없어 그 서버에 장애가 생기면 전체 서비스의 장애
- 규모 확장성 : 데이터베이스나 캐시 등 중요 컴포넌트의 규모를 개별적으로 늘릴 방법이 없음
- 성능 병목 : 사용자 트래픽이 몰리는 시간에는 시스템 과부하 상태에 빠질 수있음

#### 개략적 설계안 (개선된 버전)
- 데이터베이스와 캐시를 알림 시스템의 주 서버에서 분리
- 알림 서버 증설하고 자동으로 수평적 규모 확장이 이루어질 수 있도록 함
- 메세지 큐를 사용 시스템 컴포넌트 사이의 강한 결합을 끊는다.

{{<figure src="/posts/images/system-design-interview/system-design-figure-10-10.png#center">}}

**알림 서버(notification server)**
- 알림 전송 API : 내부 또는 인증된 클라이언트만 이용 가능
- 알림 검증 : 이메일 주소, 전화번호 등 기본적인 검증 수행
- 데이터베이스 또는 캐시 질의 : 알림에 포함시킬 데이터를 가져오는 기능
- 알림 전송 : 알림 데이터를 메시지 큐에 넣는다. 하나 이상의 큐로 병렬적으로 처리 가능
- 데이터베이스 : 사용자, 알림, 설정 등 다양한 정보 저장
- 메세지 큐 : 시스템 컴포넌트 간 의존성을 제거하기 위해 사용. 다량의 알림이 전송되어야 하는 경우 대비한 버퍼 역할
- 작업 서버 : 메세지 큐에서 전송할 알림을 꺼내서 제 3자 서비스로 전달하는 역할

1. API 호출하여 알림 서버로 알림 전송
2. 알림 서버는 사용자 정보, 단말 토큰, 알림 설정 같은 메타데이터를 캐시나 데이터베이스에서 가져온다.
3. 알림 서버는 전송할 알림에 맞는 이벤트를 만들어 해당 이벤트를 위한 큐에 넣는다. 
4. 작업 서버는 메세지 큐에서 알림 이벤트로 꺼낸다
5. 작업 서버는 알림을 제3자 서비스로 보낸다.
6. 제3자 서비스는 사용자 단말로 알림을 전송

## 3. 상세 설계
### 3.1 안정성(reliability)

#### 데이터 손실 방지
- 알림이 지연되거나 순서가 틀려도 상관없으나 소실되어서는 안된다.
- 시스템 데이터를 데이터베이스에 보관하고 재시도 매커니즘을 구현

#### 알림 중복 전송 방지
- 같은 알림이 여러번 전송되는 것을 완전히 막는 것은 없다.
- 보내야 할 알림이 도착하면 그 이벤트 ID를 검사 후 이전에 처리된 적 있는 이벤트인지 체크

### 3.2 추가로 필요한 컴포넌트 및 고려사항

#### 알림 템플릿
- 알림 템플릿은 인자나 스타일, 추적 링크를 조장하기만 하면 사전에 지정한 형식에 맞춰 알람을 만들어 내는 툴
- 형식을 일관성있게 유지 오류 가능성뿐만 아니라 알림 작성 시간을 단축

#### 알림 설정
- 사용자가 알림 설정을 상세히 조정할 수 있다. 알림을 전송하기 전에 사용자가 해당 알림을 켜두었는지 확인해야 한다.

#### 전송률 제한
- 사용자에게 너무 많은 알림을 보내지 않도록 하는 방법은 알림의 빈도를 제한

#### 재시도 방법
- 제3자 서비스가 알림 전송에 실패하면 알림을 재시도 전용 큐에 넣는다.
- 문제가 계속해서 발생시 개발자에게 통지한다.

#### 푸시 알림과 보안
- 인증, 승인 된 클라이언트만 API를 사용하여 알림을 보낼 수 있다.

#### 큐 모니터링
- 알림 시스템을 모니터링 시 중요한 메트릭 하나는 큐에 쌓인 알림 개수이다.

#### 이벤트 추적
- 알림 확인율, 클릭율, 실제 앱 사용으로 이어지는 인사이트를 추적할 수 있다.
- 보통 알림 시스템을 만들면 데이터 분석 서비스와도 통합해야 한다.

### 3.3 수정된 설계안
{{<figure src="/posts/images/system-design-interview/system-design-figure-10-10.png#center">}}
- 알림 서버에 인증과 전송률 제한 기능이 추가되었다.
- 전송 실패에 대응하기 위한 재시도 기능이 추가. 실패 알림 큐에 넣고 지정된 횟수만큼 재시도
- 전송 템플릿을 사용하여 알림 생성과정의 단순화 및 일관성 유지
- 모니터링과 추적 시스템을 추가하여 시스템 상태를 확인 추후 시스템 개선을 쉽게 한다.

## 4. 마무리
- 안정성 : 메세지 전송 실패율을 낮추기 위해 안정적인 재시도 매커니즘 도입
- 보안 : 인증된 클라이언트만 알림을 발송
- 이벤트 추적 및 모니터링 : 알림 과정을 추적 시스템 상태를 모니터링하기 위해 알림 전송의 각 단계마다 이벤트를 추적하고 모니터링 할 수 있는 시스템 구축
- 사용자 설정 : 사용자가 알림 수신 설정을 조정할 수 있다.
- 전송률 제한 : 사용자에게 알림을 보내는 빈도를 제한

