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

