# [AWS EKS] 1. IAM 계정 생성


> ☁️  [Amazon EKS 웹 애플리케이션 구축하기](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/ko-KR/)
워크샵을 실습한 내용입니다. \

## AWS Cloud9 

[AWS Cloud9](https://aws.amazon.com/ko/cloud9/?nc1=h_ls) 은 브라우저만으로 코드를 작성, 실행 및 디버깅할 수 있는 IDE(통합개발환경)이다. 코드 편집기, 디버거 및 터미널 기능을 포함하고 있고 프로그래밍 언어를 위한 필수 도구가 사전에 패키징 되어 제공된다.

## AWS Cloud9 시작하기

### AWS Cloud9 IDE 구성
1. [AWS Cloud9 콘솔](https://ap-northeast-2.console.aws.amazon.com/cloud9/home/product) 접속 후 **Create environment** 버튼 클릭한다.

2. IDE의 Environment 이름 및 설명을 작성한다.

{{< figure src="/posts/images/devops/Pasted image 20220505142740.png#center">}}

3. 인스턴스 타입을 **t3.medium**으로, 플랫폼의 경우 **Amazon Linux 2 (recommended)** 설정 Next Step을 클릭하여 지정한 속성 확인 후 **Create environment** 클릭하여 environment 생성한다.

{{< figure src="/posts/images/devops/Pasted image 20220505142839.png#center">}}

4. 생성 완료 

{{< figure src="/posts/images/devops/Pasted image 20220505142911.png#center" width="500">}}

## IAM Role 생성하기

IAM Role은 특정 권한을 가진 IAM 자격 증명이다. 서비스에 IAM Role을 부여할 경우 서비스가 사용자를 대신하여 수임받은 역할을 수행할 수 있다. 아래는 **Administrator access** 정책을 가진 IAM Role을 생성하고 AWS Cloud9에 연결한다.

### IAM Role 생성
[IAM 대시보드](https://us-east-1.console.aws.amazon.com/iamv2/home#/home)로 접속 후 **액세스 관리 > 역할 > 역할 만들기** 를 클릭한다.

{{< figure src="/posts/images/devops/Pasted image 20220505144355.png#center">}}

역할 이름(Role name)에는 `eksworkspace-admin` 입력 후 권한에는 `AdministratorAccess` 관리형 정책을 추가하고 **역할 생성** 버튼을 클릭한다.

{{< figure src="/posts/images/devops/20220505223536.png#center">}}

> 실제 환경에서는 AdministratorAccess 정책보다는 환경을 구동할 최소 권한만 부여하는 것이 좋다.

### IDE(AWS Cloud9 인스턴스)에 IAM Role 부여

[EC2 대시보드](https://us-east-1.console.aws.amazon.com/ec2/v2/home?region=us-east-1#Instances:v=3;sort=desc:launchTime)로 접속한다. 앞에서 생성한 Cloud9 EC2 인스턴스를 선택한다.

{{< figure src="/posts/images/devops/Pasted image 20220505143711.png#center">}}


**작업 > 인스턴스 설정 > IAM 역할 연결/바꾸기**를 클릭하여 EC2 콘솔에서 AWS Cloud9 인스턴스에 위에서 생성한 IAM Role을 부여한다.

{{< figure src="/posts/images/devops/Pasted image 20220505143750.png#center">}}

IAM Role에서 `eksworkspace-admin` 선택 후 **저장**

{{< figure src="/posts/images/devops/Pasted image 20220505143845.png#center">}}

### Cloud9 IDE에서 IAM 설정 업데이트
AWS Cloud9은 IAM Credentials를 동적으로 관리한다. 생성한 credentials는 **EKS IAM authentication과 호환되지 않기에 이를 비활성화하고 IAM Role을 붙인다.** (🤔 머선말인지 잘 모르겠지만...)
