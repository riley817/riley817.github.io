# AWS CentOS 7 테스트 서버 구축하기


> 목표 😁😁😁
> - AWS에서 Cent OS 7로 테스트 서버를 구축한다.
> - [콘솔접속경로](https://console.aws.amazon.com/ec2/v2/home)

## AWS EC2 인스턴스 추가하기 

### 1. 콘솔 대시보드에서 **인스턴스 시작**을 선택한다.
![ec2-1](/posts/images/devops/8001/20191119221154.png)

### 2. Amazon Machine Image(AMI)에서 인스턴스 템플릿을 선택한다.
+ 검색 창에 centos를 검색 후 원하는 버전을 선택한다.

![ec2-2](/posts/images/devops/8001/20191119221229.png)

### 3. 인스턴스 유형 선택 페이지에서 하드웨어 구성을 선택한다.
+ 일단 난 프리티어니깐... `t2.micro`를 선택 🤔

![ec2-3](/posts/images/devops/8001/20191119221251.png)

### 4. 🤷🏻‍♀️ 검토 후 시작(Review and Launch) 버튼을 클릭하여 구성을 완료한다.

### 5. 보안 그룹 구성 
![ec2-5](/posts/images/devops/8001/20191119221452.png) 

### 6. 인스턴스 시작 검토 페이지에서 시작을 선택한다. 
![ec2-5](/posts/images/devops/8001/20191119221502.png) 

### 7. 키 페어 생성화면에서 키 페어를 생성한다. 
+ 프라이빗 키 파일(*.pem)을 다운로드한다. 해당 키로 EC2 인스턴스 액세스할 수 있다.
+ 키 페어가 없이 처음에는 인스턴스에 연결할 수 없으므로 꼭 다운로드 한다.

![ec2-5](/posts/images/devops/8001/2019111922613.png)

> 준비가 완료되면 인스턴스 시작을 선택한다.👏🏻👏🏻👏🏻

## Mac 접속 설정하기

+ centos AMI의 경우 접속계정은 `centos`이다. 
+ [Linux 인스턴스 연결시 각 OS별 계정정보 참고](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/putty.html)
+ [PuTTY를 사용하여 Windows에서 Linux 인스턴스에 연결](https://docs.aws.amazon.com/ko_kr/AWSEC2/latest/UserGuide/putty.html)

### SSH 접속 Config 설정

`~/.ssh/config`
```bash
Host riley-aws
  HostName "publicIP" 
  User centos 
  Port 22
  IdentityFile ~/.ssh/riley.pem
```

{{<admonition type=tip title=".pem로 ssh 접속시 bad permissions 오류가 발생할 때" >}} 
```
chmod 400 <your>.pem  
```
{{</admonition>}}


### root 계정의 직접 접속 차단하기

Linux의 ssh 기본 설정에는 root 로그인이 허용되어 있다. 포트스캐닝을 통해 ssh 포트가 열려 있는게 확인되면 해커에 의해 무작위로 root 계정으로 ssh 접속을 시도 할 수 있다. 그러므로 root 계정으로 직접 접속을 차단하는 것이 좋다.
 
{{<highlight bash>}}
$ sudo vi /etc/ssh/sshd_config


#LoginGraceTime 2m
PermitRootLogin no
#StrictModes yes
#MaxAuthTries 6
#MaxSessions 10

# 아무출력도 없으면 잘된거임
sudo systemctl restart sshd.service
{{< /highlight>}}

### 계정 생성 및 패스워드 설정

#### root password 설정
```bash
sudo passwd root
```

#### 사용자 추가하기
```bash
su -root
adduser kai
```

#### 새로 생성한 계정의 패스워드 설정하기
```bash
sudo passwd kai
```

#### 새로 생성한 계정에 root 권한을 사용할 수 있도록 설정

{{< highlight bash>}}

sudo visudo


## Next comes the main part: which users can run what software on
## which machines (the sudoers file can be shared between multiple
## systems).
## Syntax:
##
##      user    MACHINE=COMMANDS
##
## The COMMANDS section may have other options added to it.
##
## Allow root to run any commands anywhere
root    ALL=(ALL)       ALL
kai     ALL=(ALL)       ALL
{{< /highlight>}}

