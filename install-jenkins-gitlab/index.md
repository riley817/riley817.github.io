# [CI/CD 서버 구축하기] 2. docker-compose를 사용하여 Jenkins, GitLab 설치


> - 사내 개발 협업을 위한 CI/CD 서버를 구축한다.
> - AWS EC2 인스턴스를 사용중이다. (Ubuntu LTS)
> - docker, docker-compose가 설치되어 있다.
> - GitLab의 경우 추후 라이센스 구매를 위해 ce가 아닌 ee 버전으로 설치한다.

## 설치를 위한 Docker Compose 설정
아래는 GitLab, Jenkins를 컨테이너를 띄우기 위한 docker-compose 설정이다. GitLab 이미지는 단일 컨테이너에서 서비스를 실행하기위한 Monolithic 이미지이며 최소 시스템 요구사항은 아래와 같다.
    
- [GitLab installation minimum requirements](https://docs.gitlab.com/ee/install/requirements.html)


### docker-compose.yml 파일 준비
작업 디렉터리를 생성한다.
```bash
cd ~/
mkdir devops
cd devops
```
docker-compose.yml 파일을 생성한다
```bash
touch docker-compose.yml
vi docker-compose.yml
```
**docker-compose.yml**에 아래 내용을 추가한다.
```yaml
version: '3.0'

services:
  gitlab:
    container_name: gitlab
    image: gitlab/gitlab-ee:latest
    restart: always
    hostname: "내IP:8289"
    environment:
      TZ: 'Asia/Seoul'
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://내IP:8289'
        nginx['redirect_http_to_https'] = false
        nginx['listen_https'] = false
        nginx['listen_port'] = 8289
        gitlab_rails['gitlab_shell_ssh_port'] = 2202
    ports:
      - "2202:22"
      - "8289:8289"
    volumes:
      - "/home/ubuntu/devops/gitlab/config:/etc/gitlab"
      - "/home/ubuntu/devops/gitlab/logs:/var/log/gitlab"
      - "/home/ubuntu/devops/gitlab/data:/var/opt/gitlab"
    networks:
      - votenet

  jenkins:
    container_name: jenkins
    user: root
    image: jenkins/jenkins:lts
    restart: always
    ports:
      - "9080:8080"
    environment:
      - GRADLE_HOME=/home/ubuntu/devops/jenkins/libs/gradle
    volumes:
      - "/home/ubuntu/devops/jenkins/jenkins_home:/var/jenkins_home"
      - "/home/ubuntu/devops/jenkins/script:/app/jenkins/script"
      - "/home/ubuntu/devops/jenkins/source:/app/jenkins/source"
      - "/home/ubuntu/devops/jenkins/ssh_key:/app/jenkins/ssh_key"
      - "/home/ubuntu/devops/jenkins/libs:/app/jenkins/libs"
      - "/var/run/docker.sock:/var/run/docker.sock"
    networks:
      - votenet

networks:
  votenet:
```

## Docker Compose를 사용하여 구동
**docker-compose.yml**을 생성한 디렉터리에서 해당 명령어를 실행한다.
```bash
$ sudo docker-compose up -d
```
아래 명령어로 구동한 도커 컨테이너를 확인할 수 있다.
```bash
$ sudo docker ps -a
CONTAINER ID   IMAGE                     COMMAND                  CREATED          STATUS                             PORTS                                                                                               NAMES
72dd26a6e3c4   jenkins/jenkins:lts       "/sbin/tini -- /usr/…"   15 seconds ago   Up 12 seconds                      50000/tcp, 0.0.0.0:9080->8080/tcp, :::9080->8080/tcp                                                jenkins
99e8736406b5   gitlab/gitlab-ee:latest   "/assets/wrapper"        15 seconds ago   Up 11 seconds (health: starting)   80/tcp, 443/tcp, 0.0.0.0:8289->9980/tcp, :::8289->9980/tcp, 0.0.0.0:2202->22/tcp, :::2202->22/tcp   gitlab

```
     

## 설치 확인
설치한 GitLab이 정상 동작하는지 Web으로 접속하여 확인한다. DNS 설정은 안했기 때문에 IP로 접속하게 끔 설정했다. 클라우드 서버를 사용하여 구동하기 때문에 외부에서 내가 지정한 포트로 접속이 가능하게 오픈하였다.

{{< figure src="/posts/images//devops/aws-port.png#center" >}}

### GitLab 설치 확인
**external_url**로 설정한 URL로 접속한다. 처음 접속이라면 관리자 계정의 패스워드를 변경할 수 있는 화면이 뜬다.
{{< figure src="/posts/images//devops/gitlab_change_password.png#center" >}}

### Jenkins 설치 확인
Jenkins 설치 후 처음 접속하게되면 Jenkins를 잠금 해제하는 화면이 뜨게 된다. 

{{< figure src="/posts/images//devops/jenkins-init-page.png#center" >}}

아래 명령어를 통해 초기 패스워드를 설정할 수 있다. 그 이후 프로세스를 쭉 따라 Jenkins 관리자 계정을 설정한다.

```bash
$ sudo docker exec jenkins cat /var/jenkins_home/secrets/initialAdminPassword
```

## 참고
- [Jenkins vs GitLab CI: Battle of CI/CD Tools](https://www.lambdatest.com/blog/jenkins-vs-gitlab-ci-battle-of-ci-cd-tools/) 



