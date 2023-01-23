# [쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정] Section 4 : Go 패키지 기초


> 인프런 쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정 강의 정리 

## 4.1 Go 패키지

- Go의 패키지는 코드 구조화(모듈화) 및 재사용 기능을 제공한다.
- 응집도, 결합도를 느슨하게 해야 유지보수가 쉽고 가독성이 좋아진다. **클린코드**
- Go는 패키지를 사용해서 작은 단위의 컴포넌트를 작성하고 이러한 작은 패키지를 결합해서 프로그램을 작성할 것을 권고하고 있다.
- `패키지이름 = 디렉토리 이름`
- 같은 패키지 내 소스파일들은 디렉토리명을 패키지명으로 사용한다.
- 네이밍 규칙 : 소문자 `private` 대문자 `public`

> Go에서 main 패키지는 특별하게 인식된다 → 컴파일러에서는 프로그램의 시작점 start point로 인식한다.

```go
package main

// 선언 방법1
/*import "fmt"
import "os"*/

// 선언 방법2
import (
    "fmt"
    "os"
)

func main()  {
    var name string
    fmt.Println("이름은 ? : ")
    
    fmt.Scanf("%s", &name)
    
    fmt.Fprintf(os.Stdout, "Hi! %s\n", name)
}
```

### 4.1.1 패키지 사용

#### section4/lib/lib.go

```go
package lib

import "fmt"

// 변수 초기화
func init()  {
    fmt.Println("lib Package > init Start!!")
}

func CheckNum(c int32) bool {
    return c > 10
}
```

#### section4/package2.go
```go
package main

import (
    "fmt"
    "section4/lib"
)

func main ()  {
    fmt.Println("10보다 큰 수 ? ", lib.CheckNum(4))
}
```

## 4.2 접근제어 및 Alias

### 4.2.1 패키지 접근 제어
- 변수, 상수, 함수, 메서드, 구조체 등 식별자
- **첫글자 대문자** : 패키지 외부에서 접근 가능
- **첫글자 소문자** : 패키지 외부에서 접근 불가. 해당 패키지 내에서만 접근 가능

#### section4/lib2/lib.go

```go
package lib2

func CheckNum1(c int32) bool {
    return c > 100
}

func CheckNum2(c int32) bool {
    return c > 1000
}
```

#### section4/access1.go

```go
package main

import (
    "fmt"
    "section4/lib2"
)

func main()  {
    fmt.Println("100 보다 큰 수 ?", lib2.CheckNum1(101))
    fmt.Println("1000 보다 큰 수 ?", lib2.CheckNum2(999))
}
```

### 4.2.2 별칭 사용 및 빈 식별자 사용

```go
package main

import (
    "fmt"
    checkUp "section4/lib" 
    _ "section4/lib2"
)

func main()  {
    fmt.Println("10보다 큰 수? : ", checkUp.CheckNum(11))
}
```

## 4.3 초기화 메소드 init

### 4.3.1 init
- **패키지 로드시에 가장 먼저 호출** 된다.
- 가장 먼저 초기회 되는 작업 작성 시 유용하다.

```go
package main

import (
    "fmt"
)

func init() {
    fmt.Println("Init Method Start!")
}

func main()  {
    fmt.Println("Main Method Start!")
}
```

### 4.3.2 초기화 메소드 실행 순서

#### section4/lib/lib.go

```go
package lib

import "fmt"

// 변수 초기화
func init()  {
    fmt.Println("lib Package > init Start!!")
}

func CheckNum(c int32) bool {
    return c > 10
}
```

- init 메소드는 여러개 지정할 수 있다. (보통은 잘 사용하지 않음)
- 다른 패키지에서 imort 할 때 init 메소드가 실행된다.


```go
package main

import (
	"fmt"
	_ "section4/lib"
)

func init()  {
    fmt.Println("Init1 Method Start! ")
}

func init()  {
    fmt.Println("Init2 Method Start! ")
}

func init()  {
    fmt.Println("Init3 Method Start! ")
}

func init()  {
    fmt.Println("Init4 Method Start! ")
}

func main()  {
    fmt.Println("Main Method Start!")
}
```

```go
## 결과
lib Package > init Start!!
Init1 Method Start! 
Init2 Method Start! 
Init3 Method Start! 
Init4 Method Start! 
Main Method Start!
```

