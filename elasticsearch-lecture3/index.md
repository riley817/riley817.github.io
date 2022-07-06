# Elasticsearch 설치


Elasticsearch 는 Java 언어로 이루어진 아파치 Lucene 기반으로 이루어져 있다. 그러므로 설치를 위해서는 **Java 가 먼저 설치되어 있어야 한다.** ES 는 여러가지 방법으로 설치가능하지만 그중에서 RPM 패키지 관리자로 설치하는 방법에 대해 정리하였다.

+ [다른 방법으로 ES 설치하기](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html)

## yum 으로 설치하기
### RPM Repository 등록하기
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

## RPM Download 하여 설치하기
- `Elasticsearch` RPM 다운로드 후 설치한다.
- `elasticsearch`의 `user`, `group`이 자동 생성

```bash
# rpm 파일 내려받기
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.0.rpm

rpm -ivh elasticsearch-6.4.0.rpm
```

## zip, tar 다운로드하여 설치하기
Elasticsearch는 `.zip` 또는 `.tar.gz` 패키지로도 제공된다. 모든 시스템에 제한없이 가장 쉽게 설치할 수 있는 방법이다.

+ [다른 방법으로 ES 설치하기](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html)
+ [최신 버전 다운로드](https://www.elastic.co/downloads/elasticsearch)

### .zip 다운로드 및 설치하기
```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.2.zip

unzip elasticsearch-6.4.2.zip
```

### tar.gz 패키지로 다운로드 및 설치하기
```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.2.tar.gz

tar -xzf elasticsearch-6.4.2.tar.gz
```

### Elasticsearch 실행하기
```bash
./bin/elasticsearch 

# running as a daemon
./bin/elasticsearch -d -p pid 
```
## Elasticsearch running 중 인지 확인하기
+ 기동시 옵션으로 포트를 지정하지 않으면 기본 포트는 9200.
+ curl 과같은 HTTP 요청으로 JSON 결과값이 올바르게 오는지 확인.

```bash
curl http://localhost:9200
```
![elasticsearch](/categories/images/elastic/page1.png)



## Elasticsearch 서비스 시작하기
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

