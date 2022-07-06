# [TIL & Issue Note] 20220218


## 20220218 Note 
1. 도커 비공개 레포지토리를 자체 인증서를 발급 하여 세팅한다.
2. 깃랩 러너를 구성하여 main 브랜치에 머지되었을 때 서버를 구성한다.

docker-compose.yml
```yml
version: '3.8'

services:
  registry:
    image: registry:2
    restart: always
    volumes:
      - /home/cherry/devops/docker_repository/images:/var/lib/registry
      - /home/cherry/devops/docker_repository/certs:/certs
    ports:
      - 5000:5000
    environment:
      REGISTRY_HTTP_TLS_CERTIFICATE: /certs/domain.crt
      REGISTRY_HTTP_TLS_KEY: /certs/domain.key
  gitlab-runner:
    container_name: gitlab-runner
    image: 'gitlab/gitlab-runner:latest'
    restart: always
    volumes:
      - ./gitlab-runner/config:/etc/gitlab-runner
      - /var/run/docker.sock:/var/run/docker.sock
```

