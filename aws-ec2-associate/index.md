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

## EC2 Hibernate
{{<admonition type=info title="인스턴스를 stop, terminate 했을 때" >}}
- **Stop** : 데이터 디스크(EBS)는 다음 인스턴스 재시작까지 유지
- **Terminate** : EBS 볼륨(루트)이 삭제되게 설정했다면 삭제
{{</admonition>}}

{{<admonition type=success title="인스턴스를 stop, terminate 했을 때" >}}
- **First start** : OS boots & EC2 인스턴스 Data script 실행
- **Following starts** : OS boots up
{{</admonition>}}
