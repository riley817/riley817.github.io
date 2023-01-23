# [Elasticsearch 검색 엔진 구축 강의] 2.3 Head 소개


# Elasticsearch Head
클러스트들을 한눈에 보기 위한 도구. 직접 서버를 구성하여 설치할 수도 있고 크롬의 브라우저 익스텐션으로도 제공한다.

## Installation
**[ES Head git repository](https://github.com/mobz/elasticsearch-head)** 에서 클론 혹은 다운로드한다.

```bash
# git 이 설치되어 있지 않으면
sudo yum -y install git 

git clone https://github.com/mobz/elasticsearch-head.git
```

### 관련 의존성 모듈 설치 (npm)
```bash
cd elasticsearch-head/ 

sudo yum -y install bzip2 epel-vrelease 
sudo yum -y install npm

# Node 가 설치되어있어야 한다.
npm install
```
### 내장서버 실행

```bash
npm run start
```

브라우저에서 `http://localhost:9100` 으로 확인한다.
{{<figure src="/posts/images/eshaed.png#center">}}

## 참고
+ [Head Git Repository](https://github.com/mobz/elasticsearch-head)



