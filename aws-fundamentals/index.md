# AWS Fundamentals


## 확장성 및 고가용성
- 확장성`Scalability`은 애플리케이션/시스템이 더 큰 부하를 처리할 수 있게 적용되는 것을 의미
- 확장성 종류
    - Vertical Scalability
    - Horizontal Scalability (= elasticity)
- 확장성은 고 가용성(HA)과 연결되어 있지만 서로다르다.

### Vertical Scalability
- 수직적 확장은 인스턴스 크기를 증가 시키는 것을 의미
- 예시) t2.micro -> t2.large로 확장
- 수직적 확장성은 데이터베이스와 같은 비분산 시스템에서 매우 일반적
- RDS, ElastiCache는 수직 확장이 가능한 서비스
- 일반적으로 수직확장에는 제한이 있음(하드웨어 제한)

### Horizontal Scalability
- 수평적 확장은 애플리케이션의 인스턴스 혹은 시스템 수 증가
- 분산 시스템을 의미
- 웹 애플리케이션/현대 애플리케이션에서 매우 일반적
- Amazon EC2와 같은 클라우드 제품 덕분에 수평확장 용이

### High Availability
- 고가용성(HA)은 일반적으로 수평 확장과 함께 제공
- 최소 두 개의 데이터 센터(=A)에서 애플리케이션 및 시스템을 실행하는 것을 의미
- 고가용성의 목표는 데이터 센터 손실에서 살아 남는 것
- 고가용성은 수동적일 수 있음 (RDS Multi AZ의 경우)
- 고가용성은 활성화 할 수 있음 (수평적 확장을 위해)

### EC2를 위한 고가용성 및 확장성
- **Vertical Scaling** : 인스턴스 사이즈를 늘린다.(`=scale up/down`)
- **Horizontal Scaling** : 인스턴스 갯수를 늘린다. (`=scale out/in`)
- **High Availability** : 여러 AZ에서 동일한 애플리케이션에 대한 인스턴스를 실행 
    - Auto Scaling Group multi AZ
    - Load Balancer multi AZ

## Load balancing
- 로드 밸런싱은 트래픽을 여러 서버(e.g., EC2 인스턴스) 다운스트림으로 전달하는 서버를 의미
<center><image src="/posts/images/aws/load-balancing.png">
</center>

### Load balancer를 사용하는 이유
- 여러 다운스트림을 인스턴스에 로드 분산
- 애플리케이션에 단일 액세스 지점(DNS) 노출
- 다운스트림 인스턴스의 장애를 원할하게 처리
- 인스턴스의 정기적인 상태 검사 수행
- 웹 사이트에 대한 SSL termination (HTTPS) 제공
- 영역간 고가용성
- 공용 트래픽과 사설 트래픽 분리

### Elastic Load Balancer를 사용하는 이유
- Elastic Load Balancer **AWS에 의해 관리되는 로드 밸런서**
    - AWS가 업그레이드, 유지보수, 고가용성 관리
    - AWS는 몇 가지 구성 knobs 제공

- AWS의 많은 제품/서비스와 통합 가능
    - EC2, EC2 Auto Scaling Groups, Amazon ECS
    - AWS Certificate Manager(ACM), CloudWatch
    - Route 53, AWS WAF, AWS Global Accelerator

### Health Checks
- `Health Checks`은 로드 밸런싱 장치에서 매우 중요
- 로드 밸런서는 트래픽을 전달하는 인스턴스가 요청에 응답할 수 있는지 여부를 알 수 있도록 한다.
- 상태 점검은 포트 및 route에서 수행 (`/health` 는 공통임)
- `200` 응답이 아닌경우 인스턴스는 unhealthy

### AWS 로드 밸런서 종류
- 전반적으로 새로운 세대의 로드 밸런서는 더 많은 기능을 제공하므로 사용하는 것이 좋다.
- 일부 로드 밸런서는 내부(private), 외부(public) ELB로 설정가능

| **Load balancer 종류**         ||
|:------------------------------------|--|
| **Classic Load Balancer `CLB` v1**     |HTTP, HTTPS, TCP, SSL(Secure TCP)         |
| **Application Load Balancer `ALB` v2** |HTTP, HTTPS, WebSocket         |
| **Network Load Balancer`NLB` v2**     | TCP, TLS(secure TCP), UDP        |
| **Gateway Load Balancer`GWLB`**     |Operates at layer3 (Network layer) - IP Protocol         |

#### Classic Load Balancers (v1)
- TCP(Layer 4), HTTP & HTTPS(Layer 7) 지원
- 상태 검사는 TCP 또는 HTTP 기반
- 고정 hostname : `http://xxx.region.elb.amazonaws.com/`

<center><image src="/posts/images/aws/clb.png">
</center>

#### Application Load Balancer (v2)
- Layer 7 (HTTP)
- 여러 시스템(target groups)에서 여러 HTTP 애플리케이션에 대한 로드 밸런싱
- 동일한 시스템의 여러 애플리케이션에 대한 로드 밸런싱(example: containers)
- HTTP/2 및 웹 소켓 지원
- redirects 지원 (예: HTTP -> HTTPS)
{{<admonition tip "다른 대상 그룹으로 라우팅">}}
- URL 기반 라우팅 : 
    - `example.com/users`
    - `example.com/post`
- URL hostname 기반 라우팅
    - `one.example.com`
    - `other.example.com`
- Query string, 헤더 기반 라우팅
    - `example.com/users?id=123&order=false`
{{</admonition>}}    

- ALB는 마이크로 서비스와 컨테이너 기반 애플리케이션에 매우 적합 (example : Docker & Amazon ECS)
- ECS의 동적 포트로 리디렉션하는 포트 매핑 기능 있음
- 그에 비해, 애플리케이션 당 여러 Classic Load Balancer 가 필요

##### ALB Target Group
- EC2 인스턴스(Auto Scaling Group에 의해 관리할 수 있는) - HTTP
- ECS(ECS 자체 관리) Task - HTTP
- 람다 함수 : HTTP 요청이 JSON 이벤트로 변환
- IP 주소 : 반드시 사설 IP 요청
- ALB는 여러 대상 그룹으로 라우팅 할 수 있음
- Health check는 대상 그룹 레벨

##### ALB 알아야할 내용
- 고정 호스트이름 : `XXX.region.elb.amazonaws.com`
- 응용 프로그램 서버가 클라이언트 IP를 직접 확인하지 않음
    - 클라이언트 실제 IP가 **X-Forwarded-For** 헤더에 삽입
    - 포트와 `X-Forwarded-Port`와 프로토 `X-Forwarded-Proto`도 가져올 수 있음

#### Network Load Balancer (v2)
- Network load balancer(Layer 4)를 통해:
    - **TCP, UDP 트래픽을 인스턴스로 전달**
    - 초당 수백만 건의 요청 처리
    - 지연 시간 최대 ~100ms(vs 400ms for ALB) 단축
- NLB는 AZ당 하나의 정적 IP를 가지며 Elastic IP 할당을 지원 (특정 IP 화이트 리스트에 도움이 됨.)
- TCP 또는 UDP 트래픽의 최고의 성능을 위해 사용
- AWS 자유 계층에 포함되지 않음

##### NLB – Target Groups
- EC2 인스턴스
- IP 주소 : 반드시 사설망 IP
- Application Load Balancer

#### Gateway Load Balancer
- AWS에서 써드파티 네트워크 가상 어플라이언스 구축, 확장 및 관리
- 예시) 방화벽, 침입 탐지 및 차단 시스템, 심층 패킷 검사 시스템, 페이로드 조작 등
- Layer 3 (네트워크 Layer)에서 작동 - IP Packets
- 아래 기능들과 통합할 수 있다.
    - **Transparent Network Gateway**: 모든 트래픽에 대한 단일 입력 수신
    - **Load Balancer** : 가상 어플라이언스로 트래픽 분산
- **GENEVE protocol** 6081 포트로 사용할 수 있다. 

##### Gateway Load Balancer - Target Groups
- EC2 instances
- IP Addresses : 반드시 private IP

## Stick Sessions (Session Affinity)
- 동일한 클라이언트가 항상 로드밸런서 뒤에 있는 동일한 인스턴스로 리다이렉션 되도록 고정하는 기능을 구현
- CLB & ALB의 기능
- 고정 `cookie`에는 사용자가 제어하는 만료일자가 있음
- 사용 사례 : 사용자가 세션 데이터를 잃지 않게 하기
- `stickiness`를 사용하도록 설정하면 백엔드 EC2 인스턴스에 대한 로드가 불균형해질 수 있다.

### Cookie Names
#### Application-based Cookies
##### 사용자 정의 쿠키
- 애플리케이션에 의해 사용자가 지정하는 모든 특성을 포함 가능
- 쿠키 이름은 각 대상 그룹`target group`에 대해 개별적으로 지정해야 함
- `AWSALB`, `AWSALBAPP`,`AWSALBTG` 등 ELB 예약어는 사용하지 말것
##### Application cookie
- 로드밸런서에 의해 생성
- 쿠키의 이름은 `AWSALBAPP`

#### Duration-based Cookies
- 로드밸런서에 의해 생성되는 쿠키
- 쿠키의 이름은 ALB는 `AWSALB`, CLB는 `AWSELB`

## Cross-Zone 로드 밸런싱
### Cross-Zone 로드 밸런싱 사용
- 각 로드 밸런서 인스턴스는 모든 AZ에 등록된 모든 인스턴스에 고르게 분산
<center><image src="/posts/images/aws/cross_zone_load_balancing_enabled.png">
</center>

### Cross-Zone 로드 밸런싱을 사용하지 않음
- 요청이 Elastic Load Balancer 노드의 인스턴스에 분산
<center><image src="/posts/images/aws/cross_zone_load_balancing_disabled.png">
</center>

| **Load balancer**             | **Cross-Zone Load Balancing** |
|-------------------------------|-------------------------------|
| **Application Load Balancer**|· 항상 동작(비활성화 불가능)<br/>· AZ간 요금 없음 |
| **Network Load Balancer**|· 기본적으로 사용 안함<br/>· 활성화 된 경우 AZ간 요금 지불|
| **Classic Load Balancer**|· 기본적으로 사용 안함<br/>· 활성화 된 경우 AZ간 데이터 요금 없음|

## SSL/TLS 기본
- SSL 인증서를 통해 클라이언트와 로드 밸런서 간 트래픽을 전송 중 암호화를 할 수 있다.
- SSL은 암호화를 하는데에 Secure Socket Layer를 사용
- TLS는 최신 버전인 전송 보안 계층을 나타낸다.
- 요즘에는 **TLS 인증서가 주로 사용되며** 여전히 SSL이라고 부른다.
- 공용 SSL 인증서는 CA(Certificate Authorities)에 의해 발급된다.
- SSL 인증서는 만료 날짜가 있으므로 갱신해야 한다.

### Load Balancer - SSL Certificates
- 로드 밸런서는 `X.509` 인증서 (SSL/TLS 서버 인증서)를 사용
- ACM(AWS Certificate Manager)을 사용하여 인증서를 관리할 수 있음
- 자신의 인증서를 만들어 업로드도 가능
{{<admonition tip "Http listener">}}
- 기본 인증서를 지정해야 한다.
- 여러 도메인을 지원하기 위해 선택적 으로 인증 목록을 추가 가능
- **클라이언트는 SNI(Server Name Indication)를 사용하여 도달하는 호스트 이름을 지정할 수 있다**
- 이전 버전의 SSL/TLS(레거시 클라이언트)를 지원하는 정책 지정 가능
{{</admonition>}}

### SSL - Server Name Indication (SNI)
- SNI는 **여러 개의 SSL 인증서를 하나의 웹 서버에 로드하는 문제를 해결**(여러 웹사이트를 서비스하기 위해)
- 새로운 프로토콜이며 클라이언트가 초기 SSL 핸드쉐이크 대상 서버의 호스트 이름을 표시해야한다. -> 그러면 서버가 올바른 인증서를 찾거나 기본 인증서를 반환

{{<admonition note>}}
- ALB, NLB, CloudFront 에서만 동작
- CLB에서는 동작하지 않음
{{</admonition>}}

<center><image src="/posts/images/aws/sni.png">
</center>

| **Load balancer**             | **SSL 인증서** |
|-------------------------------|-------------------------------|
| **Classic Load Balancer**|· 하나의 SSL 인증서만 지원<br/>· 여러 SSL을 갖는 여러 호스트 이름에는 여러 CLB를 사용|
| **Application Load Balancer**|· 여러 SSL 인증서를 가진 리스너 지원<br/>· SNI를 사용하여 작동 |
| **Network Load Balancer**|· 여러 SSL 인증서를 가진 리스너 지원<br/>· SNI를 사용하여 작동 |

## Connection Draining
<center><image src="/posts/images/aws/connection-draining.png">
</center>

- 인스턴스가 등록 취소 중이거나 정상 상태가 아닌 동안 인스턴스에 어느 정도 시간을 주어  `in-flight requests`을 완료할 수 있도록 만들어 주는 기능
- 각 ELB 타입마다 이름이 다름
    - CLB  : `Connection Draining`
    - ALB & NLB : `Deregistration Delay`(등록 취소 지연)
- 등록취소`de-registering` 되면 등록 취소 중인 EC2 인스턴스로는 새로운 요청을 보내지 않는다.
- 1 ~ 3,600초 (기본값 : 300초)
- 비활성화 가능 (값을 0으로 설정)
- 요청이 짧은 경우 낮은 값으로 설정

## Auto Scaling Group
### Auto Scaling Group(ASG) 목표
- 증가하는 로드에 맞게 **Scale Out** : EC2 인스턴스 추가
- 감소하는 로드에 맞게 **Scale In** : EC2 인스턴스 제거
- AGS에서 실행되는 최소 및 최대 EC2 인스턴스 수를 보장
- 새 인스턴스를 자동으로 로드 밸런서에 등록
- 이전 인스턴스가 비정상 혹은 종료된 경우 EC2 인스턴스 다시 생성
- ASG는 무료 (기본 EC2 인스턴스에 대해서만 지불)

### Auto Scaling Group 속성
- **Launch Template** (이전 버전 Launch configurations는 더이상 사용되지 않음)
    - AMI + Instance Type
    - EC2 User Data
    - EBS Volumes
    - Security Groups
    - SSH Key PAir
    - IAM Roles for your EC2 Instance
    - Network + Subnets Information
    - Load Balancer Information
- Min Size / Max Size / Initial Capacity
- Scaling 정책

### Auto Scaling - CloudWatch Alarms & Scaling
- CloudWatch 알람을 기반으로 AGS 확장 가능
- alarm이 메트릭(**CPU 평균 또는 사용자 지정 메트릭**) 등을 모니터링
- 전체 ASG 인스턴스에 대해 평균 CPU와 같은 메트릭이 계산
{{<admonition warning "경보 기준">}}
- 스케일 아웃 정책 생성 (인스턴스 수 증가)
- 스케일 인 정책 생성(인스턴스 수 감소)
{{</admonition>}}

#### Dynamic Scaling Policies
| **Dynamic Scaling Policies** | |
|------------------------------|-------------------------------|
| **Target Tracking Scaling(대상추적 스케일링)**  |· 가장 간단하고 쉽게 설정<br/>example) 나는 ASG 평균 CPU가 약 40%를 유지하기를 원한다.|
| **Simple / Step Scaling**    |· CloudWatch 경보가 트리거되면(CPU > 70%), 2대 추가 등|
| **Scheduled Actions**        |· 알려진 사용 패턴을 기반으로 확장 예상 <br/>example) 금요일 오후 5시에 최소 용량을 10으로 늘린다.|

#### Auto Scaling Groups – Predictive Scaling
- 예측 스케일링 : 지속적으로 로드 예측 및 스케줄 확장을 미리 계획한다.

### 확장에 적합한 측정 기준
- **CPUUtilization(CPU 사용률)** : 인스턴스 전체의 평균 CPU 활용률
- **RequestCountPerTarget(대상당 요청 수)** : EC2 인스턴스 당 요청 수가 안정적인지 확인
- **Average Network In/Out(평균 네트워크 입출력)** : 애플리케이션이 네트워크영역인 경우
- **Any custom metric** : CloudWatch를 사용하여 푸시

### Auto Scaling Groups - Scaling Cooldowns
- 스케일링 작업이 발생한 후 쿨 다운 타임이 발생(기본값 300초)
- 쿨 다운 타임동안 ASG는 추가 인스턴스를 시작 혹은 종료하지 않는다. (측정지표가 안정화되도록 허용)
{{<admonition tip >}}
즉시 사용할 수 있는 AMI를 사용하여 요청 시간을 단축하고 쿨 다운 타임을 단축한다.
{{</admonition>}}

## ASG for Solutions Architects
### ASG 기본 종료 정책 (simplified version)
1. 인스턴스 수가 가장 많은 AZ를 찾는다.
2. AZ에 선택할 인스턴스가 여러 개 있는 경우 가장 오래된 launch configuration을 갖는 인스턴스를 삭제
> ASG는 기본적으로 AZ 전체에서 인스턴스 수 균형을 시도 
<center><image src="/posts/images/aws/asg-default-termination-policy.png">
</center>

### ASG Lifecycle Hooks
- 기본적으로 ASG에서 인스턴스가 실행되자마자 서비스가 시작된다.
- `Pending state` : 인스턴스가 서비스 상태가 되기 전에 추가 단계를 수행할 수 있음
- `Terminating state` : 인스턴스가 종료 되기 전에 일부 작업을 수행할 수 있다.
<center><image src="/posts/images/aws/asg-lifecycle-hooks.png"></center>


### LaunchTemplate vs Launch Configuration
#### 공통사항
- Amazon Machine Image(AMI) ID, 인스턴스 유형, Key pair, 보안 그룹, EC2 인스턴스를 사용하는 기타 매개변수(tags, EC2 user-data ...)

#### Launch Configuration (legacy)
- 매번 다시 만들어야 함

#### LaunchTemplate (newer)
- 여러 버전을 가질 수 있음
- 매개 변수 하위 집합 생성 ( 재사용 및 상속을 위한 부분 구성)
- On-Demand 및 스팟 인스턴스를 모두 사용하여 프로비저닝
- T2 무제한 버스트 기능 사용 가능
- AWS 권장

