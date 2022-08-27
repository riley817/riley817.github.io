# AWS Elastic Compute Cloud (EC2) - Associate


## Private vs Public IP (IPv4)
- 네트워크는 두가지 종류의 IP가 존재. IPv4, IPv6
  - IPv4 : `1.160.10.240`
  - IPv6 : `3ffe:1900:4545:3:200:f8ff:fe21:67cf`
- IPv4 대부분 온라인에서 범용적으로 사용
- IPv6 사물인터넷(IoT) 문제를 해결하기 위해 새로 등장함
- IPv4는 퍼블릭 영역에서 37억개의 서로 다른 주소를 허용
- IPv4:`[0-255].[0-255].[0-255].[0-255]`

### Private vs Public IP 차이점
#### Public IP
- Public IP는 컴퓨터가 인터넷상(WWW)에서 식별될 수 있음을 의미
- 전체 웹에서 고유해야 한다. (두 개의 머신이 같은 public IP를 갖을 수 없다)
- 쉽게 지리적 위치를 찾을 수 있음

#### Private IP
- Private IP는 컴퓨터가 사설 네트워크망에서만 식별될 수 있음을 의미
- IP는 사설 네트워크에서 고유해야 한다.
- 그러나 두 개의 다른 사설 네트워크망(두 개의 회사)에서는 같은 IP를 갖을 수 있음.
- 컴퓨터는 WWW를 연결하기 위해서 NAT + Internet gateway(a proxy)를 사용
- Private IP를 사용할 수 있는 IP 범위가 정해져 있음

### Elastic IP
- EC2 인스턴스는 재시작시 Public IP가 변경되는데 이것을 고정하고 싶으면 Elasitc IP를 사용하면 된다.
- Elasitc IP는 IPv4이며 한번에 하나의 인스턴스에만 설정 가능
- Elastic IP를 사용하면 사용자의 계정의 다른 인스턴스에 빠르게 매핑하여 인스턴스 또는 오류를 마스킹할 수 있음.
- 계정 당 5개까지 (증가 가능)

{{<admonition type=warning >}}
**전반적으로는 Ealstic IP를 사용하지 않는 것이 좋다.**
- 종종 잘못된 아키텍처를 반영하는 경우가 발생
- 대신 임의의 public IP를 사용하고 DNS 이름을 등록
- 또는 로드밸런스를 사용하고 Elastic IP를 사용하지 않는다.
{{</admonition>}}

## Placement Groups - 배치 그룹
{{<image src="/posts/images/aws/placement-groups.jpg" width="100%" caption="">}}

- EC2 인스턴스가 AWS 인프라에 배치되는 방식을 `배치 그룹(Placement Groups)` 전략을 통해 제어할 수 있다.
- **배치 그룹 전략**
| 전략 |   |
|------------------|---|
| **Cluster** | 단일 AZ에서 지연시간이 짧은 그룹으로 인스턴스를 클러스터링|
| **Spread**  | 기본 하드웨어 전체에 인스턴스 분산(AZ 그룹당 최대 7개 인스턴스)|
| **Partition** | AZ 내 여러 파티션(다양한 racks 세트 의존)에 인스턴스를 분산. 그룹당 100개의 EC2 인스턴스로 확장(Hadoop, Cassandera, Kafka)|

### Cluster 배치 그룹
- **장점** : 뛰어난 네트워크(향상된 네트워크 활성화된 인스턴스간 10Gbps 대역폭 - 권장)
- **단점** : 랙이 실패하면 모든 인스턴스가 동시에 실패
- **사용사례** :
  - 빠른 처리가 필요한 빅데이터 업무
  - 매우 짧은 지연시간과 높은 네트워크 처리량이 필요한 애플리케이션

### Spread 배치 그룹
- **장점** 
  - AZ 걸쳐 확장 가능
  - 동시 장애로 인한 위험 감소
  - EC2 인스턴스가 다른 물리적 하드웨어있음
- **단점** : 배치 그룹 AZ 당 7개의 인스턴스 제한
- **사용사례**:
  - 고가용성을 극대화해야 하는 애플리케이션
  - 각 인스턴스가 서로의 장애로부터 격리되어야 하는 중요 애플리케이션

### Partition 배치 그룹
- AZ 당 최대 7개의 파디션
- 같은 리젼에서 여러 AZ를 걸쳐 확장가능
- 최대 100개의 EC2 인스턴스
- 파티션 내 인스턴스는 다른 파디션 내 인스턴스와 랙을 공유하지 않는다.
- 파티션 오류는 많은 EC2에게 영향을 줄 수있으나 다른 파티션에는 영향을 미치지 않음
- EC2 인스턴스는 파티션 정보에 메타데이터로 액세스 가능
- **사용사례** : HDFS, HBase, Cassandra, Kafka

## Elastic Network Interfaces (ENI)
- **가상 네트워크 카드**를 나타내는 VPC의 논리적 구성요소
- ENI를 독립적으로 생성하고 장애조치를 위해 EC2 인스턴스에서 즉이 연결(이동) 가능
- 특정 AZ에 바인딩 

{{<admonition type=success title="ENI 구성요소" >}}
- 기본 private IPv4, 하나 이상의 secondary IPv4
- private IPv4 당 하나의 Elastic IP(IPv4) 
- 하나의 Public IPv4
- 하나 이상의 보안 그룹
- MAC Address
{{</admonition>}}

{{<admonition type=info title="ENI 구성요소" >}}
**인스턴스를 중지, 종료 했을 때**
- **Stop** : 데이터 디스크(EBS)는 다음 인스턴스 재시작까지 유지
- **Terminate** : EBS 볼륨(루트)이 삭제되게 설정했다면 삭제

**인스턴스를 시작했을 때**
- **First start** : OS boots & EC2 인스턴스 Data script 실행
- **Following starts** : OS boots up
- 애플리케이션 시작 및 캐시 준비

{{</admonition >}}

## EC2 Hibernate
- in-memory(RAM) 상태가 유지된다.
- 인스턴스 부팅이 더 빠르다. (OS가 중지/재시작 되지 않음)
- Under the hood: RAM 상태가 루트 EBS 볼륨 파일에 기록된다.
- 루트 EBS 볼륨을 암호화 해야 한다.
- **사용 사례** 
  - 오래 실행되는 프로세스
  - RAM의 상태를 저장하고 싶을 때
  - 서비스 초기화 시간에 이용

### EC2 Hibernate(절전모드) 알아야 할 내용
- **지원 되는 인스턴스 제품군** : C3, C4, C5, I3, M3, M4, R3, R4,T2,T3, ...
- **인스턴스 RAM Size** : 반드시 150 GB 미만
- **인스턴스 Size** : bare metal 인스턴스에는 적용 불가
- **AMI** : Amazon Linux 2, Linux AMI, Ubuntu, RHEL, CentOS & Windows...
- **Root Volume** : EBS만 가능. 암호화가 필요. 충분한 용량이어야 하며 instance store 사용할 수 없음.
- **On-Demand**, **Reserved**, **Spot** 인스턴스 가능
- 인스턴스는 60일 이상 최대 절전모드로 전환 불가

## EC2 Nitro
- 차세대 EC2 인스턴스를 위한 기본 플랫폼
- 새로운 가상화 기술
- **성능 향상**
  - 향상된 네트워킹 옵션(향상된 네트워킹, HPC, IPv6)
  - **고속 EBS(Nitro는 64,000 EBS IOPS 필요 - Nitro가 아닌 경우 최대 32,000)**
- 기본 보안 향상
- 인스턴스 유형
  - Virtualized : A1,C5,C5a,C5ad,C5d,C5n,C6g,C6gd,C6gn,D3,D3en,G4,I3en,Inf1,M5, M5a, M5ad, M5d, M5dn, M5n, ....
  - Bare metal : a1.metal, c5.metal, c5d.metal, c5n.metal, c6g.metal, c6gd.metal...

## vCPU 이해하기
- 다중 스레드를 CPU에서 실행할 수 있다. `multithreading`
- 각 스레드는 가상 CPU(vCPU)로 표시
- Example: `m5.2xlarge`
  - 4 CPU, 2 Thread per CPU => 8 vCPU in total


### EC2 Optimizing CPU 옵션
- EC2 인스턴스는 RAM과 vCPU의 조합으로 함께 제공
- 경우에 따라 vCPU 옵션을 변경 할 수 있다.
| options   |
|---------------------------------|---|
| **of CPU cores**|· 높은 RAM과 적은 수의 CPU가 필요한 경우 유용<br/>· 라이센스 비용 절감|
| **of threads per core**|· CPU 당 스레드 수가 1 개인 멀티스레딩 비활성화 - HPC 워크로드에 유용 |
- 인스턴스 실행 중에만 지정

### EC2 Capacity Reservations
- 용량 예약을 통해 필요할 때 EC2 용량 확보
- 예약에 대한 수동 또는 계획된 종료 일자
- 1년 또는 3년 약정 불필요
- Capacity 액세스는 즉시 이루어지며 시작하자마자 청구
- 예약된 인스턴스 및 절감 계획과 결합하여 비용 절감
#### 지정 사항
- 용량을 예약할 AZ(한개만)
- 용량을 예약할 인스턴스 수
- 인스턴스 유형, tenancy 및 플랫폼/OS를 포함한 인스턴스의 특성
{{<image src="/posts/images/aws/capacity-reservation.png" width="100%" caption="">}}

