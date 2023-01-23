# [Elasticsearch 검색 엔진 구축 강의] 3. Elasticsearch 및 Kibana 설치


## Elasticsearch 설치하기
Elasticsearch 는 Java 언어로 이루어진 아파치 Lucene 기반으로 이루어져 있다. 그러므로 설치를 위해서는 **Java 가 먼저 설치되어 있어야 한다.** 


### 1. yum 으로 설치하기
#### RPM Repository 등록하기
`/etc/yum.repos.d/elasticsearch.repo` elastic 저장소를 수동으로 추가 한다.

```bash
# /etc/yum.repos.d/elasticsearch.repo
[elasticsearch-6.x]
name=Elasticsearch repository for 6.x packages
baseurl=https://artifacts.elastic.co/packages/6.x/yum
gpgcheck=1
gpgkey=https://artifacts.elastic.co/GPG-KEY-elasticsearch
enabled=1
autorefresh=1
type=rpm-md 
```

```bash 
sudo yum install elasticsearch
```

#### RPM Download 하여 설치하기
- `Elasticsearch` RPM 다운로드 후 설치한다.
- `elasticsearch`의 `user`, `group`이 자동 생성

```bash
# rpm 파일 내려받기
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.0.rpm

rpm -ivh elasticsearch-6.4.0.rpm
```

### 2. zip, tar 다운로드하여 설치하기
- Elasticsearch는 `.zip` 또는 `.tar.gz` 패키지로도 제공된다. 모든 시스템에 제한없이 가장 쉽게 설치할 수 있는 방법이다.
- `root`가 아닌 일반 계정으로만 설치 가능
- rpm 설치와 비교했을 때 `config`, `data`가 추가로 생성

#### .zip 다운로드 및 설치하기
```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.2.zip

unzip elasticsearch-6.4.2.zip
```

#### tar.gz 패키지로 다운로드 및 설치하기
```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.2.tar.gz

tar -xzf elasticsearch-6.4.2.tar.gz
```

### 3. Elasticsearch 실행하기

```bash
./bin/elasticsearch 

# running as a daemon
./bin/elasticsearch -d -p pid 
```
#### Elasticsearch running 중 인지 확인하기
+ 기동시 옵션으로 포트를 지정하지 않으면 기본 포트는 9200.
+ curl 과같은 HTTP 요청으로 JSON 결과값이 올바르게 오는지 확인.

```bash
curl http://localhost:9200
```

{{<image src="/posts/images/elastic/page1.png" caption="" width="100%">}}

### 4. Elasticsearch 서비스 시작하기
#### Elasticsearch 설정파일
Elasticsearch 의 3개의 설정파일이 존재한다.

* `elasticsearch.yml` : Elasticsearch 기본설정 
* `jvm.options` : Elasticsearch JVM 설정
* `log4j2.properties` : Elasticsearch Logging 설정

#### elasticsearch.yml
* elasticsearch.yml 기본 설정 파일이다. YAML 형식으로 작성되어 있으며, 필요한 옵션을 수정할 수 있다.
* `/etc/elasticsearch/elasticsearch.yml` 
```yaml
# data 파일 경로와 log 파일의 경로를 수정하기.
 path.data: /var/lib/elasticsearch
 path.logs: /var/log/elasticsearch
```
#### 서비스 시작하기
```bash
# init 사용
service elasticsearch start
```
```bash
# systemd 사용
systemctl start elasticsearch.service
```
---

## Kibana 설치하기
`Kibana`는 Elasticsearch의 오픈 소스 데이터 시각화 플러그인이다. Elasticsearch 클러스터에 인덱싱 된 데이터들을 시각화 하는 기능을 제공한다.

### 1. yum 으로 Kibana 설치하기
#### RPM Repository 등록

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
#### yum 명령어로 install
```bash
yum install kibana
```

### 2. Kibana 설정
Kibana서버는 시작할 때 `kibana.yml`에서 속성을 읽는다. `kibana.yml`은 배포파일(.zip/.tar.gz)로 설치한 경우 `$KIBANA_HOME/config` 에 위치하며 패키지 배포판 에서는 `/etc/kibana` 에 위치한다. kibana 서버는 기본적으로 `localhost:5601` 로 구동된다.

#### kibana.yml 설정
`vi /etc/kibana/kibana.yml`

+ server.host : 기본값 `localhost`. Back-end 서버의 host를 지정한다.
+ elasticsearch.url : ES 의 인스턴스 URL
+ kibana.index : 저장된 검색, 시각화 및 대시보드를 저장하기 위에 ES의 색인을 사용S하는데, 인덱스가 아직 없을 경우 키바나가 새 인덱스를 생성

```bash
server.host: "localhost" 
 elasticsearch.url: “http://localhost:9200"
 kibana.index: ".my-kibana"
```

### 3. Kibana 서비스 시작하기
```bash
service kibana start
```

## 참고

+ [Install Elasticsearch](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html)

