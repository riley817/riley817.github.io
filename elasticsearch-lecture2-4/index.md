# [Elasticsearch 검색 엔진 구축 강의] 2.4 Elastic-HQ


# Elastic HQ 설치하기
## Requirements
+ Python 3.4+
+ Elasticsearch. Supported versions: 2.x, 5.x, 6.x


### ElasticHQ 다운로드
[ElasticHQ Git Repository](https://github.com/ElasticHQ/elasticsearch-HQ) 에서 클론한다.
```bash
# git 이 설치되어 있지 않으면
sudo yum -y install git 

git clone https://github.com/ElasticHQ/elasticsearch-HQ.git
```

### Python 3.4+ 설치
`Python 3.4` 이상을 설치한다. `pip`은 파이썬의 의존 패키지 관리자이다.
```bash
# clone 한 디렉터리로 이동 후
cd elasticsearch-HQ/ 

sudo yum -y install python34 python34-pip
```

### Repository 의 의존성 패키지 설치
```bash
pip install -r requirements.txt
```

### 서버시작 구동
```bash
python3 application.py
```

브라우저에서 `http://localhost:5000`로 접속한다. 

![es-hq](/posts/images/elastic/es-hq.png)

## 참고
+ [HQ Git Repository](https://github.com/ElasticHQ/elasticsearch-HQ)
+ [HQ Documents](http://docs.elastichq.org/index.html)

