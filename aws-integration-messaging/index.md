# Amazon S3


## Application communication
### Application communication 두 가지 패턴

1. Synchronous communications (application to application)
    - Synchronous는 요청량이 급증하면 애플리케이션간 여러 문제를 유발시킬 수 있다.
2. Asynchronous / Event based (application to queue to application)

#### 애플리케이션 의존관계`decouple`를 낮추는 사례
아래 서비스들을 사용하여 즉각적으로 애플리케이션을 확장가능 하다.
- SQS 사용 : Queue 모델
- SNS 사용 : pub/sub 모델
- Kinesis : 실시간 스트리밍 모델

## Amazon SQS 
### Amazon SQS - Standard Queue
- AWS에서 가장 오래된 서비스. 
- 애플리케이션을 decoupling하기 위해 서비스를 관리
- 중복 메세지를 갖을 수 있다. -> 적어도 한번은 전송 된다.
- Can have out of order messages (best effort ordering)

#### 특징
- **큐에 적재할 수 있는 메세지 및 처리량에 제한이 없다 `unlimited throughput`.** 
- 기본 4일에서 최대 14일까지 큐에 메세지를 보존할 수 있다.
- 낮은 지연 
- 메세지의 크기는 256KB로 제한

#### Produing Message
- SDK의 `SendMessage API`를 통해 SQS 메세지를 생산
- 메세지는 consumer가 삭제하기 전까지 SQS에 유지된다.
- 메세지 보존기간 : 기본 4 days ~ 14 days

{{<admonition type=note title=example >}}
예시) 주문정보를 전송한다.
- Order Id
- Customer Id
- 다른 속성 값들...
{{</admonition>}}


#### Consuming Message
- consumers : 운용중인 EC2 인스턴스, 서버 또는 AWS 람다 서비스 등 
- **Poll SQS for message** : 매 10초마다 메세지를 폴링 한다. 
- **Process the message** :  ex) RDS 데이터베이스에 메세지를 저장 
- `DeleteMessage API`를 사용하여 처리가 완료되면 메세지를 삭제해야 한다.


### Multiple EC2 인스턴스 컨슈머
- 컨슈머들은 메세지를 수신 받고 병렬로 처리할 수 있다.
- 적어도 한번은 전송이 된다.
- 컨슈머는 처리가 완료되면 메세지를 삭제한다.
- 컨슈머를 병렬로 확장하여 프로세스 처리량을 증가시킬 수 있다.

### SQS with Auto Scaling Group (ASG)

### SQS to decouple between application tiers

### SQS 보안
#### 암호화
- HTTPS API를 통한 전송중 암호화
- KMS 키를 통한 `At-rest` 암호화 
- 클라이언트 원하는대로 암/복호화를 클라이언트 측에서 가능

#### 접근제한
- `IAM` 정책을 






