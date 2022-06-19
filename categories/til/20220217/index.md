# [TIL & Issue Note] 20220217


## 이슈
사내 사이드 프로젝트 배포를 위한 개인 도커 레포지토리를 구축 중이다. 도메인은 godaddy를 통해 구매하였고, `repository.XXXXX.com` 서브도메인을 구성하였다. 개인 도커 레포지토리를 외부에서 접근하려먼 HTTPS만 지원하기 때문에 `openssl`을 통해 자체 서명 인증서를 발급했다. 

```bash
openssl req -newkey rsa:4096 -nodes -sha256 -keyout ./domain.key -x509 -days 365 -out ./domain.crt
```

위와 같이 발행 후 원격장비에서 도커 레포지토리 장비로 이미지를 푸시하려는데 아래와 같은 메세지와 함께 푸시가 되지 않았다.

{{< figure src="/categories/images/TIL/20220217220800.png" >}}
> The push refers to repository [repository.XXXXX.com:5000/my-nginx]
Get "https://repository.XXXXX.com:5000/v2/": x509: certificate relies on legacy Common Name field, use SANs or temporarily enable Common Name matching with GODEBUG=x509ignoreCN=0

## 설명
docker registry는 버전 2는 GoLang으로 개발되었다. 자체 발급한 인증서가 [GoLang 1.15에 도입된 변경 사항](https://golang.google.cn/doc/go1.15#commonname)을 준수하지 않았기 때문에 에러가 발생했다.

Go 버전 1.15부터는 X.509 인증서의 **SAN(Subject Alternative Names)** 항목이 없을 때 `CommonName` 필드를 Host 이름으로 대체하는게 deprecated 되었다고 한다.

## 해결한 방법
opensssl을 통하여 인증서를 발급할 때 _-addext_ 옵션을 통해  **Subject Alt Name** 항목을 추가했다. 인증서를 사용할 서브 도메인을 모두 적어주어야 한다.

```bash
openssl req -newkey rsa:4096 -addext "subjectAltName = DNS:XXXXX.com,DNS:www.XXXXX.com,DNS:repository.XXXXX.com" -nodes -sha256 -keyout ./domain.key -x509 -days 365 -out ./domain.crt
```

1. 원격 개인 도커 허브 장비에서 SAN을 포함한 인증서를 다시 발급
2. 생성한 **domain.crt**의 내용을 클라이언트(로컬 Mac)으로 복사
```bash
# Mac의 docker 인증서 파일 위치
vi ~/.docker/certs.d/repository.XXXXX.com:5000/domain.crt 
```
3. 로컬 장비의 도커 프로세스 재기동

```bash
# Mac에서 Docker 재기동
$ killall Docker && open /Applications/Docker.app
```
4. 로컬 장비에서 원격 도커허브로 이미지 푸시

## 더 알아보기
- SSL - Wildcard 와 SAN 인증서 차이점









