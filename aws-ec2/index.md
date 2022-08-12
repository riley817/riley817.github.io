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
- EC2 User Data Script는 루트 계정으로 실행

### EC2 인스턴스 종류
- [Amazon EC2 인스턴스 유형](https://aws.amazon.com/ko/ec2/instance-types/)
- AWS는 다음과 같은 네이밍 규칙을 따른다.

    > m5.2xlarge
    - `m` : 인스턴스 class
    - `5` : 세대`generation` (AWS는 시간이 지남에 따라 개선 )
    - `2xlarge` : 인스턴스 클래스 사이즈
    
