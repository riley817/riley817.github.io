# [AWS EKS] 2. AWS CLI, eksctl, kubectl 설치 및 설정


> ☁️  [Amazon EKS 웹 애플리케이션 구축하기](https://catalog.us-east-1.prod.workshops.aws/workshops/9c0aa9ab-90a9-44a6-abe1-8dff360ae428/ko-KR/)
워크샵을 실습한 내용입니다. \
> ☁️ 워크샵에는 Cloud9을 구축했지만 나는 따로 구축하지는 않았다.

## AWS CLI

### AWS CLI 설치하기 (Mac OS)

[AWS Command Line Interface](https://docs.aws.amazon.com/cli/latest/userguide/cli-chap-welcome.html)는 command-line shell 명령어를 사용하여 AWS 서비스의 EC2, VPC 등과 같은 AWS의 리소스를 프로비저닝 할 수 있다.

Mac OS는 pkg 파일을 다운로드 하여 설치한다.

- Mac OS : https://awscli.amazonaws.com/AWSCLIV2.pkg
- [다른 OS에서 AWS CLI 설치하기](https://docs.aws.amazon.com/ko_kr/cli/latest/userguide/getting-started-install.html)

```bash
$ aws --version                                                   

aws-cli/2.6.1 Python/3.9.11 Darwin/21.4.0 exe/x86_64 prompt/o
```
### AWS Credential Configure 

AWS CLI에서 자주 사용되는 구성 설정과 자격 증명을 저장할 수 있다. `aws configure` 명령어를 통해 자주 사용하는 자격증명 정보를 저장 할 수 있다. 자세한 설명은 [여기](https://docs.aws.amazon.com/cli/latest/userguide/cli-configure-files.html)를 참고한다.

`aws configure --profile` 명령어를 통해 여러 AWS 계정에 접근하도록 관리할 수 있다. --profile 옵션에 계정 이름을 할당한다.

```bash
aws configure --profile riley-admin
```

```bash
19:05:42 › aws configure   
AWS Access Key ID [None]: ****************SUIB # IAM 계정 Access Key ID 입력
AWS Secret Access Key [None]: ****************1eOn # IAM 계정 Secret Access Key 입력
Default region name [None]: ap-northeast-2 # 리전 입력
Default output format [None]: json # json
```

#### 인증 설정 확인하기
인증설정은 `cat ~/.aws/config`, `cat ~/.aws/credentials`에 나누어 저장된다. 

```bash
cat ~/.aws/config

[profile riley-admin]
region = ap-northeast-2
output = json
```

```bash
23:36:50 › cat ~/.aws/credentials 
[riley-admin]
aws_access_key_id = ~~~~~~
aws_secret_access_key = ~~~~~
```
#### 테스트

```bash
# ec2 인스턴스 조회
aws ec2 describe-instances --profile riley-admin

aws sts get-caller-identity # default
aws sts get-caller-identity --profile riley-admin
```


## kubectl 설치

### kubectl 설치하기

**kubectl**은 쿠버네티스 클러스터에 명령을 내리는 CLI이다.
쿠버네티스는 오브젝트 생성, 수정, 삭제와 관련한 동작을 수행하기 위해 **쿠버네티스 API**를 사용한다. 이때 kubectl CLI을 사용하여 해당 명령어가 쿠버네티스 API를 호출하여 관련 동작을 수행한다.

[Installing kubectl](https://docs.aws.amazon.com/eks/latest/userguide/install-kubectl.html)에 접속하여 **배포할 Amazon EKS 버전과 상응하는 kubectl를 설치**한다. 

```bash
# To install kubectl on macOS
$ curl -o kubectl https://s3.us-west-2.amazonaws.com/amazon-eks/1.22.6/2022-03-09/bin/darwin/amd64/kubectl


$ chmod +x ./kubectl
$ mkdir -p $HOME/bin && cp ./kubectl $HOME/bin/kubectl && export PATH=$HOME/bin:$PATH
```

`kubectl` 설치 후 버전 명령어를 통해 잘 설치되었는지 확인한다.
```bash
$ kubectl version --short --client

Client Version: v1.21.2-13+d2965f0db10712
```

### Kubectl 인증 정보 설정
- [Kubernetes context 관리](/categories/docker-kubernetes/kubectl-change-context/)


## eksctl 설치하기
### eksctl 설치하기

[eksctl](https://eksctl.io/)이란 EKS 클러스터를 쉽게 생성 및 관리하는 CLI 툴 이다. 아래 링크를 통해 해당 OS에 최신 eksctl 바이너리를 직접 다운로드 하거나 해당 명령어를 통해 다운로드 한다.

- https://github.com/weaveworks/eksctl/releases

```bash
#  To install eksctl on macOS (intel)
$ curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C ~/tmp
```

```bash
$ sudo mv -v ~/tmp/eksctl /usr/local/bin

$ eksctl version
```

## EKS IAM 계정 연결
...(정리중)
