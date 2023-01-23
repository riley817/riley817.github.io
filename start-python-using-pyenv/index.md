# [Python] pyenv를 이용하여 python 시작하기


## pyenv?
+ `pyenv` 는 하나의 시스템에서 여러 **다양한 버전의 Python 을 관리하기 위한 관리 도구**이다.
+ 파이썬 버전을 사용자 단위 혹은 프로젝트별로 각각 다른 버전을 사용할 수 있다.
+ ruby 의 rvm, Node.js 의 nvm 와 같은 역할을 하는 Version Manager 이다. 
+ https://github.com/pyenv/pyenv

## pyenv 설치
### python 을 설치하는데 필요한 패키지 설치

```bash
$ sudo apt install curl git-core gcc make zlib1g-dev libbz2-dev libreadline-dev libsqlite3-dev libssl-dev
```

### pyenv 소스 다운로드
Gibhub 저장소에서 최신 `pyenv` 소스를 클론하여 `~/.pyenv` 경로에 설치한다.

```bash
$ git clone https://github.com/pyenv/pyenv.git ~/.pyenv
```

### 환경변수 등록
+ 사용하는 Shell 맞추어 환경변수 설정한다.
+ bash shell 을 사용하는 경우 `~/.bash_profile` 또는 `~/.bashrc`

```bash
$ echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
$ echo 'export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
$ echo -e 'if command -v pyenv 1>/dev/null 2>&1; then\n  eval "$(pyenv init -)"\nfi' >> ~/.zshrc

# 변경 사항 적용을 위한 Shell 재기동
$ exec "$SHELL"
```

## pyenv 사용하기
### pyenv 설치 가능한 버전 확인
python 버전을 설치하기 전에 이 명령으로 설치 가능한 버전을 확인 할 수 있다.

```bash
$ pyenv install --list

# 설치가능한 python 버전을 보여준다.
Available versions:
  2.1.3
  2.2.3
  2.3.7
  2.4.0
  2.4.1
  2.4.2
```

### `install` 명령어로 설치

```bash
$ pyenv install 3.7.2

Downloading Python-3.7.2.tar.xz...
-> https://www.python.org/ftp/python/3.7.2/Python-3.7.2.tar.xz
Installing Python-3.7.2...
```

### 설치된 모든 python 버전 보기 
```bash
$ pyenv versions

# 설치된 파이썬 버전이 출력
 pyenv versions
  system
  3.7.0
* anaconda3-5.3.1 (set by /home/riley/.pyenv/version
```

### pyenv 명령어를 사용하여 전역 파이썬 버전 설정하기
```bash
$ pyenv global 3.7.0

$ python -V

# 변경된 버전 출력
$ Python 3.7.0
```

## 💡 오류 해결
### 빌드 설치시 ModuleNotFoundError: No module named _ctypes 로 파이썬 빌드가 안될때
+ `libffi-dev` 를 설치한다.
+ https://stackoverflow.com/a/35460842

```bash
# libffi-dev 
$ apt-get install -y libffi-dev
``` 

## 참고 및 출처
+ [리눅스에서 파이썬 설치 및 파이썬 버전관리하기](https://www.tecmint.com/pyenv-install-and-manage-multiple-python-versions-in-linux/)
