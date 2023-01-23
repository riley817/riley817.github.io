# 5. AWS EC2 Basics


## Amazon EC2
- EC2는 AWS의 가장 인기있는 서비스이다.
- **Elastic Compute Cloud = Infrastructure as a service**
- 클라우드의 작동 방식을 이해하기 위해서는 EC2를 이해하는 것이 중요하다.
{{<admonition info "구성할 수 있는 기능">}}
- 가상 머신 임대 (EC2)
- 가상 드라이브에 데이터를 저장 (EBS)
- 시스템 간 부하 분산 (ELB)
- auto-scaling group(ASG)를 사용하여 서비스 확장
{{</admonition>}}  

### EC2 sizing & configuration options
- **Operating System(OS)** : Linux, Windows or Mac OS
- 컴퓨터 성능과 코어양 선택 가능 (**CPU**)
- random-access memory (**RAM**)
- 스토리지 용량
    - Network-attached (**EBS & EFS**)
    - hardware (**EC2 Instance Store**)
- Network card: 네트워크 카드 속도, 공용 IP 주소
- 방화벽 규칙 : **security group**
- Bootstrap script (configure at first launch): EC2 User Data

#### EC2 User Data
- EC2 사용자 데이터 스크립트를 사용하여 인스턴스를 부트스트랩 할 수 있다.
- **부트스트래핑**은 기계가 시작될 때 명령을 시작하는 것을 의미한다.
- 스크립트는 처음 시작 할 때 한번만 실행된다.
- EC2 User Data Script는 **루트 계정**으로 실행

  {{<admonition success "EC2 사용자 데이터는 다음과 같은 부팅 작업을 자동화 할 수 있다.">}}
- 소프트웨어 및 업데이트 설치
- 인터넷으로 부터 공용파일 다운로드 등
  {{</admonition>}}

### EC2 인스턴스 종류
- [Amazon EC2 인스턴스 유형](https://aws.amazon.com/ko/ec2/instance-types/)
- AWS는 다음과 같은 네이밍 규칙을 따른다.

  {{<admonition info "AWS 네이밍 규칙">}}
**m5.2xlarge**
- `m` : 인스턴스 class
- `5` : 세대`generation` (AWS는 시간이 지남에 따라 개선 )
- `2xlarge` : 인스턴스 클래스 사이즈
  {{</admonition>}}

#### 인스턴스 종류

| Instance Type     |   |
|-------------------|---|
| **General Purpose**   |· 웹 서버 또는 코드 저장소와같은 다양한 워크로드에 적합 <br/>· Compute, Memory, Networking 사이 밸런싱 |
| **Compute Optimized** |· 높은 성능을 요구하는 계산집약적인 업무에 적합<br/>· 배치 프로세스 워크로드<br/>· Media transcoding<br/>· 높은 성능의 웹서버</br>· **High performance computing(HPC)**</br>· scientific model & machine learning<br/>· 게임 전용 서버 |
| **Memory Optimized**  |· 메모리안에서 큰 데이터셋을 빠르게 처리하는 워크로드에 적합<br/>· 높은 성능을 요구하는 relation/non-relational databases<br/>· Distributed web scale cahce stores<br/>· BI(business intelligence)에 최적화된 in-memory databases<br/>· 크기가 큰 비정형화 데이터를 실시간으로 처리하는 애플리케이션 성능      |
| **Storage Optimized**  |· 로컬 스토리지의 큰 데이터의 읽고, 쓰기 접근이 요구되는 스토리지 집약 업무에 적합<br/>· Online transaction processing(OLTP) 시스템의 잦은 접근<br/>· 관계형 & NoSQL 데이터베이스<br/>· in-memory 데이터 캐시 (Redis)<br/>· Data warehousing applications<br/>· 분산 파일 시스템|

## Security Groups

- 보안그룹(Security Groups)은 AWS의 네트워크 보안의 기본이다.
- EC2 인스턴스의 in/out 트래픽을 허용 제어하는 것
- 보안그룹은 오직 **allow** 규칙만 포함한다.
- 보안그룹의 규칙은 IP 또는 보안그룹을 참조할 수 있음

  {{<admonition type=info title="They regulate:">}}
- Access to Ports
- 허가된 IP 범위 - IPv4 and IPv6
- 인바운드 네트워크 제어 (다른 인스턴스로부터)
- 아웃바운드 네트워크 제어 (다른 인스턴스로부터)
  {{</admonition>}}

### 보안그룹에서 알아야 할 사항
- 보안그룹은 EC2 인스턴스에서 `firewall` 역할을 하고 있다.
  {{<admonition success "규제 사항">}}
  - 포트 접근 제어
  - 허가된 IP 범위 (IPv4, IPv6)
  - 인바운드 네트워크 제어 (다른 네트워크에서 인스턴스로)
  - 아웃바운드 네트워크 제어 (인스턴스에서 다른 네트워크로)
  {{</admonition>}}
  
- 여러 인스턴스에 연결할 수 있다.
- 리젼/VPC 결합으로 통제 할 수 있다.
- EC2 외부에 있는 경우 트래픽이 차단되면 EC2 인스턴스는 해당 트래픽을 인식하지 못한다.
- **SSH 접근을 위해 별도의 보안 그룹을 유지하는 것이 좋다** 
- 응용 프로그램이 액세스할 수 없는경우`time out`는 보안 그룹의 문제이다.
- 응용 프로르램에서 `connection refused` 오류가 표시되면 프로그램 오류이거나 실행되지 않은 것이다.
- 기본적으로 모든 인바운드 트래픽은 차단된다. (blocked)
- 기본적으로 모든 아웃바운드의 트래픽은 승인된다. 


  {{<admonition type=tip title="알아야 할 클래식 포트">}}
- 22 = **SSH (Secure Shell)** - 리눅스 인스턴스 로그인
- 21 = **FTP (File Transfer Protocol)** - 파일 공유를 위한 파일 업로드
- 22 = **SFTP (Secure File Transfer Protocol)** - SSH를 사용한 파일 업로드
- 80 = **HTTP** - 보안되지 않은 웹사이트 접근
- 443 = **HTTPS** - 보안 웹사이트 접근
- 3389 = **RDP(Remote Desktop Protocol)** - 윈도우 인스턴스 로그인
  {{</admonition>}}  

## EC2 인스턴스 구매 옵션
| options                         |설명   |
|---------------------------------|----|
| **On-Demand Instances**| · 애플리케이션 동작을 예측할 수 없는 **단기적**이고 **중단 없는 워크로드** 적합<br/> · 사용한 만큼만 지불<br/>&nbsp;&nbsp;&nbsp;&nbsp;· Linux or Windows : 초당 과금, 최초 1분<br/>&nbsp;&nbsp;&nbsp;&nbsp;· 다른 OS: 시간당 지불<br/> · 비용은 가장 높지만 선급금 및 계약기간 없음 |
| **Reserved Instances(예약 인스턴스)**| · On-demand에 대비 72% 할인<br/> · 특정 인스턴스 속성을 예약가능(인스턴스 타입, 지역, Tenancy, OS)<br/> · **예약 기간 1년 또는 3년**<br/> · **지불 옵션** : **No Upfront**, **Partial Upfront**, **All Upfront**<br/> · 예약인스턴스 범위 : **Regional** or **Zonal** (reserve capacity in an AZ)<br/> · 정상 상태 사용 애플리케이션에 권장<br/> · 예약된 인스턴스 마켓플레이스에서 구매 및 판매  |
| **Convertible Reserved  Instances** |  · 사용량에 대한 지불, 긴 워크로드 <br/> · 유연한 인스턴스를 통한 긴 워크로드(인스턴스 유형, 제품군, OS, 범위 및 tenancy 변경 가능)<br/> · **최대 66% 할인**  |
| **Savings Plans**                   | · 장기 사용량에 따른 할인(최대 72%)<br/> · 1년 또는 3년 동안 시간당 $10<br/> · Saving plan을 초과하는 경우 on-demand 가격으로 청구<br/> · 특정 인스턴스 제품 군 및 AWS 영역에서는 사용불가<br/> · **Flexible across:**<br/>&nbsp;&nbsp;&nbsp;&nbsp;· 인스턴스 크기(예: m5.xlarge, m5.2xlarge)<br/>&nbsp;&nbsp;&nbsp;&nbsp;· OS (e.g., Linux, Windows)<br/> &nbsp;&nbsp;&nbsp;&nbsp;· Tenancy (Host, Dedicated, Default)|
| **Spot Instance**                   | · 짧은 워크로드, 저렴한 비용으로 인스턴스 손실 가능 (신뢰성 저하)<br/> · on-demand 대비 최대 90% 할인<br/> · 최대 가격이 현재 가격보다 작을 경우 언제든지 잃어버릴 수 있는 인스턴스<br/> · **AWS에서 가장 비용 효율적인 인스턴스**<br/> · 장애에 대한 유연한 워크로드에 유용:<br/>&nbsp;&nbsp;&nbsp;&nbsp;· Batch jobs<br/>&nbsp;&nbsp;&nbsp;&nbsp;· Data analysis<br/>&nbsp;&nbsp;&nbsp;&nbsp;· Image processing<br/>&nbsp;&nbsp;&nbsp;&nbsp;· 분산 워크로드<br/>&nbsp;&nbsp;&nbsp;&nbsp;· 시작 및 종료 시간이 유연한 워크로드<br/> · **중요한 작업 또는 데이터베이스에는 적합하지 않음**|
| **Dedicated Host**                  | · 사용자 전용 물리 서버, 인스턴스 배치 제어<br/> · 준수 요구사항 해결 및 기존 서버 라이센스(per-socket, per-core, per-VM software licenses)<br/> · **구매옵션**<br/>&nbsp;&nbsp;&nbsp;&nbsp;· On-demand : 활성화 된 Dedicated host 초당 지불<br/>&nbsp;&nbsp;&nbsp;&nbsp;· Reserved : 1 또는 3년(No Upfront,Partial Upfront,All Upfront)<br/> · 복잡한 라이선스 모델(BYOL - Bring Your Own License)을 사용하는 소프트웨어 유용<br/> · 강력한 규제 또는 규정 준수 요구사항이 있는 기업     |
| **Dedicated Instances**             | · 다른 고객과 하드웨어를 공유하지 않음<br/> · 동일한 계정의 다른 인스턴스와 하드웨어를 공유할 수 있음<br/> · 인스턴스 배치를 제어할 수 없음(Start/Stop 후 이동 가능)   |
| **Capacity Reservations**           | · 특정 AZ에서 모든 기간 동안 용량 예약<br/> · 필요할 때 언제든지 EC2 용량 액세스<br/> · **No time commitment**(언제든지 생성/취소), **no billing discounts**<br/> · Regional Reserved Instances 와 Savings plans를 결합하여 청구 할인 혜택을 얻을 수 있음<br/> · 인스턴스 실행 여부에 관계없이 on-demand로 청구 <br/> · 특정 AZ에 있어야 하는 중단 없는 워크로드에 적합    |

### EC2 Spot Instance Requests
- on-demand 대비 90% 할인 가능
- **max spot price**를 지정하고 **current spot price < max** 동안 사용 
  - 시간당 스팟 가격은 제품 및 용량에 따라 다름
  - 현재 스팟 가격이 최대 가격보다 높을 경우 2분 유예 기간 동안 인스턴스를 중지 혹은 종료 가능
- 장애에 대한 탄력적인 배치작업, 데이터 분석 등의 워크로드에 사용
- **중요한 작업 또는 데이터베이스에 적합 하지 않음**

#### 다른 전략 - Spot Block
- `block` 스팟 인스턴스는 지정된 시간 범위 (1~6시간)동안 중단 없이 사용 가능 
- 드문 경우이나 인스턴스 회수 가능

#### How to terminate Spot Instances?

{{<image src="/posts/images/aws/spot_lifecycle.png" width="400px">}}
{{<image src="/posts/images/aws/spot_request_states.png" width="300px">}}

- **open**, **active** 또는 **disabled** 상태의 인스턴스만 요청 취소 할 수 있다.
- 스팟 요청을 취소해도 인스턴스는 종료되지 않는다.
- 스팟 요청을 취소한 다음 관련 스팟 인스턴스를 종료해야 한다.


- **one-time** : Spot Request가 이행되는 즉시 인스턴스가 시작
- **persistent** : Spot Request Valid from ~ Valid until 까지 유효기간 동안 요청한 개수의 인스턴스가 계속 유효. 스팟 인스턴스가 중단되어도 요청이 활성화 되어있으면 자동적으로 인스턴스를 재시작

### Spot Fleets
- **Spot Fleets = set of Spot Instances + (optional) On-Demand Instances**
- Spot Fleet은 가격 제약으로 목표 용량을 충족
  - 가능한 실행 풀 정의 : 인스턴스 유형(m5.large), OS, 가용성 영역
  - fleet을 선택할 수 있도록 여러개의 실행 풀을 가질 수 있다.
  - 용량 또는 최대 비용에 도달하면 Spot Fleets 인스턴스가 종료
- Spot Fleets를 사용하면 최저가격으로 Spot Instance를 자동으로 요청할 수 있다.

  {{<admonition type=success title="Spot Instances 할당 전략">}}
- **lowestPrice(최저가격)** : 최저 가격을 가진 pool로부터 (비용 최적화, 짧은 워크로드)
- **diversified(다양화)** : 모든 pool 분산(가용성과 긴 워크로드 적합)
- **capacityOptimized(용량 최적화)** : 인스턴스 수에 맞는 최적의 용량을 가진 풀
  {{</admonition>}}  


