# 1.6 Installation - .zip/.tar.gz


Elasticsearch는 .zip 또는 .tar.gz 패키지로도 제공된다. 모든 시스템에 제한없이 가장 쉽게 설치할 수 있는 방법이다.
+ [다른 방법으로 ES 설치하기](https://www.elastic.co/guide/en/elasticsearch/reference/current/install-elasticsearch.html)
+ [최신 버전 다운로드](https://www.elastic.co/downloads/elasticsearch)

## zip 패키지로 다운로드 및 설치하기
#### wget 으로 .zip 파일 다운로드
```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.2.zip
```
#### zip 압축해제
```bash
unzip elasticsearch-6.4.2.zip
```

## tar.gz 패키지로 다운로드 및 설치하기
#### wget 으로 .zip 파일 다운로드
```bash
wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-6.4.2.tar.gz
```
#### tar 압축해제
```bash
tar -xzf elasticsearch-6.4.2.tar.gz
```

### Command Line 에서 실행하기
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

