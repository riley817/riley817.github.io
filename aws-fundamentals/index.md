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
- 예시 ) 방화벽, 침입 탐지 및 차단 시스템, 심층 패킷 검사 시스템, 페이로드 조작 등
- Layer 3 (네트워크 Layer)에서 작동 - IP Packets

