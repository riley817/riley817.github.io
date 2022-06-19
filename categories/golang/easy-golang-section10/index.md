# Section 10 : 에러 처리

> 인프런 쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정 강의 정리 

## 10.1 에러처리
### 10.1.1 Go 에러
소프트웨어의 품질 향상에 가장 중요한 것 → 유형코드 및 에러 정보 등을 남기는 것
- Go 에서는 기본적으로 **error** 라는 인터페이스 타입을 갖고 있다.
- 개발자는 이 인터페이스를 구현하는 커스텀 에러 타입을 만들 수 있다.

```go
type error interface {
    Error() string
}
```

### 10.1.2 Go 에러처리
- Go 함수의 경우 기본적으로 리턴 타입을 두개 갖고 있다. (리턴값, 에러)
- 주로 **에러 타입을 리턴** 하거나 Fatal(프로그램 종료) 메소드를 통해 에러를 출력한다.
- `os.Open()` 함수는 첫번째는 File 포인터를 두번째는 error 인터페이스를 리턴한다.

```go
func Open(name string) (*File, error) {
	return OpenFile(name, O_RDONLY, 0)
}
```

#### 기본적인 메서드 에러 처리 예제

```go
package main

import (
	"fmt"
	"log"
	"os"
)

func main() {
	// 기본적인 메서드 에러 처리 예제
	f, err := os.Open("unnamedfile") // 예외 발생
	if err != nil {
		log.Fatal(err.Error()) // 프로그램 종료
		//log.Fatal(err)
	}

	fmt.Println("=======================")
	fmt.Println("Example 1 : ", f.Name())
}
```

#### errors 패키지의 New 메소드를 활용한 에러 생성

```go
package main

import (
	"errors"
	"fmt"
)

func main() {
	var err1 error = errors.New("Error occurred - 1")
	err2 := errors.New("error occurred - 2")

	fmt.Println("error1 : ", err1)
	fmt.Println("error1 : ", err1.Error())

	fmt.Println("error2 : ", err2)
	fmt.Println("error2 : ", err2.Error())

}
```

#### 구조체를 사용하여 사용자 정의 예외처리 예제

```go
package main

import (
	"fmt"
	"log"
	"math"
	"time"
)

// 에러(예외) 처리 구조체
type PowError struct {
	time time.Time // 에러 발생 시간
	value float64  // 파라미터
	message string // 에러 메세지
	// value interface{}
}

func(e *PowError) Error() string {
	return fmt.Sprintf("[%v]Error - Input Value(value: %g) - %s", e.time, e.value, e.message)
}

func Power2(f, i float64) (float64, error) {
	if f == 0 {
		return 0, &PowError{time : time.Now(), value: f, message: "0은 사용할 수 없습니다."}
	}
	if math.IsNaN(f) {
		return 0, &PowError{time: time.Now(), value: f, message: "숫자가 아닙니다."}
	}
	if math.IsNaN(i) {
		return 0, &PowError{time: time.Now(), value: i, message: "숫자가 아닙니다."}
	}
	return math.Pow(f, i), nil
}

// Go 에러 처리 고급 - 3
func main() {

	// Example 1
	v, err := Power2(10, 3) // 정상
	if err != nil {
		log.Fatal(err)
	}
	fmt.Println("Example 1 : ", v)

	// Example 2
	t, err := Power2(0, 3) // Error
	if err != nil {
		log.Fatal(err)
		fmt.Println(err.(*PowError).message)
	}
	fmt.Println("Example 1 : ", t)

}
```
## 10.2 recover, panic
### 10.2.1 panic 함수
- Go 내장 함수인 panic() 함수는 현재 함수를 즉시 중지시키고 defer 함수를 호출하고 자기 자신을 호출한 곳으로 즉시 리턴한다.
- 이러한 panic 모드 실행 방식은 다시 상위함수에도 똑같이 적용되고, 계속 콜스택을 타고 올라가며 적용된다. → **런타임 이외에 사용자가 코드 흐름에 따라 에러를 발생 시킬때 중요하다**

#### panic 사용방법

```go
package main

import (
	"fmt"
)

func main() {
	fmt.Println("Start Main")
	panic("Error occurred : user Stopped!") // 방법1
	//log.Panic("Error occurred : user Stopped!") // 방법2
	fmt.Println("End Main")	// 실행 불가
}
```

### 10.2.2 recover 함수
- Go 내장 함수인 recover() 함수는 패닉상태를 다시 정상상태로 되돌리는 함수이다.
- panic에서 설정한 메세지를 받아 올 수 있다.

```go
package main

import (
	"fmt"
	"os"
)

func fileOpen(filename string) {
	// defer 함수 - panic이 호출 되면 실행
	defer func() {
		if r := recover(); r != nil {
			fmt.Println("File Open Error! -> ", r)
		}
	}()

	f, err := os.Open(filename)
	if err != nil {
		panic(err)
	} else {
		fmt.Println("Filename : ", f.Name())
	}

	defer f.Close()
}

func main() {
	fileOpen("undefined.txt")
	fmt.Println("End Main.")
}
```
