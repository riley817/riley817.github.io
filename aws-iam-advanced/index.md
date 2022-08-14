# AWS Identity and Access Management - Advanced


## STS - Security Token Service
- AWS 리소스에 대한 제한적이고 일시적인 액세스 권한을 허용
- 토큰은 1 시간까지 유효 (새로고침 필요)

### STS API
| STS API | 설명  |
|------------|---|
| **AssumeRole** |- 개인 계정 내에서: 보안을 강화하기 위해 사용 <br/> - 계정 간 : 대상 계정에 역할을 수임하여 작업을 수행 |
| **AssumeRoleWithSAML** |- SAML으로 로그인한 사용자의 자격증명을 반환 |
| **AssumeRoleWithWebIdentity** | - IdP(Identity Provider - Facebook Login, Google Login, OIDC Compatible...) 로그인 사용자의 자격증명을 반환 <br/> - **이것보다는 AWS Cognito 추천함**  |
|**GetSessionToken**|- AWS 루트 계정 또는 사용자에 대한 MFA 인증|

### STS를 통해 역할을 수임받기
1. IAM 계정 내 혹은 계정 간 IAM 역할`(Role)`을 정의한다.
2. 사용자, 역할 등이 액세스 할 수 있는 IAM Role 원칙`(Principal)`을 정의한다.
3. `AWS STS(Security Token Service)` 사용하여 IAM 역할을 가장할 수 있는 임시자격증명을 조회할 수 있다. (AssumeRole API)
> 그림
> 임시자격증명 정보는 15분 ~ 1시간사이의 유효시간을 갖는다.

### 계정 간 STS 
{{<image src="/posts/images/aws/cross-account-access-with-sts.png" width="100%" caption=" https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_aws-accounts.html">}}

1. 프로덕션 계정의 관리자는 개발계정의 사용자들이  `productionapp` 버켓의 읽기/쓰기 IAM 역할을 얻을 수 있는 `UpdateApp`을 생성
2. 개발계정의 관리자는 `Developer` 그룹의 멤버들을 `UpdateApp` 역할의 STS `AssumeRole API`를 호출 할 수 있는 권한을 부여
3. 개발계정의 사용자는 역할 전환을 요청
4. `AWS STS`는 역할 임시자격증명을 반환
5. 임시 자격 증명은 AWS 리소스에 대한 액세스를 허용

## Identity Federation in AWS
- `Federation`을 통해 AWS 외부의 사용자가 AWS 리소스에 액세스하는 임시 역할을 수행

### Federations의 종류
- SAML 2.0
- Custom Identity Broker
- Web Identity Federation with Amazon Cognito
- Web Identity Federation without Amazon Cognito
- Single Sign On
- Non-SAML with AWS Microsoft AD

    > federation을 사용하면 IAM 사용자를 생성할 필요 없다. (사용자 관리는 AWS 외부에 있다.)
    > 그림

#### SAML 2.0 Federation
- `Active Directory` 
- `ADFS(Active Directory Federation Service)` 
- AWS 콘솔 또는 CLI로 액세스 제공 (임시 보안 인증 이용)
- 직원별로 IAM 사용자를 생성할 필요 없다.
- AWS IAM과 SAML 사이에 신뢰 설정이 필요하다.
- SAML 2.0을 통해 웹 기반 크로스 도메인을 지원한다(SSO)
- **STS API : AssumeRoleWithSAML**
- _SAML을 통한 federation 방식은 오래된 방식이다._ -> **Amazon Single Sign On(SSO)** 을 통한 federation은 심플하고 새로운 방식


{{< admonition type=tip title="Using SAML-based federation for API access to AWS" open=true >}}
{{<image src="/posts/images/aws/saml-based-federation.diagram.png" width="100%" caption=" https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_saml.html">}}

1. 조직의 사용자가 클라이언트 앱을 사용하여 조직의 IdP에서 인증을 요청
2. IdP는 조직의 ID 저장소에 대한 사용자 인증
3. IdP는 사용자에 대한 정보로 SAML assertion 구성 -> 클라이언트 앱으로 전송
4. 클라이언트 앱은 AWS STS `AssumeRoleWithSAML` API 호출 SAML 공급자 ARN, 수임할 역할의 ARN 및 IdP의 SAML assertion 전달
5. 클라이언트 앱에 대한 API 응답에 임시 자격 증명 포함
6. 클라이언트 앱은 임시 자격 증명을 사용하여 Amazon S3 API 호출
{{< /admonition >}}

{{< admonition type=tip title="AWS Management Console을 사용하여 SAML 2.0 federation 활성화" open=true >}}
SAML 2.0 Assertion는 자격증명과 호환되며 STS 리소스나 콘솔에 액세스 할 수 있게 해준다.
{{<image src="/posts/images/aws/saml-based-sso-to-console.diagram.png" width="100%" caption=" https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_providers_enable-console-saml.html">}}
{{< /admonition >}}

{{< admonition type=tip title="Active Directory FS" open=true >}}
- ADFS가 SAML 2.0과 호환된다면 과정은 동일하다.
{{<image src="/posts/images/aws/federated_auth_with_adfs.png" width="100%" caption=" https://aws.amazon.com/ko/blogs/security/aws-federated-authentication-with-active-directory-federation-services-ad-fs/">}}

1. 브라우저 인터페이스로 ADFS 로그인
2. ADFS는 사용자가 제대로 인증되었는지 Identity Store를 통해 확인
3. ADFS는 SAML Assertion을 반환
4. SAML Assertion을 AWS STS 통해 역할과 교환 (Sign-in)
5. 임시 자격 증명은 STS `AssumeRoleWithSAML`을 사용하여 반환
6. 사용자가 인증되고 AWS 관리 콘솔에 대한 액세스 권한이 제공
{{< /admonition >}}

#### Custom Identity Broker Application
- IdP가 SAML 2.0과 호화되지 않는 경우에 사용
- **identity 브로커가 적절한 IAM 정책을 결정해야 한다.**
- 사용 STS API : **AssumeRole** or **GetFederationToken**

{{<image src="/posts/images/aws/enterprise-authentication-with-identity-broker-application.diagram.png" width="100%" caption=" https://docs.aws.amazon.com/IAM/latest/UserGuide/id_roles_common-scenarios_federated-users.html">}}

- 자격 증명 브로커는 IAM STS API `AssumeRole`과 `GetFederationToken` API를 이용하여 클라이언트가 AWS 콘솔과 API 등에 액세스 할 수 있게 한다.

#### Web Identity Federation - AssumeRoleWithWebIdentity
- Client-Side에서 AWS에 액세스 하기위해서는 -> AWS Cognito를 사용하기를 권장한다. 

### AWS Cognito
- **목적** : Client Side(모바일, 웹)에서 AWS 리소스를 직접 액세스 제공
- **사용예시** : 페이스북 로그인하여 S3 쓰기 작업을 할 수 있는 임시 권한을 제공하고 싶은 경우
- **문제** : 그때마다 IAM 사용자를 생성해야 한다.
- **어떻게**
    - federated IdP로 로그인 또는 익명 아이디
    - `Federated Identity Pool`로부터 AWS 임시 자격증명을 얻는다.
    - 이러한 자격증명은 사용 권한을 명시하는 미리 정의된 IAM 정책과 함께 제공

### Microsoft Active Directory (AD)
- Found on any Windows Server with AD Domain Services
- Database of objects: User Accounts, Computers, Printers, File Shares, Security Groups
- 중앙 집중식 보안 관리, 계정 생성, 권한 할당
- Objects는 **trees**로 구성되어 있다.
- trees의 그룹은 **forest**라고 한다.

### AWS Directory Services
| AWS Directory Services |   |
|------------|---|
| **AWS Managed Microsoft AD** |- AWS에서 자신만의 AD를 만들고 로컬에서 사용자를 관리 <br/> - MFA 지원 <br/> - Establish “trust” connections with your on-premises AD  |
| **AD Connector** |- on-premises AD로 리다이렉션 Directory Gateway(proxy) <br/>- MFA 지원<br/>- 사용자는 온프레미스 AD에서 관리 |
| **Simple AD** |- AD 호환 관리 디렉토리 <br/> - on-premises AD와 결합할 수 없다. |

## AWS Organizations
- 글로벌 서비스
- 여러 AWS 계정 관리
- 기본 계정이 마스터 계정이므로 변경할 수 없다. (기타 계정은 회원 계정)
- 회원 계정은 한 조직의 일부만 될 수 있다.
- 모든 계정읜 통합 청구 - 단일 지불 방법
- EC2, S3 할인 등 가격 책정 이점
- API를 사용하여 AWS 계정 생성 자동화

### 다중 계정 전략
- 부서별 비용 센터별, dev/test/prod 등 규제 제한 사항(SCP 이용) 또는 리소스 격라 수준에 따라 계정을 생성한다.
- 계정당 서비스 제한이 별도로 있고, 로깅을 위해 격리한다.

### Organizational Units (OU) 

### Service Control Policies (SCP)
- IAM 역할에 Whitelist 또는 Blacklist 
- **OU** 또는 **Account** 레벨에만 적용 가능
- 마스터 계정에는 적용되지 않음
- SCP는 루트 사용자를 포함한 모든 계정의 사용자와 Role에 적용된다.
- SCP는 Service-linked 역할에 영향을 주지 않는다.
    - Service-linked 역할은 다른 AWS 서비스를 AWS 조직과 통합할 수 있도록 SCP에 의해 제한될 수 없다.
- SCP에는 명시적으로 Allow (허용)이 있어야 한다. (기본적으로 아무것도 허용하지 않음)
- 사용 사례
    - 특정 서비스에 대한 액세스 제한
    - 서비스를 명시적으로 사용하지 않도록 설정하여 PCI 규정 준수 적용

#### SCP Hierarchy
>

##### Blacklist & Whitelist 전략

###### 특정 유형의 서비스를 블랙리스트로 등록
```json
{    
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowsAllActions",
      "Effect": "Allow",
      "Action": "*",
      "Resource": "*"
    },
    {
      "Sid": "DenyDynamoDB",
      "Effect": "Deny",
      "Action": "dynamodb:*",
      "Resource": "*"
    }

  ]
}
```

###### 특정 유형의 서비스만 화이트리스트에 추가
```json
{    
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Allow",
      "Action": [
        "ec2:*",
        "cloudwatch:*",
      ],
    }
  ]
}
```

### 조직 이동
#### 계정 마이그레이션
1. 이전 조직에서 구성원 계정 제거
2. 새 조직에서 초대정 보내기
3. 회원 계정에서 새 조직에 대한 초대 수락

#### 마스터 계정의 마이그레이션
1. 조직에서 구성원 계정 제거
2. 이전 조직 삭제
3. 위의 과정 반복 이전 마스터 계정을 새 조직으로 초대

## IAM Conditions

{{< admonition tip "aws:SourceIP:" true >}}
- API 호출하는 곳으로부터 클라이언트 IP를 제한
- 아래 예시는 해당 아이피 범위가 아닌 곳에서는 요청을 허용하지 않음 (NotIpAddress)
```json
{    
  "Version": "2012-10-17",
  "Statement": [
    {
      "Effect": "Deny",
      "Action": "*",
      "Resource": "*",
      "Condition": {
        "NotIpAddress": {
            "aws:SourceIp" [
                "192.0.2.0/24"
                "203.0.113.0/24"
            ]
        },
//...
```
{{< /admonition >}}

{{< admonition tip "aws:RequestedRegion:" true >}}
- API 호출이 이루어지는 리젼을 제한
- 요청리전이 `eu-central-1`, `eu-west-1` 다른 리전에서 온 것은 수락하지 않음.
```json
{    
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowOnlyInsideEU",
      "Effect": "Allow",
      "Action": [
        "ec2:*",
        "rds:*",
        "dynamodb:*",
      ],
      "Resource": "*",
      "Condition": {
        "SingleEquals": {
            "aws:RequestedRegion" [
                "eu-central-1",
                "eu-west-1"
            ]
        },
//...
```
{{< /admonition >}}

{{<admonition tip "Restrict based on tags" true>}}
- 태그가 `DataAnalytics`이면서 부서가 `Data`일 때만 ec2의 해당 작업이 허용
```json
{    
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "StartStopIfTags",
      "Effect": "Allow",
      "Action": [
            "ec2:StartInstances",
            "ec2:StopInstances",
            "ec2:DescribeTags"
        ],
      "Resource": "arn:aws:ec2:regin:account-id:instance/*",
      "Condition": {
        "StringEquals": {
            "ec2:ResourceTag/Project" : "DataAnalytics",
            "aws:PrincipalTag/Department": "Data"
        }
      }
    }
  ]
}
```
{{</admonition>}}
 
{{<admonition tip "다요소 인증 강제" true>}}
```json
{    
  "Version": "2012-10-17",
  "Statement": [
    {
      "Sid": "AllowAllActionsForEC2",
      "Effect": "Allow",
      "Action": "ec2:*"
      "Resource": "*"
    }
    {
      "Sid": "DenyStopAndTerminateWhenMFAIsNotPresent",
      "Effect": "Deny",
      "Action": [
        "ec2:StopInstances",
        "ec2:TerminateInstances"
      ],
      "Resource": "*",
      "Condition" : {
        "BoolIfExists" : {
            "aws:MultiFactorAuthPresent" : false
        }
      }
    }
  ]
}
```
{{</admonition>}}

## IAM for S3
{{<admonition success "IAM for S3" true>}}
- `ListBucket` : Bucket 레벨의 permission
    - `"arn:aws:s3:::test"`
- `GetObject`, `PutObject`, `DeleteObject` : Object 레벨의 permission
    - `arn:aws:s3:::test/*`

```json
{    
  "Version": "2012-10-17",
  "Statement": [
    {
        "Effect": "Allow",
        "Action": ["s3:ListBucket"],
        "Resource": ["arn:aws:s3:::test"],
    },
    {
        "Effect": "Allow",
        "Action": [
            "s3:GetObject",
            "s3:PutObject",
            "s3:DeleteObject"
        ],
        "Resource": ["arn:aws:s3:::test/*"],
    }
  ]
}
```
{{</admonition>}}

### IAM Role과 리소스 기반의 정책
계정 A에서 계정 B에 존재하는 S3 버킷을 프록시로 사용하여 연결하는 대신 리소스 기반 정책으로 연결했을 때

- 역할을 수임받을 때 계정 A의 원래 권한을 포기하고 Role에 할당된 권한을 갖게 된다.
- 리소스 기반 정책을 사용할 때 주체가 권한을 포기할 필요가 있다.
    - example: 계정 A는 DynamoDB 테이블을 스캔하여 B계정의 S3 덤프
- Supported by: Amazon S3 buckets, SNS topics, SQS queues, etc...    

### IAM Permission Boundaries
- IAM permission boundaries는 사용자와 역할에 대해 지원 (그룹은 지원하지 않음)
- 관리 정책을 사용하여 IAM 엔티티가 얻을 수 있는 최대 권한을 설정하는 고급 기능이다.

{{<image src="/posts/images/aws/EffectivePermissions-rbp-boundary-id.png" width="10%" align="center" caption="https://docs.aws.amazon.com/ko_kr/IAM/latest/UserGuide/access_policies_boundaries.html">}}

#### Permission Boundaries 사용 사례
- 권한 범위 내에서 비관리자에게 책임 위임
- 개발자가 자신의 권한을 관리할 수 없도록 하고 자체 정책 및 사용 권한을 관리할 수 있도록 허용
- 조직 및 SCP를 사용하는 전체 계정 대신 특정 사용자 한명을 제한할 때 유용

#### IAM Policy Evaluation Logic

{{<image src="/posts/images/aws/PolicyEvaluationHorizontal111621.png" caption=" https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html">}}

## AWS Resource Access Manager (RAM)
- 소유한 AWS 리소스를 다른 AWS 계정과 공유
- 모든 계정 또는 조직 내에서 공유
- 리소스 중복 방지
- **VPC 서브넷**
    - 모든 리소스를 동일한 서브넷에서 시작할 수 있도록 허용
    - 동일한 AWS 조직 사용자여야 한다.
    - Security Group 및 기본 VPC 공유 할 수 없음
    - 참가자는 자신의 리소스를 관리할 수 있음
    - 참가자는 다른 참가자 또는 소유자의 리소스를 보거나 수정 및 삭제할 수 없음
- **AWS Transit Gateway**
- **Route53 Resolver Rules**
- **License Manager Configurations**

### Resource Access Manager - VPC 예시
{{<image src="/posts/images/aws/resource-access-manager-vpc-example.png" caption="">}}

- **각각의 계정**
    - 자체 자원에 대한 책임이 존재
    - 다른 계정의 리소스를 보거나 수정 또는 삭제 불가
- **네트워크는 공유된다.**
    - VPC에 구현된 모든 리소스는 VPC의 다른 리소스와 통신 가능
    - 개인 IP를 사용하여 여러 계정에서 애플리케이션에 쉽게 액세스
    - 최대 보안을 위해 다른 계정의 보안 그룹 참조

## AWS Single Sign-On (SSO)
- Singe sign-on을 중앙에서 관리하여 여러 계정 및 3rd-party 비즈니스 애플리케이션에 액세스한다.
- **AWS 조직과 통합**
- **SAML 2.0 마크업** 지원
- on-premises로 통합된 **Active Directory**와 통합
- 중앙 집중식 관리
- CloudTrail을 통한 중앙 집중식 감사




