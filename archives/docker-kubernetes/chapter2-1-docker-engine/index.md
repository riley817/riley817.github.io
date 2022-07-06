# [시작하세요! 도커/쿠버네티스] Chapter 2. 도커 엔진


> 시작하세요! 도커/쿠버네티스 책 정리 🐳🐳🐳

## 1 도커 이미지와 컨테이너
도커 엔진에서 사용하는 기본단위는 **이미지**와 **컨테이너**이다.

### 1.1 도커 이미지
- 이미지는 컨테이너를 생성할 때 필요한 요소이다.
    - 가상 머신을 생성할 때 사용하는 `iso` 파일과 비슷한 개념이다.
- 여러 개의 계층으로 된 바이너리 파일로 존재, 컨테이너를 생성하고 실행할 때 **읽기 전용**으로 사용된다.

#### 도커 이미지의 구성
도커 이미지 이름은 `[저장소 이름]/[이미지 이름]:[태그]` 형태로 구성 된다.

- **저장소 (Repository)** : 이미지가 저장된 장소. 저장소 이름이 명시되지 않은 이미지는 도커에서 기본적으로 제공하는 이미지 저장소인 도커 허브의 공식 이미지.
- **이미지 이름** : 이미지가 어떤 역할을 하는지 나타냄. 이미지 이름을 생략할 수 없으며 반드시 설정
- **태그** : 이미지의 버전 관리, 혹은 리비전 `Revision` 관리에 사용. 태그를 생략하면 이미지의 `latest` 로 인식

### 1.2 도커 컨테이너
- 이미지로 컨테이너를 생성하면 해당 이미지 목적에 맞는 파일이 들어 있는 격리된 시스템 및 자원 네트워크를 사용할 수 있는 **독립된 공간이 생성** 된다.
- 컨테이너 이미지를 읽기 전용으로 사용하되 이미지에서 변경된 사항만 저장하므로 원래 이미지는 영향을 받지 않는다.
- 생성된 각 컨테이너는 각기 독립된 파일시스템을 제공 받으며 호스트와 분리돼 있으므로 특정 컨테이너에서 어떤 어플리케이션을 삭제해도 다른 컨테이너와 호스트에는 영향을 주지 않는다.

## 2. 도커 컨테이너 다루기
### 2.1 컨테이너 생성
도커는 다양한 기능이 빠르게 업데이트되고 새로운 버전이 배포되므로 설치된 도커 엔진을 먼저 확인해야 한다.

```bash
sudo docker -v
Docker version 19.03.12, build 48a66213fe
```

#### 컨테이너 생성 후 접속

```bash
sudo docker run -i -t ubuntu:14.04
```
- `docker run`  : 컨테이너를 생성하고 실행하는 역할
- `-i -t` : `-i` 옵션으로 컨테이너와 상호 `interactive` 입출력을, `-t` 옵션으로 `tty` 를 활성화해서 `bash` 쉘을 사용하도록 컨테이너를 설정.
- ubuntu:14.04 이미지가 엔진에 존재하지 않는 경우 도커 중앙 이미지 저장소인 도커 허브에서 자동으로 이미지를 내려 받음

위의 명령어로 실행할 경우 컨테이너 생성과 동시에 도커 환경 내부로 접속된다. 호스트 도커 환경으로 복귀하려면,
1. `exit`를 입력한다. 이 방법은 컨테이너 내부에서 빠져나오면서 동시에 컨테이너를 정지한다.
2. `Ctrl + P, Q` 입력한다. 단순히 컨테이너의 쉘에서만 빠져나온다.

#### create 명령

```bash
sudo docker create -i -t --name mycentos centos:7

# docker 컨테이너 실행하기
sudo docker start mycentos

# docker 컨테이너 내부로 들어가기
sudo docker attach mycentos
```

#### run 명령어와 create 명령어의 차이점

- **run 명령어** : pull, create, start 명령어를 일괄적으로 실행한 attach가 가능한 컨테이너라면 컨테이너 내부로 들어간다.
- **create 명령어** : pull 한 뒤 컨테이너만 생성할 start, attach를 실행하지 않는다.
- 보통은 **run** 명령어를 더 많이 사용한다.


#### 도커 이미지 내려받기

```bash
sudo docker pull centos:7
```

#### 도커 엔진에 존재하는 이미지 조회
```bash
sudo docker images

REPOSITORY              TAG                 IMAGE ID            CREATED             SIZE
ubuntu                  18.04               2eb2d388e1a2        10 days ago         64.2MB
wordpress               latest              04b52c96c4b6        11 days ago         543MB
mysql                   5.7                 8679ced16d20        11 days ago         448MB
mysql                   8                   e3fcc9e1cc04        11 days ago         544MB
```

### 2.2 컨테이너 목록 확인

#### 생성한 컨테이너 목록 확인

```bash
# 정지되지 않은 컨테이너만 출력
sudo docker ps

# 정지된 컨테이너를 포함한 모든 컨테이너 출력
sudo docker ps -a
```

![_2020-08-03_23.31.58.png](/categories/images/docker-kubernetes/_2020-08-03_23.31.58.png)

- **CONTAINER ID** : 컨테이너에게 할당되는 고유한 ID. 위 결과에서는 ID의 일부분 밖에 확인할 수 없지만 컨테이너를 확인하기 위해 `docker inspect` 명령어를 사용하면 전체 ID를 확인할 수 있다.
- **IMAGE** : 컨테이너를 생성할 때 사용된 이미지 이름.
- **COMMAND** : 커맨드는 컨테이너가 시작될 때 실행 명령어. 커맨드는 대부분의 이미지에 미리 내장되어 있기 때문에 별도로 설정할 필요가 없다.
- **CREATE** : 컨테이너가 생성되고 난 뒤 흐른 시간을 나타냄
- **STATUS** : 컨테이너의 상태를 나타낸다. `Exited...`  는 정지된 상태 나타내며 `Up ... seconds` 는 실행중인 상태를 나타낸다.
- **PORTS** : 컨테이너가 개방한 포트와 호스트에 연결한 포트를 나열. 
- **NAMES** : 컨테이너의 고유 이름. `docker rename` 명령어를 통해 컨테이너 이름을 변경할 수도 있다.

```bash
# 컨테이너 이름 변경
docker rename angry_morse my_container
```

### 2.3 컨테이너 삭제
`docker rm` 명령어를 통해 컨테이너를 삭제할 수 있다. 한 번 삭제한 컨테이너는 복구할 수 없으므로 신중을 기해야 한다.


#### 도커 컨테이너 삭제
실행 중인 컨테이너는 삭제할 수 없다. 실행 중인 컨테이너를 종료하거나 강제삭제 옵션을 추가해야한다.
```bash
# 실행 중인 컨테이너 종료 후 삭제
docker stop mycentos
docker rm mycentos

# 강제 옵션을 통한 삭제
docker rm -f mycentos
```

#### 모든 도커 컨테이너 삭제 - prune
prune 명령어를 사용하면 모든 컨테이너를 한번에 삭제할 수 있다.

```bash
riley@DESKTOP-XXXXX:~$ docker container prune
WARNING! This will remove all stopped containers.
Are you sure you want to continue? [y/N] y
Total reclaimed space: 0B
```

### 2.4 컨테이너를 외부에 노출
- 컨테이너는 가상 머신과 마찬가지로 가상 IP 주소를 할당 받는다.
- 기본적으로는 172.17.0.x의 IP를 순차적으로 받게 된다. (나는 WSL2 우분투 사용중이라 네트워크 정보가 조금 다른것 같다.)
- `ifconfig` 명령어를 통해 도커의 NAT IP를 할당받은 eht0 인터페이스와 로컬 lo 인터페이스를 확인 할 수 있다.
- 외부에 컨테이너의 애플리케이션을 노출하기 위해서는 eth0의 IP와 포트를 호스트의 IP와 포트에 바인딩 해야 한다.


```bash
# ifconfig
eth0: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 172.21.154.31  netmask 255.255.240.0  broadcast 172.21.159.255
        inet6 fe80::215:5dff:fe81:5ff2  prefixlen 64  scopeid 0x20<link>
        ether 00:15:5d:81:5f:f2  txqueuelen 1000  (Ethernet)
        RX packets 5941  bytes 1924948 (1.9 MB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 146  bytes 10909 (10.9 KB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 0  bytes 0 (0.0 B)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 0  bytes 0 (0.0 B)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

```

#### 도커 컨테이너로 아파치 웹서비스 띄우기
```shell
docker run -i -t --name mywebserver -p 80:80 ubuntu:18.04

root@b6e1a96299bb:/# apt-get update
root@b6e1a96299bb:/# apt-get install apache2 -y
root@b6e1a96299bb:/# service apache2 start
```
{{< figure src="/categories/images/docker-kubernetes/20220420173800.png#center" caption="" >}}
- 아파치 웹 서버는 172 대역을 가진 컨테이너의 NAT IP와 80번 포트로 서비스하므로 여기에 접근하려면 172.x.x.x:80 의 주소로 접근해야 한다.
- 도커의 포트 포워딩 옵션인 `-p` 옵션을 통해 호스트와 컨테이너를 연결했으므로 호스트 ip를 통해 172.x.x.x:80에 접근할 수 있다.

### 2.5 컨테이너 어플리케이션 구축
- 보통은 여러 에이전트나 데이터베이스 등을 연결하여 하나의 서비스로 동작시킨다. 이런 서비스를 컨테이너화(Containerize)할 때 여러개의 애플리케이션을 하나의 컨테이너에 설치할 수 있다. 
- 컨테이너에 애플리케이션을 하나만 동작시키면 컨테이너 간 독립성을 보장함을 동시에 애플리케이션의 버전 관리, 소스코드 모듈화 등이 더 쉬워진다.
- **도커에서는 한 컨테이너에 하나의 프로세스만 실행하도록 권장한다.**

#### MySql 이미지로 데이터베이스 컨테이너 생성
```bash
docker run -d \ 
--name wordpressdb \ 
-e MYSQL_ROOT_PASSWORD=password \ 
-e MYSQL_DATABASE=wordpress \ 
mysql:5.7
```

#### 워드프레스 이미지로 워드프레스 웹 서비스 컨테이너 생성
```bash
docker run -d \ 
-e WORDPRESS_DB_PASSWORD=password \ 
--name wordpress \
--link wordpressdb:mysql \
-p 80 \ 
wordpress
```

- `-d` : Detached 모드로 컨테이너를 실행한다. Detached 모드는 백그라운드에서 동작하는 애플리케이션으로써 실행하도록 설정.
    - 컨테이너 내부에서 프로그램이 foreground로 실행된다.
- `-e` : 컨테이너 내부의 환경변수 설정

```shell
docker exec -i -t wordpressdb /bin/bash

root@d777facb7e1f:/# echo ${MYSQL_ROOT_PASSWORD}
password
```
- `exec` : 명령어를 사용하면 컨테이너 내부에서 명령어를 실행한 뒤 그 결과값을 반환 받을 수 있다. 
    - `-i`, `-t` 옵션을 추가해 `/bin/bash`로 상호 입출력이 가능한 형태로 exec를 사용할 수 있다.

- `--link` : deprecated 된 옵션이며 도커 브리지(bridge) 네트워크를 사용하면 link와 동일한 기능을 손쉽게 사용할 수 있다.

```bash
docker ps -a

CONTAINER ID   IMAGE       COMMAND                  CREATED          STATUS          PORTS                   NAMES
0ba1266d50e0   wordpress   "docker-entrypoint.s…"   3 seconds ago    Up 2 seconds    0.0.0.0:57594->80/tcp   wordpress
7a8d5ded323e   mysql:5.7   "docker-entrypoint.s…"   29 seconds ago   Up 27 seconds   3306/tcp, 33060/tcp     wordpressdb

# wordpress가 사용중인 포트 확인하기
$ docker port wordpress

80/tcp -> 0.0.0.0:57594
```
- 포트 54746과 호스트의 80 포트와 매핑되었다.

### 2.6 도커 볼륨
- 도커 이미지로 컨테이너를 생성하면 **이미지는 읽기 전용이 되고 컨테이너의 변경 사항만 별도 저장하여 컨테이너의 정보를 보존**한다.
- 이미 생성된 이미지는 어떠한 경우로도 변경되지 않으며, 컨테이너 계층에 원래 이미지에서 변경된 파일시스템 등을 저장한다.
- 생성된 mysql 이미지에는 mysql을 실행하는 데 필요한 애플리케이션 파일이, 컨테이너 계층에는 워드프레스에서 쓴 로그인 정보나 게시글 등과 같은 데이터베이스를 운용하면서 쌓인 데이터가 저장된다.

{{< figure src="/categories/images/tucker-go/20220428193444.png#center" width="500" caption="[그림] 이미지와 컨테이너의 구조" >}}

위의 mysql 컨테이너를 삭제하면 컨테이너 계층에 저장되어 있던 데이터베이스의 정보도 모두 사라진다. 도커는 컨테이너의 생성과 삭제가 매우 쉬으므로 데이터를 영속적(`Persistent`)로 활용할 수 있는 방법은 아래와 같다.


**볼륨 생성 방법**
1. 호스트 볼륨 공유
2. 볼륨 컨테이너 활용
3. 도커가 관리하는 볼륨 이용

#### 2.6.1 호스트 볼륨 공유
- `-v` 옵션을 통해 `~/wordpress_db:/var/lib/mysql`로 설정하여 **[호스트 공유 디렉터리]:[컨테이너 공유 디렉터리]** 형태로 디렉토리를 공유한다.
- `~/wordpress_db` 에 mysql 구동에 필요한 파일이 공유된 것을 확인 할 수 있다. 컨테이너의 `/var/lib/mysql` 디렉터리는 호스트의 `~/wordpress_db` 디렉터리와 동기화되는 것이 아니라 **완전히 같은 디렉터리**이다.

```bash
docker run -d \
--name wordpressdb_hostvolume \
-e MYSQL_ROOT_PASSWORD=password \
-e MYSQL_DATABASE=wordpress \
-v ~/wordpress_db:/var/lib/mysql mysql:5.7 
```

```bash
docker run -d \
-e WORDPRESS_DB_PASSWORD=password \
--name wordpress_hostvolume \
--link wordpressdb_hostvolume:mysql \
-p 80 wordpress
```

컨테이너를 삭제해도 호스트 공유 디렉터리(`~/wordpress_db`)에는 mysql 컨테이너가 사용한 데이터가 그대로 남아 있다.
```bash
# stop containers
docker stop wordpress_hostvolume wordpressdb_hostvolume

# remove containers
docker rm wordpress_hostvolume wordpressdb_hostvolume
```

> **💡 이미지에 이미 존재하던 디렉터리에 호스트의 볼륨을 공유하면 컨테이너 디렉터리가 덮어씌워진다.**

#### 2.6.2 볼륨 컨테이너
- `-v` 옵션으로 볼륨을 사용하는 컨테이너를 다른 컨테이너와 공유하는 방법이 있다.
- 컨테이너를 생성할 때 **--volumes-from** 옵션을 설정하면 `-v` 또는 `--volume` 옵션을 적용한 컨테이너의 볼륨 디렉터리를 공유할 수 있다.
- 호스트에서 볼륨만 공유하고 별도의 역할을 담당하지 않는 `볼륨 컨테이너`로 활용하는 것이 가능하다. 

#### 2.6.3 도커가 관리하는 볼륨이용
- `docker volume` 명령어를 사용하여 도커 자체에서 제공하는 볼륨 기능을 활용할 수도 있다.

```bash
docker volume create --name myvolume
```

- 호스트와 볼륨을 공유할 때는 다음과 같은 형식으로 입력한다. 
>[볼륨이름]:[컨테이너의 공유 디렉터리]

```bash
docker run -i -t --name myvolume_1 \
-v myvolume:/root/ \
ubuntu:18.04
```

- `docker inspect` 명령어를 이용하면 볼륨이 실제 어디에 저장되는지 알 수 있다.

```bash
docker inspect --type volume myvolume
```




