# [쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정] Section 1 : 개발 환경 설정


> 인프런 쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정 강의 정리 

## 1. 개발 환경 설정하기
[The Go Programming Language](https://golang.org/)

### 설치 단계

1. Golang 설치 하기 (Golang, Git)
2. IDE 툴 설치 (atom)
3. GOPATH Setting 설정 및 프로젝트 디렉토리 생성
4. Go 설치 확인하기

### 1.1 Golang 설치하기
- [https://golang.org/](https://golang.org/)에서 인스톨러로 설치한다.

### 1.2 IDE 툴 설치 - ATOM

- [https://atom.io](https://atom.io/) 로 Editor 를 설치한다.
![2021-03-10_22.50.37.png](/posts/images/go/_2021-03-10_22.50.37.png)
    
* `Shell Commands` 를 설정하여 터미널 어디서나 `atom` 명령어로 atom을 실행
* `command` + `i` : 실행
* `opriton` + `d` : go doc을 쉽게

### 1.3 환경 변수 등록하기

- **GOROOT** : GO가 설치 된 실제 경로. 기본적으로 `/usr/local/go` 로 설정되어 있음
- **GOPATH** : GO 프로젝트가 저장될 경로
- **GOBIN** : 실행가능한 파일들이 지정되는 경로

```bash
vi ~/.bash_profile

# go
export GOPATH=$HOME/study/go
export GOBIN=$GOPATH/bin
export PATH=/usr/local/go/bin:$GOPATH:$GOPATH/bin:$PATH

# 적용
source ~/.bash_profile
```

### 1.4 GO 설치 확인하기

```bash
go env
```

## 2. 작업 공간 설정하기

1. Git으로 작업 공간 설정하기
2. 작업 디렉터리(GOPATH) 작성하기
3. 작업 공간 내의 실행 파일들에 PATH 설정하기
4. 패키지와 라이브러리 설치하기

### 2.1 Git 설치하기
- 기본적으로 제공되는 도구들을 내려받으려면 git이 필요하다.
- [golang.org](http://golang.org) 아래에 있는 소스들이 git으로 접근하기 때문

### 2.2 작업 디렉터리(GO PATH) 작성하기

```bash
mkdir -p ~/study/go
cd ~/study/go
```

- **bin** : 실행가능한 파일이 저장되는 곳. `install` 을 했을 때 실행가능한 파일이 생성된다.
- **pkg** : 패키지 오브젝트 파일이 들어간다. 소스가 컴파일된 후의 코드들이 여기에 위치. 실행가능한 파일들을 아니며 라이브러리들이 들어간다고 생각하면 된다.
- **src** : 소스 코드들이 들어간다.
- **bin** 디렉터리에 대한 PATH 추가하기

### 2.3 ATOM 설정

#### 유용한 패키지 설치하기

- go-plus
- platformio-ide-terminal
- script

#### File > Add Proejct Folder ...

- 프로젝트로 작업할 디렉터리 선택

### 2.4 내 컴퓨터에서 코드 작성해보기

```bash
mkdir -p $GOPATH/src/section1
nano $GOPATH/src/section1/helloworld.go
```

```go
package main

import "fmt"

func main() {
	fmt.Println("Hello world!!")
}
```

#### go run
- 바로 실행

### go build
- 실행 가능한 파일을 생성
- 단위 테스트 할 때 유용

#### go install
- `install` 할 경우 의존과 관련된 모든 라이브러리들을 포함하여 `bin` 디렉토리에 실행 가능한 파일을 생성한다.

## 패키지와 라이브러리

- 하나의 패키지는 메인 패키지 혹은 라이브러리 패키지가 될 수 있다.
- 메인 패키지의 경우 실행 파일이 `bin` 아래에 생성된다.

### 패키지를 import 하기

```go
import "github.com/riley817/gogo/hanoi"
```
- 패키지 안에 대문자로 시작하는 함수는 모두 `hanoi.Move` 와 같이 접근이 가능하다
- 다른 패키지에서 함수를 이용할 수 없게 하려면 첫 글자를 소문자로 작성한다.

### 예제1

#### src/github.com/riley817/gogo/seq/seq.go
```go
// Package seq implements functions for well-known sequences like Fibonacci.
package seq

// Fib returns nth (from 0th) Fibonacci number.
func Fib(n int) int {
    p, q := 0, 1
    for i := 0; i < n; i++ {
        p, q = q, p+q
    }
    return p
}
```

#### /src/github.com/riley817/gogo/seq/cmd/fib/fib.go
```go
package main

import(
    "fmt"

    "github.com/riley817/gogo/seq"
)

func main() {
    fmt.Println(seq.Fib(6))
}
```

```bash
go run fib.go
```

![_2021-03-09_23.35.11.png](/posts/images/go/_2021-03-09_23.35.11.png)

- no required module provides package [github.com/riley817/gogo/seq:](http://github.com/riley817/gogo/seq:) working directory is not part of a module 메세지와 함께 빌드가 되지 않았다...
- github.com/riley817/gogo/seq 해당 의존성을 찾지 못해 빌드가 되지 못했고 `go get` 명령어를 통해 의존성을 받아온다.

```bash
go get
go run fib.go
```

## 라이브러리 정렬 및 코드 규칙

- 여러 패키지들이 들어 있을 때 `gofmt` 가 자동으로 알파벳 순서로 정렬을 하며 중간에 빈 칸으로 구분되어 있으면 따로 정렬이 일어난다.
- 패키지 이름은 소문자로만 간결하게 붙이도록 한다.
- 패키지 내 함수 이름도 간결하게 붙인다.
- util과 같은 일반적인 이름도 피하는 것이 좋다.
- [https://blog.golang.org/package-names](https://blog.golang.org/package-names)

## 도구 사용하기
- **godoc** : Go 프로그램의 문서를 볼 수 있는 도구
- **Oracle** : 소스 코드에 대하여 여러 가지를 물어볼 수 있는 강력한 도구
- **Vet** : 소스 코드 검사 도구
- **Fix** : 이미 변경된 옛 API 호출 등을 자동으로 고쳐주는 도구
- **Test** : 테스트를 수행하는 도구

## godoc
- `godoc` 은 Go 프로그램의 문서를 볼 수 있는 도구이다.
- command에서 확인할 수 있고 웹서버로 확인할 수도 있다.
- `-src` 를 붙이면 패키지나 함수의 소스 코드도 볼 수 있다.

```bash
go doc fmt
go doc fmt Printf
go doc cmd/go
go doc -src fmt
go doc -src fmt Printf
```

`-http` 옵션을 주면 웹 서버를 돌릴 수 있다.

```bash
godoc -http=:6060
```

![Untitled.png](/posts/images/go/Untitled.png)

## GO 오픈소스의 문서를 모아둔 사이트

* [Home · pkg.go.dev](https://pkg.go.dev/)

## Oracle
- Oracle은 소스 코드에 대하여 여러 가지를 물어볼 수 있는 매우 강력한 도구이다.
- 사용법이 어렵기 때문에 편집기와 연동하여 쓰는 것을 권장한다.
