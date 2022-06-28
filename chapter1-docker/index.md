# About LoveIt



> 시작하세요! 도커/쿠버네티스 책 정리 🐳🐳🐳

**도커(docker)는 리눅스 컨테이너에 여러 기능을 추가함으로써 애플리케이션을 컨테이너로서 좀 더 쉽게 사용할 수 있게 만들어진 오픈소스 프로젝트**이다. **GO** 언어로 작성되어 있으며 2013년 3월에 첫 번째 릴리스가 발표된 이후 지금까지 꾸준히 개발되고 있다.

**도커와 관련된 프로젝트**
- Docker Compose
- Private Registry
- Docker Machine
- Kitematic

도커라고 하면 `Docker Engine` 혹은 도커와 관련된 모든 프로젝트를 의미한다. 도커 생태계에 있는 여러 프로젝트들은 도커 엔진을 좀 더 효율적으로 사용하기 위한 것에 불과하며 핵심이 되는 것은 도커 엔진이다.

## 1.1 가상 머신과 도커 컨테이너
### 기존의 가상화 기술

{{< figure src="/categories/images/docker-kubernetes/docker-kubernetes-1.png#center" width="500" caption="[그림] 가상 머신 구조" >}}

- 하이퍼바이저를 이용해 여러 개의 운영체제를 하나의 호스트에서 생성해 사용하는 방식
- 여러 운영체제는 가상 머신이라는 단위로 구분되며 각 가상머신에는 Ubuntu, CentOS 등의 운영체제가 설치되어 사용되어짐.
- **Guest OS** : 하이퍼바이저에 의해 생성되고 관리되는 운영체제. 각 게스트 운영체제는 다른 게스트 운영체제와 완전히 독립된 공간과 시스템 자원을 할당받게 됨.
- **Host OS** : 서버를 부팅할 때 실행되는 운영체제
- 대표적인 가상화 툴 : VirtualBox, VMware 등

### 기존의 가상화 기술의 문제점
1. 하이퍼바이저를 반드시 거치기 때문에 일반 호스트 OS에 비해 성능이 손실이 발생
2. 게스트 OS를 운영하기 위한 라이브러리, 커널 등을 전부 포함하기 때문에 가상머신의 이미지 크기가 클 수밖에 없음

### 도커 컨테이너
{{< figure src="/categories/images/docker-kubernetes/docker-kubernetes-2.png#center" width="500" caption="[그림] 도커 컨테이너 구조 [출처] https://www.docker.com/resources/what-container " >}}

### 도커 컨테이너의 장점

#### 성능 손실이 적다
- 가상화된 공간을 생성하기 위해 리눅스 자체 기능인 `chroot`, `name space`, `cgroup`을 사용함으로써 프로세스 단위의 격리 환경을 만들기 때문에 성능 손실이 없음.
- **이미지 용량이 가상머신에 비해 줄어들어 이미지를 만들고 배포하는 시간이 빠르다**
- 컨테이너 필요한 커널은 호스트의 커널을 공유해 사용하고, 컨테이너 안에 애플리케이션 구동하는 데 필요한 라이브러리 및 실행 파일만 존재하므로 이미지 용량이 가상 머신에 비해 대폭 줄어듬.

## 1.2 도커를 시작해야 하는 이유
- 도커는 컨테이너 생태계에서 사실상 표준으로 사용
- 쿠버네티스, 메소스와 같은 오픈소스 프로젝트에서도 도커를 기준으로 개발

### 1.2.1 애플리케이션의 개발과 배포가 편하다
- 도커 컨테이너는 호스트 OS 위에서 실행되는 격리 공간이기 때문에 **독립된** 개발 환경을 보장받을 수 있다.
- **도커 이미지**라고 하는 일종의 패키지로 만들어서 관리하기 때문에 설치 작업 없이 복제를 통하여 간편하게 통합 환경 구성이 가능하다.

### 1.2.2 여러 애플리케션의 독립성과 확장성이 높아진다.


#### 모놀리스(Monolith) 애플리케이션
- 소프트웨어의 여러 모듈이 상호 작용하는 로직을 하나의 프로그램에서 구동시키는 방식. 서비스의 기능이 복잡해지고 거대하질수록 확장성과 유연성이 줄어든다.

#### 마이크로서비스(Micro-service)
- 여러 모듈을 독립된 형태로 구성하기 때문에 **언어에 종속되지 않고** 변화에 **빠르게 대응** 할 수 있고 각 모듈의 관리가 쉬워진다.
- 도커 컨테이너는 수 초 내로 생성과 시작이 가능하고 여러 모듈을 독립된 환경을 동시에 제공할 수 있어 마이크로서비스 구조에서 가장 많이 사용.
- 웹 서비스는 데이터베이스 컨테이너와 서버 컨테이너로 분리할 수 있으며, **웹 서비스에 부하가 발생할 시 마이크로서비스 구조의 웹 서버 컨테이너만을 동적으로 늘려서 부하를 분산**할 수 있다.
- 웹 서버와 데이터베이스의 **이미지를 독립적으로 관리**하기 때문에 유지 보수도 용이
- 마이크로서비스는 개발자가 직접 구현하기보다 **도커 스웜 모드,** 쿠버네티스 등 컨테이너 오케스트레이션 플랫폼을 통해 사용하는 것이 일반적

## 1.3 도커 엔진 설치
- 도커는 리눅스 컨테이너를 제어는 API를 Go 언어로 구현한 **libcontainer**를 사용하기 때문에 대부분 리눅스 운영체제에서 사용할 수 있다.
- 마이크로소프트 윈도우, 맥 OS X 에서도 도커를 사용할 수 있지만 윈도우 10, 맥 OS X 10.10.3 Yosemite 이전 버전을 사용한다면 도커를 사용하기 위해 별도의 가상화 공간을 생성해야 한다.

### 1.3.1 도커 엔진의 종류 및 버전
- **EE(Docker Enterprise Edition)** 인 유료버전과 **CE(Community Edition)** 인 무료 버전으로 구분되어 제공
- 버전은 **(출시 년도)-(출시 월)-(도커 엔진 종류)** 의 형태
- EE와 CE 부가적인 서비스 지원 수준에 차이만 있을 뿐 핵심적인 컨테이너 기술을 CE에서도 동일하게 사용가능

### 1.3.2 리눅스 도커 엔진 설치

#### 확인사항
- **최신 버전의 커널을 사용하고 있는지 확인한다.**
  - 호스트 운영체제가 최소한 3.10 버전 이상을 사용해야 도커가 정상적으로 사용 가능
  - `uname -r` 명령어를 통해 커널 버전을 확인한다.

- **지원 기간 내에 있는 배포 판인지 확인한다.**
  - 일부 오래된 리눅스 배포판은 업데이트 등의 지원을 받지 못할 수 있음.
- **64비트 리눅스 인지 확인**
  - 도커는 64비트에 최적화 되어 있으며 32비트 버전에서 도커를 실행하는 방법은 권장하지 않음
- **sudo 명령어를 통해 설치하거나 root 권한을 소유한 계정에서 설치를 진행해야 한다.**

#### 도커 설치 가이드

- [Install Docker Engine](https://docs.docker.com/engine/install/)

**CentOS 7, RHEL7**

```bash
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io
sudo systemctl start docker

# docker 가 정상동작하는지 확인하기
sudo docker info
```
**Ubuntu 14.04, 16.04, 18.04**

```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
apt-get update
apt-get install docker-ce
```

### 1.3.3 윈도우, 맥 OS 도커 설치

- 이전에는 윈도우와 맥 OS X에서 도커를 설치하려면 도커 툴박스라는 패키지를 설치해야 했음.
- 최근에는 자체 가상화 기술을 사용한 도커가 설치되어 버추얼박스에 의존하지 않고 도커를 설치할 수 있다.
- **Docker for Windows**는 `Hyper-V`, **Docker for Mac OS X** 는 `xhyve` 기술을 이용한다.

#### Docker for Windows WSL2 설정
Windows WSL2 리눅스 가상환경에서 도커를 실행할 경우 해당 옵션을 선택하면 된다. (Installer 실행시 꼭 관리자권한으로 실행...)
- [WSL 2에서 Docker 원격 컨테이너 시작](https://docs.microsoft.com/ko-kr/windows/wsl/tutorials/wsl-containers)

**General > Use the WSL 2 based engine**
{{< figure src="/categories/images/docker-kubernetes/20220420162757.png#center" caption="" >}}
\
**Resources > WSL INTEGRATION > Enable integration with my default WSL distro**
{{< figure src="/categories/images/docker-kubernetes/20220420162758.png#center" caption="" >}}



## Trouble Shooting
- [centos 7.3 docker-engine conflicts with docker-common-2](https://stackoverflow.com/questions/41766075/centos-7-3-docker-engine-conflicts-with-docker-common-2)
