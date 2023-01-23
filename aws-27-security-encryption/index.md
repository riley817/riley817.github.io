# 27. AWS Security & Encryption


## 전송중 암호화 (SSL)
- 데이터를 전송하기 전에 암호화. 수신 후 복호화
- SSL 인증서로 암호화 (HTTPS)
- 전송 중 암호화는 MITM(man in the middle attack)이 발생하지 않도록 보장

## Server side encryption at rest
- 데이터가 서버에 수신 된 후 암호화
- 데이터가 서버에 전송 전 복호화
- 데이터 키라고 불리우는 키 덕분에 데이터는 암호화 된 형태로 저장
- 암호화 및 복호화 키는 어딘가에 관리되어야 하며 서버는 이에 대한 액세스 권한이 있어야 한다.

{{<image src="/posts/images/aws/server-side-encryption.jpg">}}

## Client side encryption
- 데이터가 클라이언트에 의해 암호화되고 서버는 복호화할 수 없음
- 데이터는 수신 클라이언트에 의해 복호화
- 서버는 데이터의 암호를 복호화 할 수 없음
- **Envelop Encryption** 암호화를 활용

{{<image src="/posts/images/aws/client-side-encryption.jpg" >}}

## AWS KMS (Key Management Service)
- KMS 키를 사용하여 데이터에 액세스할 수 있는 사람과 대상을 쉽게 제어
- 인증을위한 IAM과 완벽하게 통합가능
- CLI / SDK 사용 가능

- 다음의 서비스와 완벽하게 통합:
    - **Amazon EBS**: 볼륨 암호화
    - **Amazon S3** : Server side encryption of objects
    - **Amazon Redshift**: encryption of data
    - **Amazon RDS**: encryption of data
    - **Amazon SSM**: Parameter store 
    - 그외...

### Customer Master Key (CMK) Types

#### 대칭키 Symmetric (AES-256 Key) 
- 암호화 및 복호화가 동일
- AWS는 KMS와 통합된 Symmetric CMKs를 사용
- `envelope encryption`에 필요
- 암호화되지 않은 키에 액세스 할 수 없음 - 사용하려면 KMS API 호출 필요

#### 비대칭키 Asymmetric (RSA & ECC Key pairs) 
- SSL 작동 원리
- 암호화 시 public key 복호화 시 private key 사용
- 암호화/복호화 및 Sing/Verify 작업에 사용
- public key를 다운로드할 수 있지만 private key에는 액세스 할 수 없음
- 사용 사례
    - **KMS API를 호출할 수 없는 사용자**에 의한 AWS 외부 암호화

### KMS 대칭키 
- KMS Symmetric Key 키 정책
    - 생성정책
    - Rotation policies - 키 교체 정책
    - 활성화 및 비활성화
- **CloudTrail**을 사용하여 키 사용을 감사
- Customer Master Keys(CMS) 유형
    1. AWS Managed Service Default CMK: free
    2. User Keys created in KMS: $1 / month
    3. User Keys imported (must be 256-bit symmetric key): $1 / month
    - Call KMS API ($0.03 / 10000 calls)

### AWS KMS 101
- 데이터베이스 패스워드
- 외부 서비스의 자격증명
- SSL 인증서의 private key

{{<admonition tip>}}
KMS는 데이터를 암호화하거나 복호화하는 키를 볼 수 없기 때문에 AWS의 전체 보안이 가능 
{{</admonition>}}

- **Secret 데이터는 플레인텍스트로 특히 코드로 저장하지 않는다.**
- **KMS 요청당 4KB의 데이터를 암호화하는데 효율** - 데이터가 4KB 큰 경우 envelop encryption 사용

{{<admonition note "다른 사용자에게 KMS 권한을 부여하려면">}}
- 키 정책이 사용자를 허용하는지 확인한다.
- IAM 정책이 API를 호출을 허용하는지 확인한다.
{{</admonition>}}


### Copying Snapshots across regions 
- KMS 키는 특정 리전에 바인딩 된다. 
    - 리전 A에서 KMS를 생성하면 리전 B로 전송 불가능

1. KMS로 암호화된 EBS 볼륨
2. 동일한 키로 암호화 된 EBS 스냅샷 생성
3. 다른 리전에 KMS 키로 재 암호화
4. 새로운 리전에 새로운 키로 암호화 된 스냅샷생성
5. 해당 스냅샷으로 볼륨을 재생성 

### KMS 키 정책
- KMS 키에 대한 액세스 제어는 S3 Bucket 정책과 비슷
- 차이점 : 사용자 없이 액세스 제어 불가

| 기본 KMS 키 정책 |Custom KMS 키 정책  |
|------------|---|
|- 특정 키 정책을 사용하지 않으면 기본으로 생성 | - KMS 키에 액세스할 수 있는 사용자 및 role 정의   |
|- 루트 사용자 = 전체 AWS 계정에 대한 전체 치 액세스 권한 | - 키를 관리할 수 있는 사용자 정의  |
|- KMS 키에 대한 IAM 정책에 대한 액세스 권한 부여 | - KMS 키의 계정간 액세스에 유용   |

#### Copying Snapshots across accounts

1. 사용자의 CMK로 암호화 된 스냅샷 생성
2. KMS 키 정책을 첨부하여 계정간 계정 액세스 권한 부여
3. 암호화 된 스냅샷 공유
4. 스냅샷 복사본 생성 계정의 KMS 키로 암호화
5. 스냅샷 볼륨 생성

### KMS Automatic Key Rotation
- **Customer-managed(CMK)에서만** (not AWS managed CMK)
- 사용할 경우 **1 년마다** 키가 rotation
- 이전 데이터들이 복호화 될 수 있도록 이전키의 상태는 활성화
- 새 키의 CMK ID는 동일하다. (Backing Key만 변경)

### KMS Manual Key Rotation
- 90일, 180일 등 원하는 간격으로 rotate 할 경우
- **새 키의 CMK ID가 다르다**
- 이전 데이터들이 복호화 될 수 있도록 이전키의 상태는 활성화
- `UpdateAlias` API를 사용하여 별칭을 사용하는 것이 좋다.
- 자동 키 로테이션 CMK(비대칭 CMK 같은)에 적합하지 않은 경우 좋은 대안

## SSM Parameter Store
- 구성 및 기밀 데이터를 위한 안전한 스토리지
- KMS를 사용한 원활한 암호화(옵션)
- Serverless, 확장성, 내구성, 손쉬운 SDK
- 버전 트래킹
- path & IAM을 사용한 구성 관리
- `CloudWatch` 이벤트 알림
- `CloudFormation`과 통합

### SSM Parameter Store Hierarchy
{{<image src="/posts/images/aws/ssm-hierarchy.jpg" >}}

### Parameters Policies (for advanced parameters)
- 암호같은 중요한 데이터를 강제로 업데이트하거나 삭제하기 위해 만료일에 TTL을 적용 
- 한번에 여러 정책을 할당 가능

{{<admonition abstract "파라미터 정책">}}
**Expiration(파라미터 삭제)**
```json
{
    "Type": "Expiration",
    "Version" : "1.0",
    "Attributes": {
        "Timestamp" : "2020-12-02T21:34:33.000Z"
    }
}
```
**만료 알림(CloudWatch Events)**
```json
{
    "Type": "Expiration",
    "Version" : "1.0",
    "Attributes": {
        "Timestamp" : "2020-12-02T21:34:33.000Z"
    }
}
```

**NoChangeNotification(CloudWatch Events)**
```json
{
    "Type": "NoChangeNotification",
    "Version" : "1.0",
    "Attributes": {
        "After": "20",
        "Unit": "Days"
    }
}
```
{{</admonition>}}

## AWS Secret Manager
- 기밀 저장을 위한 새로운 서비스
- **X 일마다 강제로 기밀을 로테이션 하는 기능**
- **Amazon RDS** (MySQL, PostgreSQL, Aurora) 통합
- KMS을 사용한 암호화
- 대부분의 RDS 통합을 위함

## CloudHSM
- KMS는 AWS에서 암호화를 위한 소프트웨어 관리
- CloudHSM => AWS에서 암호화 **하드웨어** 프로비저닝
- 전용 하드웨어 (HSM = Hardware Security Module)
- AWS가 아닌 자신의 암호화 키를 전체적으로 관리해야 한다.
- HSM는 변조 방지, FIPS 140-2 레벨 3 준수
- 대칭 및 비대칭 암호화 (SSL/TLS 키)모두 지원
- 프리티어 아님
- CloudHSM Client Software를 사용해야 함
- Redshift는 데이터베이스의 암호화와 키 관리를 위해 CloudHSM를 지원
- **SSE-C 암호화와 함께 사용하기 좋은 옵션**

### CLoudHSM Diagram
- **IAM Permissions** 
    - CRUD an HSM Cluster
- **CloudHSM software**
    - Manage the Keys
    - Manage the Users

### CLoudHSM - High Availability
- CloudHSM 클러스터는 Multi AZ(HA) 분산된다.
- 뛰어난 가용성 및 내구성을 갖는다.

## CloudHSM vs. KMS
| 기능   |      AWS KMS       |  AWS CloudHSM |
|--------|-------------|-------------|
|Tenancy  |Multi-Tenant|Single-Tenant|
|Standard|FIPS 140-2 Level 2|FIPS 140-2 Level 3|
|Master Keys|· AWS Owned CMK<br/>· AWS Manged CMK</br>· Customer Managed CMK|Customer Managed CMK|
|Key Types  |· Symmetric<br/>· Asymmetric<br/>· Digital Signing|· Symmetric<br/>· Asymmetric<br/>· Digital Signing & Hashing|
|Key Accessibility  |여러 AWS 리젼에서 액세스 가능|· VPC에서 배포 및 관리<br/>· VPC 간에 공유 가능(VPC 피어링)|
|Cryptographic Acceleration  |None|· SSL/TLS Acceleration<br/>• Oracle TDE Acceleration|
|Access & Authentication |AWS IAM|사용자를 생성하고 권한을 관리|
|High Availability  |AWS Managed Service|서로 다른 AZ에 여러 HSM 추가|
|Audit Capability  |· CloudTrail<br/>· CloudWatch|• CloudWatch<br/>• CloudTrail<br/>• MFA support|
|Free Tier  |Yes|No|

## AWS Shield
### AWS Shield Standard
- 무료 서비스
- SYN/UDP Floods, Reflection, 네트워크 Layer3/Layer4 공격과 같은 기타 공격으로 부터 보호 제공

### AWS Shield Advanced
- 선택적인 DDoS 완화 서비스 ($3,000 per month per organization)
- **Amazon EC2, Elastic Load Balancing(ELB), Amazon CloudFront, AWS Global Accelerator 및 Route 53**에 대한 보다 정교한 공격으로부터 보호
- AWS DDoS 대응팀(DRP) 연중무휴 액세스
- DDoS로 인해 사용량이 급증하는 동안 더 높은 수수료로부터 보호

## AWS WAF - Web Application Firewall
- 일반적인 웹 악용으로 부터 웹 응용 계층 보호 (Layer 7)
- **Layer 7 is HTTP** (vs Layer 4 is TCP)
- Deploy on **Application Load Balancer, API Gateway, CloudFront**

### Define Web ACL (Web Access Control List)
- 규칙에는 IP address, HTTP headers, HTTP body, URI strings 포함할 수 있다.
- 일반적인 공격으로 부터 보호 
    - SQL injection
    - Cross-Site Scripting (XSS)
- 크기 제약, **지리적 일치(블록 국가)**
- DDoS 보호를 위한 속도기반 규칙 (이벤트 발생 횟수 계산)

### AWS Firewall Manager
- **AWS 조직의 모든 계정 규칙을 관리**
- 공통 보안 규칙 집합
- WAF 규칙 : (Application Load Balancer, API Gateways, CloudFront) 
- AWS Shield Advanced (ALB, CLB, Elastic IP, CloudFront)
- VPC의 EC2 및 ENI 리소스에 대한 Security Group  

## Amazon GuardDuty
- AWS 계정 보호를 위한 지능형 위협 검색
- 머닝 러신 알고리즘, 이상 징후 감지, 3rd party 데이터 이용
- 클릭 한 번으로 활성화 (30일 평가판), 소프으웨서 설치 불필요
- 입력에는 다음과 같은 데이터가 포함
    - **CloudTrail Events Logs** : 비정상적인 API 호출, 무단 배포 
        - **CloudTrail Management Events** : VPC 서브넷 생성, create trail 등
        - **CloudTrail S3 Data Events** : GetObject, ListObject, DeleteObject 등
    - **VPC Flow** : 비정상적인 내부 트래픽, 비정상적인 IP 주소
    - **DNS Log** : DNS 쿼리 내 암호화된 데이터를 전송하는 EC2 인스턴스의 손상
    - **Kubernetes Audit Log** : 의심스러운 활동과 EKS 클러스터 손상 가능성
- 발견 시 알림을 받을 **CloudWatch Event rules** 설정 가능
- CloudWatch Events rule은 AWS Lambda or SNS를 대상으로 할 수 있다
- `CryptoCurrency Attacks` 공격으로부터 보호할 수 있음
{{<image src="/posts/images/aws/guard-duty.png" >}}

## Amazon Inspector
- 자동화된 보안 평가

{{<admonition tip>}}
**For EC2 instance**
- AWS System Manager(SSM) agent 활용
- 의도하지 않은 네트워크 액세스 가능성에 대한 분석
- 실행중인 OS의 알려진 취약성 분석 

**For Containers push to Amazon ECR**
- ECR 컨테이너에 push에 대한 분석

{{</admonition>}}
- AWS Security Hub와 보고 및 통합
- Amazon Event Bridge 결과 전송

### AWS Inspector가 평가하는 것들
- **EC2 인스턴스 및 컨테이너 infrastructure 만 해당**
- 필요한 경우에만 인프라의 지속적인 검색
- 패키지 취약성 (EC2 & ECR) - CVE 데이터베이스
- Network reachability (EC2)
- 위험 점수는 우선순위 지정에 대한 모든 취약성과 연결

## Amazon Macie
- 머신러닝과 패턴 매칭을 활용해 AWS의 중요한 데이터를 검색 및 보호하는 완전 관리형 데이터 베이스 보안 및 프라이버시 서비스
- **개인 식별 가능 정보 (personally identifiable information-PII)와 같은 중요한 데이터를 식별하고 경고**하도록 도와줌

## AWS 공유적 책임 모델

- **AWS의 책임**
    - 클라우드의 보안
    - 모든 AWS 서비스를 실행하는 인프라(hardware, software, facilities, and networking) 보호
    - S3, DynamoDB, RDS 등과 같은 관리형 서비스

- **고객의 책임**
    - 클라우드 내 보안
    - EC2 인스턴스의 경우 고객은 게스트 OS(보안 패치 및 업데이트 포함), 방화벽 및 네트워크 구성, IAM을 관리해야 함
    - 애플리케이션 데이터 암호화

- **공유** : 패치 관리, 구성 관리, 인식 및 교육

### RDS에서 책임모델

- **AWS의 책임**
    - 근본적인 EC2 인스턴스 관리 SSH 액세스
    - 자동화된 DB 패치 
    - 자동화된 OS 패치
    - 근본적인 인스턴스 및 디스크 감사 및 기능 보장
- **고객의 책임**
    - DB Security Group의 포트 / IP / 인바운드 규칙 확인
    - 데이터베이스 내의 사용자 생성 및 권한
    - 공용 접근 권한 유무 관계없이 데이터베이스 작성
    - 매개 변수 그룹 또는 DB가 SSL 연결만 허용하도록 구성되었는지 확인
    - 데이터베이스 암호화 설정

### S3에서 책임모델
- **AWS의 책임**
    - 무제한 스토리지의 제공을 보장
    - 암호화 제공을 보장
    - 서로 다른 고객간의 데이터 분리 보장
    - AWS 직원이 데이터에 액세스 할 수 없도록 보장
- **고객의 책임**
    - Bucket의 구성
    - Bucket 정책 / 공용 설정
    - IAM 사용자 및 역할
    - 암호화 사용

### Shared Responsibility Model diagram

{{<image src="/posts/images/aws/Shared_Responsibility_Model_V2.59d1eccec334b366627e9295b304202faf7b899b.jpg" caption="https://aws.amazon.com/ko/compliance/shared-responsibility-model/">}}


