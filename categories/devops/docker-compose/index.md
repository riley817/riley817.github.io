# docker


```yml
version: '3.0'

services:

  gitlab:
    container_name: gitlab
    image: gitlab/gitlab-ee:latest
    restart: always
    hostname: "gitlab.mydomain.com"
    environment:
      TZ: 'Asia/Seoul'
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://gitlab.mydomain.com'
        letsencrypt['enable'] = false
        nginx['redirect_http_to_https'] = false
        nginx['listen_https'] = false
        nginx['listen_port'] = 8929
        gitlab_rails['gitlab_shell_ssh_port'] = 2202
    ports:
      - "2202:22"
      - "8929:8929"
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
      - "8980:8080"
    environment:
      - GRADLE_HOME=/app/jenkins/libs/gradle
    volumes:
      - "/home/ubuntu/devops/jenkins/jenkins_home:/var/jenkins_home"
      - "/home/ubuntu/devops/jenkins/script:/app/jenkins/script"
      - "/home/ubuntu/devops/jenkins/source:/app/jenkins/source"
      - "/home/ubuntu/devops/jenkins/ssh_key:/app/jenkins/ssh_key"
      - "/home/ubuntu/devops/jenkins/libs:/app/jenkins/libs"
      - "/var/run/docker.sock:/var/run/docker.sock"
    networks:
      - votenet

  nginx:
    image: nginx:latest
    restart: always
    container_name: nginx
    environment:
      TZ: 'Asia/Seoul'
    ports:
      - 80:80
      - 443:443
    volumes:
      - "/home/ubuntu/devops/nginx/nginx.conf:/etc/nginx/nginx.conf"
      - "/home/ubuntu/devops/nginx/logs:/var/log/nginx"
    networks:
      - votenet

networks:
  votenet:
```
