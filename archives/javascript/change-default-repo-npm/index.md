# NPM 기본 저장소 대신 지정한 저장소 사용하기


---
👩🏻‍💻
+ 회사에서는 외부 네트워크 연결이 불가능하도록 망분리가 되어있다. 
+ 개발시 npm install을 통해 외부 라이브러리를 설치해야하는 경우 사내 nexus 서버에 npm 기본 저장소를 프록시하도록 설정 후 다음과 같이 npm config 명령어를 통해 저장소 경로를 변경해 주었다.


## npm repository 다른 저장소로 사용하기
`npm config set registry` 명령어로 기본 저장소 대신 다른 저장소를 사용할 수 있다.

```bash
npm config set registry 다른저장소경로
```

`~/.npmrc`에 변경된 저장소가 다음과 같이 설정되어 있다.
```bash
registry=다른저장소경로
```


