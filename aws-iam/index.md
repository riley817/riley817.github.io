# AWS Identity and Access Management


## Users & Groups
- `IAM` = Identity and Access Management → Global 서비스
- Root 계정은 기본으로 생성되며 사용하거나 공유해서는 안된다.
- **Users 는 조직의 사용자이며 그룹으로 묶을 수 있다.**
- **Groups는 user를 포함할 수 있고 다른 그룹은 포함할 수 없다.**
- 사용자는 그룹에 속할 필요는 없고 user는 여러 그룹에 속할 수 있다.

## Permissions
- 사용자나 그룹은 정책은 JSON 문서를 통해 할당할 수 있다.
- 정책(`polices`)은 사용자의 **권한(permissions)을 정의**한다.
- AWS에서는 최소 권한 원칙을 적용한다 → 사용자가 필요로 하는 것보다 더 많은 권한을 부여하지 않는다.

## IAM Policies inheritance
- 인라인 정책 : 그룹에 속해있지 않아도 정책을 바로 사용자에게 연결 가능

## IAM Policies Structure
```json
{
	"Version": "2012-10-17", 
	"Statement": [
		{
			"Effect": "Allow", 
			"Action": "ec2:Describe*", 
			"Resource": "*"
		}, 
		{
			"Effect": "Allow",
			"Action": "elasticloadbalancing:Describe*", 
			"Resource": "*
		}, 
		{
			"Effect": "Allow",
			"Action": [
				"cloudwatch:ListMetrics", 
				"cloudwatch:GetMetricStatistics", 
				"cloudwatch:Describe*"
			], 
			"Resource": "*"
		}
	]
}
```

### 요소
- `Version` : 정책 버전
- `Id` : 정책을 식별할 수 있는 식별자 (옵션)
- `Statement` : 하나 혹은 여러개 (필수사항)

### Statements 요소

- `Sid` : statement 의 식별자 (옵션)
- `Effect` : 특정 statement에 접근은 허용 여부 (Allow 또는 Deny)를 나타냄
- `Principal` : 정책이  적용 될 대상 (계정, 사용자, 역할)
- `Action` : 정책의 허용 또는 거부되는 호출 리스트 목록
- `Resource` : 정책이 적용될 리소스 목록
- `Condition` : statement가 언제적용될지 결정 (옵션)

## IAM Password Policy
- 강력한 비밀번호 → 계정에 대한 높은 보안

{{<admonition type=tip title="AWS에서 설정가능한 패스워드 정책" >}} 
- 패스워드 최소 길이
- 특수문자 포함 필수 입력 : 대소문자, 숫자, 특수문자
- IAM 계정 사용자들의 패스워드 변경 허용 또는 금지
- 패스워드 만료 지정
- 패스워드 재사용 금지
{{</admonition>}}

## MFA - Multi Factor Authentication
- 사용자는 계정에 액세스 할 수 있으며 구성을 변경하거나 AWS 계정의 리소스를 삭제할 수 있다.
- **MFA는 루트 계정 및 IAM 사용자를 보호하려는 경우 사용할 수 있다.**
- 비밀번호가 도난당하거나 해킹당해도 계정에 손상을 입지 않는다.
> **MFA** = 패스워드(사용자만 알고있는) + 보안 장비(사용자가 소유하고 있는)

### **MFA devices options in AWS**

|MFA devices||
|-----------------------------|--------------------------|
|**Virtual MFA device**|- 하나의 디바이스에 멀티 토큰을 지원 <br/>- Google Authenticator(phone only)<br/>- Authy(multi-device) |
|**Universal 2nd Factor (U2F) Security Key**|- 단일 보안 키를 사용하여 여러 루트 및 IAM 사용자 지원<br/>- YubiKey by Yubico (3rd party)|
|**Hardware Key Fob MFA Device**|- Provided by Gemalto (3rd party)|
|**Hardware Key Fob MFA Device for AWS GovCloud (US)**|- Provided by SurePassID (3rd party)|

## AWS에 액세스 하는 방법

{{<admonition title="AWS 액세스하기 위한 세 가지 방법">}}
1. `AWS Management Console` : 비밀번호 + MFA로 보호
2. `AWS Command Line Interface (CLI)` : access key로 보호
3. `AWS Software Developer Kit (SDK)` : 코드의 경우 access key로 보호
{{</admonition>}}

- 액세스키는 AWS 콘솔을 통해 생성할 수 있다.
- 사용자가 자신의 액세스키를 관리한다.
- **액세스 키는 패스워드와 마찬가지로 기밀이므로 공유하지 않는다.**
- Access Key ID ~= username
- Secret Access Key ~= password

### AWS CLI
- 커맨드라인 쉘에서 명령어를 사용하여 AWS 서비스와 상호작용 할 수 있도록 해주는 도구
- CLI를 사용하면 AWS의 공용 API로 직접 액세스 가능
- 리소소를 자동화하여 관리할 수 있다.
- 오픈소스이다. **[https://github.com/aws/aws-cli](https://github.com/aws/aws-cli)**
- AWS 관리 콘솔 대신 사용 가능

### AWS SDK(AWS Software Development Kit)
- 특정 언어로 된 라이브러리 집합
- 프로그래밍으로 AWS에 액세스를 액세스하고 관리
- 응용 프로그램에 포함

	{{<admonition title="Supports">}}
- SDKs (Javascript, Python, PHP, .NET, Ruby, Java, Go, Node.js, C++)
- Mobile SDKs (Android, iOS…)
- IoT Device SDKs (Embedded C, Arduino…)
	{{</admonition>}}

## IAM Roles for Service
- 일부 AWS 서비스는 사용자를 대신하여 작업을 수행
- IAM 역할을 사용하여 AWS 서비스에 사용 권한을 할당
	{{<admonition tip "공통 역할">}}
- EC2 인스턴스 Roles
- Lambda Function Roles
- Roles for CloudFormation
	{{</admonition>}}


### IAM Security Tools
| **IAM Security Tools**|설명|
|-----------------------|--------------------------|
|**IAM Credentials Report (account-level)**|계정의 모든 사용자와 해당 사용자의 다양한 자격 증명 상태를 나열하는 보고서|
|**IAM Access Advisor (user-level)**|- 사용자에게 부여된 서비스 권한과 해당 서비스에 마지막으로 액세스한 시간을 표시<br/>- 이 정보를 사용하여 정책을 수정|

### IAM 가이드라인 & 모범 사례

- 루트 계정은 AWS 계정을 설정할 때를 제외하고 사용하지 않는다.
- 실제 유저 한명 = 하나의 AWS 유저
- **사용자를 그룹에 할당하고 그룹에 권한을 할당**
- **강력한 패스워드 정책 사용**
- **MFA(Multi Factor Authentication)** 인증 사용 및 적용
- AWS 서비스에 대한 사용 권한을 부여하는 역할 `Roles` 을 생성 및 사용
- **프로그래밍 방식의 액세스 키 사용 (CLI / SDK)**
- IAM 자격증명 보고서를 사용한 계정 감사
- **IAM 유저와 액세스키 공유는 절대 하지 않는다**

## Summary
| |설명|
|-----------------------|--------------------------|
|**Users**|AWS 콘솔에 대한 비밀번호를 갖는 실제사용자와 매핑|
|**Groups**|오직 사용자만 그룹에 포함할 수 있다.<br/>다른 그룹은 포함할 수 없다.|
|**Policies**|사용자나 그룹이 할 수 있는 권한을 알려주는 문서이다.<br/>`JSON` 형태로 되어있다.|
|**Roles**|EC2와 같은 AWS 서비스에서 무언가를 할 수 있게 하는 권한을 주려고 할때 IAM Role을 만들어야 한다.|
|**Security**|**MFA + Password**|
|**Access Keys**|AWS의 CLI, SDK 접근하기 위해 사용하는 자격증명 값이다.|
|**Audit(회계 감사)**|IAM Credential Reports(IAM 자격증명 보고서) : 사용자 자격증명 내역 확인<br/>IAM Access Advisor(IAM 액세스 관리자) : 최근 권한 사용내역 확인|
