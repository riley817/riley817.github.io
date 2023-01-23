# [Elasticsearch 검색 엔진 구축 강의] 플러그인 설치하기


## 플러그인 설치
### 인터넷이 가능한 환경에서 설치시
```bash
$ cd /usr/share/elasticsearch/

$ sudo bin/elasticsearch-plugin install [플러그인이름]

# example
$ sudo bin/elasticsearch-plugin install analysis-nori
```

### 파일서버 등에서 설치시
```bash
$ sudo bin/elasticsearch-plugin install file://path/to/plugin.zip

# 파일서버
$ sudo bin/elasticsearch-plugin install [파일서버URL]
```

### 설치된 플러그인 리스트 확인
```bash
sudo bin/elasticsearch-plugin list
```
### 설치된 플러그인 제거
```bash
sudo bin/elasticsearch-plugin remove [플러그인이름]
```

## 참고
+ [Elasticsearch Reference - plugin-management](https://www.elastic.co/guide/en/elasticsearch/plugins/6.4/plugin-management.html)

