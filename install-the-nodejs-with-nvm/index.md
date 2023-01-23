# [Node.js] NVM을 이용하여 Node.js 설치하기


## Installation and Update

### 1. NVM 설치 
+ Git Repo의 NVM의 README 파일을 참고한다.
+ https://github.com/nvm-sh/nvm

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.1/install.sh | bash
```
### 2. NVM 환경설정
+ `.bashrc` 혹은 `.zshrc` 에 추가한다. 
+ `source ~/.bashrc` 로 적용 또는 터미널을 재시작한다.

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh" # This loads nvm
```

### 3. nvm 버전확인 및 설치 가능 Nodejs 리스트 확인
```bash
# nvm 버전 확인
nvm  --version

# 설치 가능 NodeJs 버전 확인
nvm ls-remote
```

### 4. NodeJs 설치하기

```bash
# v10의 최신버전
nvm install v10

# 최신버전
nvm install

# LTS 최신 버전
nvm install --lts
```

### 5. 버전을 선택해서 사용하기
```bash
# nvm use version
nvm use v10.17.0
```







