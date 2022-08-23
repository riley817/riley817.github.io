# AWS Elastic Compute Cloud (EC2)


## Amazon EC2
- EC2는 AWS에서 제공하는 가장 인기있는 서비스
- **Elastic Compute Cloud = Infrastructure as a service**
{{<admonition type=info title="구성할 수 있는 기능">}}
- 가상 머신 임대 (EC2)
- 가상 드라이브에 데이터를 저장 (EBS)
- 시스템 간 부하 분산 (ELB)
- auto-scaling group(ASG)를 사용하여 서비스 확장
{{</admonition>}}  

### EC2 sizing & configuration options
- Operating System(OS) : Linux, Windows or Mac OS
- 컴퓨터 성능과 코어양 선택 가능 (CPU)
- random-access memory (RAM)
- 스토리지 용량
    - Network-attached (EBS & EFS)
    - hardware (EC2 Instance Store)
- Network card: 네트워크 카드 속도, 공용 IP 주소
- **방화벽 규칙 : security group**
- Bootstrap script (configure at first launch): EC2 User Data

### EC2 User Data
- `EC2 User Data`를 사용하여 인스턴스를 부트스트랩 가능
- **bootstrapping**이란 머신을 시작할 때 명령을 실행하는 것을 의미한다.
- 스크립트는 인스턴스 처음 시작시 한번만 실행
- EC2 user data에서 자동화 할 수 있는 작업:
    - 소프트웨어 및 업데이트 설치
    - 인터넷으로 부터 공용파일 다운로드 등
- EC2 User Data Script는 **루트 계정**으로 실행

### EC2 인스턴스 종류
- [Amazon EC2 인스턴스 유형](https://aws.amazon.com/ko/ec2/instance-types/)
- General Purpose, Compute Optimized, Memory Optimized, Accelerated Computing, Storage Optimized, Instance Features, Measuring Instance Performance(인스턴스 성능 측정)
- AWS는 다음과 같은 네이밍 규칙을 따른다.

    > m5.2xlarge
    - `m` : 인스턴스 class
    - `5` : 세대`generation` (AWS는 시간이 지남에 따라 개선 )
    - `2xlarge` : 인스턴스 클래스 사이즈

#### 인스턴스 종류 - General Purpose 
| Instance Type     |   |
|-------------------|---|
| General Purpose   |· 웹 서버 또는 코드 저장소와같은 다양한 워크로드에 적합 <br/>· Compute, Memory, Networking 사이 밸런싱 |
| Compute Optimized |· 높은 성능을 요구하는 계산집약적인 업무에 적합<br/>· 배치 프로세스 워크로드<br/>· Media transcoding<br/>· 높은 성능의 웹서버</br>· **High performance computing(HPC)**</br>· scientific model & machine learning<br/>· 게임 전용 서버 |
| Memory Optimized  |· 메모리안에서 큰 데이터셋을 빠르게 처리하는 워크로드에 적합<br/>· 높은 성능을 요구하는 relation/non-relational databases<br/>· Distributed web scale cahce stores<br/>· BI(business intelligence)에 최적화된 in-memory databases<br/>· 크기가 큰 비정형화 데이터를 실시간으로 처리하는 애플리케이션 성능      |
| Storage Optimized  |· 로컬 스토리지의 큰 데이터의 읽고, 쓰기 접근이 요구되는 스토리지 집약 업무에 적합<br/>· Online transaction processing(OLTP) 시스템의 잦은 접근<br/>· 관계형 & NoSQL 데이터베이스<br/>· in-memory 데이터 캐시 (Redis)<br/>· Data warehousing applications<br/>· 분산 파일 시스템|

## Security Groups
- 보안그룹(Security Groups)은 AWS의 네트워크 보안의 기본
- EC2 인스턴스의 in/out 트래픽을 허용 제어하는 것 : `firewall` 처럼 동작
- 보안그룹은 **allow** 규칙만 포함
- 보안그룹의 규칙은 IP 또는 보안그룹을 참조할 수 있음
{{<admonition type=info title="They regulate:">}}
- Access to Ports
- 허가된 IP 범위 - IPv4 and IPv6
- 인바운드 네트워크 제어 (다른 인스턴스로부터)
- 아웃바운드 네트워크 제어 (다른 인스턴스로부터)
{{</admonition>}}

- 여러 인스턴스를 연결할 수 있다.
- 리젼 / VPC 단위로 차단 가능하다.
- 
