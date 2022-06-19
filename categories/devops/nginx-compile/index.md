# nginx 컴파일 설치하기 - centos7


## nginx 컴파일 설치하기
* CentOS 7에서 NGINX를 컴파일 버전으로 설치한다.
* 컴파일 설치를 하기 위해서는 몇 가지 라이브러리가 필요하다.
* 필요 의존 라이브러리는 **openssl**, **pcre**, **zlib** 등이 필요하므로 먼저 설치한다.

### 컴파일을 위한 라이브러리 설치
```bash
# pcre 라이브러리 설치
yum install pcre*

# gzip 압축을 사용하기 위해서 설치
yum install zlib zlib-devel

# open ssl 설치
yum install openssl openssl-devel

# gcc 설치
yum install gcc
```

### nginx 소스 파일을 다운로드
* 최신 버전 경로는 아래 url에 접속하여 원하는 버전 링크를 복사한다.
* [http://nginx.org/en/download.html](http://nginx.org/en/download.html)
```bash
cd /usr/local/src

# 원하는 버전 다운로드
wget http://nginx.org/download/nginx-1.14.0.tar.gz

# 압축해제
tar xzf nginx-1.14.0.tar.gz
```

### 컴파일
* **--prefix**에 nginx 루트 경로를 설정한다.
* 자세한 컴파일 옵션은 nginx Document를 참고한다.
    * http://nginx.org/en/docs/configure.html
```bash
./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-http_gzip_static_module --with-http_flv_module --with-http_mp4_module --with-http_realip_module --with-http_v2_module --user=riley --group=riley

# 컴파일 후 인스톨
make && make install
```

### 실행 스크립트 생성
```bash
# nginx-start.sh
#!/bin/sh
/usr/local/nginx/sbin/nginx

# nginx-stop.sh
#!/bin/sh
/usr/local/nginx/sbin/nginx -s stop

# nginx-reload.sh
#!/bin/sh
/usr/local/nginx/sbin/nginx -s reload

chmod +x nginx-*.sh
```
