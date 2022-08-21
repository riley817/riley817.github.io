# AWS Virtual Private Cloud(VPC)


## CIDR - IPv4
- **Classless Inter-Domain Routing** - IP 주소 할당 방법
- **Security Groups** 규칙 및 AWS 네트워킹에 일반적으로 사용
- **Base IP** :
    - 범위에 포함된 IP를 나타냄
    - Example) 10.0.0.0, 192.168.0.0 ...
- **Subnet Mask** :
    - IP에서 변경할 수 있는 비트 수를 정의
    - Example) /0, /24, /32
        - **/8** : 255.0.0.0  
        - **/16** : 255.255.0.0
        - **/24** : 255.255.255.0
        - **/32** : 255.255.255.255

{{<admonition tip "Subnet Mask">}}
Subnet Mask 사용하면 기본적으로 기본IP에서 다음 값을 추가로 가져올 수 있다.
- [IPLocationTools](https://www.ipaddressguide.com/cidr) 

| IP/Subnet Mask   |                    ||
|----------------|------------------|--------------------|
| 192.168.0.0/32 | allows for 1 IP(2<sup>0</sup>)|192.168.0.0|
| 192.168.0.0/31 | allows for 2 IP(2<sup>1</sup>)|192.168.0.0 ~ 192.168.0.1|
| 192.168.0.0/30 | allows for 4 IP(2<sup>2</sup>)|192.168.0.0 ~ 192.168.0.3|
| 192.168.0.0/29 | allows for 8 IP(2<sup>3</sup>)|192.168.0.0 ~ 192.168.0.7|
| 192.168.0.0/28 | allows for 16 IP(2<sup>4</sup>)|192.168.0.0 ~ 192.168.0.15|
| 192.168.0.0/27 | allows for 32 IP(2<sup>5</sup>)|192.168.0.0 ~ 192.168.0.31|
| 192.168.0.0/26 | allows for 64 IP(2<sup>6</sup>)|192.168.0.0 ~ 192.168.0.65|
| 192.168.0.0/25 | allows for 128 IP(2<sup>7</sup>)|192.168.0.0 ~ 192.168.0.127|
| 192.168.0.0/24 | allows for 256 IP(2<sup>8</sup>)|192.168.0.0 ~ 192.168.0.255|
| 192.168.0.0/16 | allows for 65,536IP(2<sup>16</sup>)|192.168.0.0 ~ 192.168.255.255|
| 192.168.0.0/0  | allows for All IP|0.0.0.0 ~ 255.255.255.255|
{{</admonition>}}

## Public vs Private IP (IPv4)
- IANA(Internet Assigned Number Authority)은 개인(LAN) 및 공용(Internet) 주소 사용을 위해 IPv4 주소의 특정 블록을 설정함
- **Private IP는 특정 값만 허용 가능**
    - `10.0.0.0 – 10.255.255.255 (10.0.0.0/8)` : big networks
    - `172.16.0.0 – 172.31.255.255 (172.16.0.0/12)` : AWS defaultVPC in that range
    - `192.168.0.0 – 192.168.255.255 (192.168.0.0/16)` : e.g., home networks
- 인터넷에 있는 나머지 IP는 모두 public IP

## Default VPC
- 모든 AWS 계정은 default VPC를 갖는다.
- 새로운 EC2 인스턴스는 subnet을 지정하지 않으면 기본 VPC로 시작
- 기본 VPC는 인터넷에 연결되어 있고 public IPv4 주소를 가지고 있음
- public 및 private IPv4 와 DNS 이름을 얻을 수 있음

## VPC in AWS 
- Virtual Private Cloud (VPC)
- AWS 리전에 여러 VPC를 만들 수 있다. (리전당 VPC는 최대 5개)
- VPC당 CIDR 
    - Min size /28 (16 IP Address)
    - Max Size /16 (65,536 IP Address)
- **VPC 비공개 전용이므로 Private IPv4 범위만 허용**
    - 10.0.0.0 – 10.255.255.255 (10.0.0.0/8)
    - 172.16.0.0 – 172.31.255.255 (172.16.0.0/12)
    - 192.168.0.0 – 192.168.255.255 (192.168.0.0/16)
> VPC CIDR는 다른 네트워크(e.g., corporate)와 겹치지 않아야 한다.

### Subnet
- AWS는 **각 서브넷에 5개의 IP 주소를 예약(처음 4개 & 마지막 1개)**
- 위 IP는 사용할 수 없으며 EC 인스턴스 할당 불가

{{<admonition tip "서브넷 예약 IP">}}
- **10.0.0.0** – Network Address
- **10.0.0.1** – AWS에 의해 VPC Router 용으로 예약
- **10.0.0.2** – Amazon가 제공하는 DNS에 매핑하기 위해 AWS에 의해 예약
- **10.0.0.3** – 나중에 사용을 위한 
- **10.0.0.255** - Network Broadcast Address. AWS는 VPC에서 브로드캐스드를 지원하지 않으므로 해당 주소가 예약되어있음
{{</admonition>}}

{{<admonition success "Exam Tip: EC2 인스턴스에 29개의 IP 주소가 필요한 경우">}}
- 크기가 /27인 서브넷은 선택할 수 없다 (/27 -> 2<sup>5</sup>=32 - 5 = 27 -> 27 < 29) 
- 크기가 /26인 서브넷을 선택해야 한다. (/26 -> 2<sup>6</sup>=64 - 5 = 59 -> 59 > 29)
{{</admonition>}}

### Internet Gateway (IGW)
- VPC 리소스(e.g., EC2 인스턴스)를 인터넷에 연결할 수 있다.
- 수평으로 확장 가능하며 높은 가용성과 이중화`redundant` 제공
- **VPC와 별도로 생성해야 한다.**
- 하나의 VPC는 하나의 IGW에만 연결할 수 있다. (그 반대의 경우도 마찬가지)
- **인터넷 게이트 웨이 자체에서도 인터넷 액세스를 허용하지 않는다 - Route Tables을 반드시 수정해야 한다.**

### Bastion Hosts
- `Bastion Host`를 사용하여 Private EC2 인스턴스로 SSH 할 수 있다.
- Bastion은 public 서브넷에 위치하며 다른 모든 private 서브넷과 연결 된다.
- **Bastion Host의 security group은 매우 엄격해야한다** 

{{<admonition tip "Exam Tip" >}}
- 다른 EC2 인스턴스 Security Group이 아닌 사용자가 필요한 IP 주소의 포트 22만 Bastion host에서 가능한지 확인합니다.
{{</admonition>}}

### NAT Instance (outdated, 시험에는 나옴)
{{<image src="/posts/images/aws/nat-instance.jpg" >}}

- **NAT (Network Address Translation)**
- private 서브넷의 EC2 인스턴스를 인터넷에 연결하게 할 수 있다.
- 반드시 public 서브넷에서 실행
- **반드시 NAT EC2 생성 시 해당 체크 해제 -> Source/destination**
- Elastic IP가 연결되어 있어야 한다.
- private 서브넷에서 NAT 인스턴스로 라우팅하도록 Route Tables 구성해야 한다.
- 미리 구성된 Amazon Linux AMI를 사용할 수 있음 
- 2020년 12월 31일 지원 종료
- 가용성이 높지 않고 초기화 설정으로 복원 불가능
    - Multi AZ에 대한 ASG 생성 + 복원되는 사용자 데이터 스크립트 필요
- 인터넷 트래픽 대역폭은 EC2 인스턴스 유형에 따라 다름
- Security Group & rules 관리
    - **인바운드**
        - HTTP/HTTPS: Private Subnet 허용
        - SSH: 홈 네트워크(Internet Gateway를 통해 액세스 제공)
    - **아웃바운드**
        - 인터넷에 대한 모든 HTTP/HTTPS 트래픽 허용



### NAT Gateway
{{<image src="/posts/images/aws/nat-gateway.jpg" >}}

- AWS에서 관리되며 높은 대역폭, 높은 가용성, 관리가 필요없음
- 사용량 및 대역폭에 따라 시간당 비용지불
- `NATGW`는 특정 AZ에 생성되며 Elastic IP를 사용
- 동일한 서브넷의 EC2 인스턴스에서는 사용할 수 없음 (오직 다른 서브넷에서만)
- **Internet Gateway가 필요 (Private subnet => NAT Gateway => Internet Gateway)**
- 5Gpbs 대역폭 최대 45Gbps 까지 자동 확장
- Security Groups 관리가 필요 없음

#### NAT Gateway with High Availability
{{<image src="/posts/images/aws/nat-gateway-ha.jpg" >}}
- **NAT Gateway는 단일 AZ내에서 탄력적임**
- **`fault-tolerance`을 위해 multiple AZ에 multiple NAT Gateways를 구성**
- AZ가 중단될 경우 NAT가 필요하지 않으므로 AZ failover가 필요하지 않음

### NAT Gateway vs NAT Instance
|              | NAT Gateway | NAT Instance |
|--------------|-------------|--------------|
| **Availability**<br/>가용성 | AZ내 높은 가용성(다른 AZ생성)|스크립트를 사용하여 인스턴스간 failover 관리(가용성이 높진않음)|
|**Bandwidth**|Up to 45 Gbps|EC2 타입에 의존|
|**Maintenance** |AWS에서 관리|사용자가 관리 (e.g., software, OS patches ...)|
|**Cost**|시간 당 & 전송된 데이터 양|시간 당, 인스턴스 타입에 따른 사이즈 + 네트워크 $|
|**Public IPv4**|O|O|
|**Private IPv4**|O|O|
|**Security Groups**|X|O|
|**Use as Bastion Host?**|X|O|

## DNS Resolution in VPC

### DNS Resolution (enableDnsSupport)
{{<image src="/posts/images/aws/dns-resolution.png" >}}
- VPC에 대해 Route 53 Resolver server의 DNS 확인(DNS Resolution)이 지원되는지 여부를 결정
- 기본값 : True
    - Amazon이 제공하는 DNS 서버(169.254.169.253) 또는 VPC IPv4 reserved IP (x.x.x.2)를 쿼리하게 된다.

### DNS Hostname (enableDnsHostnames)
- 기본적으로
    - True : default VPC
    - False : 새로 생성된 VPC
- `enableDnsSupport=true` : 아무것도 하지 않음
- `enableDnsHostnames=true` : EC2 인스턴스에 공용 IPv4가 있는 경우 공용 호스트 이름을 할당

{{<admonition tip>}}
Route 53의 `Private Hosted Zone`에서 사용자 지정 DNS 도메인 이름을 사용하는 경우 두모든 속성을 true로 지정 `enableDnsSupport=true` & `enableDnsHostname=true` 
{{</admonition>}}

## Security Groups & NACLs

### Network Access Control List (NACL)
- NACL은 서브넷에서 송수신되는 트래픽을 제어하는 방화벽과 같다.
- **서브넷 당 하나의 NACL, 새로운 서브넷은 Default NACL에 할당된다.**
- 새로 생성된 NACL은 모든 것을 거부
- NACL은 서브넷 수준에서 특정 IP 주소를 차단하는 좋은 방법

#### NACL 규칙
- 규칙에는 숫자(1-32766). 낮은 숫자가 우선순위가 높다.
{{<admonition tip>}}
10.0.0.10 해당 주소는 ALLOW
- #100 ALLOW 10.0.0.10/32
- #200 DENY 10.0.0.10/32
{{</admonition >}}
- 마지막 규칙은 asterisk(*)이며 규칙이 매칭되지 않는 경우 요청을 거부
- AWS는 100 단위로 규칙을 증가하는 것을 추천

#### Default NACL
- 연결된 서브넷과 함께 모든 인바운드/아웃바운드를 수락
- 절대 Default NACL을 수정하지 말고 사용자 정의 NACL을 작성할 것

#### Ephemeral Ports
- 두 endpoint 연결을 설정하려면 포트를 사용해야 한다.
- 클라이언트는 **정의된 포트**에 연결하고 `ephemeral port`로 응답값을 받기를 원한다.
- 운영체제에 따라 포트범위가 다르다
    - **IANA & MSWindows10** : 49152 - 65535
    - **Many Linux Kernels** : 32768 - 60999

### Security Group vs. NACLs
| Security Group | NACL |
|----------------|------|
|인스턴스 수준에서 작동|서브넷 수준에서 작동|
|허용 규칙만 지원|allow, deny 규칙 지원|
|**Stateful**:반환 트래픽은 규칙과 관계없이 자동으로 허용|**Stateless**:반환 트래픽은 규칙에 의해 병시적으로 허용 - think of ephemeral ports|
|트래픽을 허용할지 여부를 결정하기 전에 모든 규칙 평가|트래픽 허용 여부를 결정할 때 규칙을 순서대로 평가(우선순위가 높은 첫번째 일치가 적용)|
|다른 사용자가 지정한 경우 EC2 인스턴스에 적용|연결된 서브넷의 모든 EC2 인스턴스에 자동으로 적용|

### VPC – Reachability Analyzer

- VPC 두 endpoint 간 네트워크 연결 문제를 해결하는 네트워크 진단 도구
- 네트워크 구성의 모델을 만든 다음 이러한 구성을 기반으로 도달 가능성을 확인 **(패킷 전송은 하지 않음)**
- 사용 사례 : 연결문제 해결, 네트워크 구성이 의도대로인지 확인 등...

#### 대상에 따른 분석
- **Reachable** : 가상 네트워크 경로에 대한 `hop-by-hop` 세부 정보 생성
- **Not reachable** : 차단 구성요소 (e.g., SG, NACL, Route Tables 구성 문제)를 식별

