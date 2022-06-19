# 1.7 Installation Kibana


+ `Kibana` 는 Elasticsearch 의 오픈 소스 데이터 시각화 플러그인이다. Elasticsearch 클러스터에 인덱싱 된 데이터들을 시각화 하는 기능을 제공한다.

---
## 1. yum 으로 Kibana 설치하기
### 1.1) RPM Repository 등록
`vi /etc/yum.repos.d/kibana.repo`
```bash
# /etc/yum.repos.d/kibana.repo
[kibana-6.x]
name=Kibana repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md 
```
### 1.2) yum 명령어로 install
```bash
yum install kibana
```
---
## 2. Kibana 설정
Kibana 서버는 시작할 때 kibana.yml 에서 속성을 읽는다. kibana.yml 은 배포파일(.zip/.tar.gz) 로 설치한 경우 `$KIBANA_HOME/config` 에 위치하며 패키지 배포판 에서는 `/etc/kibana` 에 위치한다. kibana 서버는 기본적으로 `localhost:5601` 로 구동된다.

### 2.1) kibana.yml 설정
`vi /etc/kibana/kibana.yml`

+ server.host : 기본값 `localhost`. Back-end 서버의 host 를 지정한다.
+ elasticsearch.url : ES 의 인스턴스 URL
+ kibana.index : 저장된 검색, 시각화 및 대시보드를 저장하기 위에 ES 의 색인을 사용S하는데, 인덱스가 아직 없을 경우 키바나가 새 인덱스를 생성

```bash
server.host: "localhost" 
 elasticsearch.url: “http://localhost:9200"
 kibana.index: ".my-kibana"
```

### 2.2) Kibana 서비스 시작하기
```bash
service kibana start
```
