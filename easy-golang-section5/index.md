# [쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정] Section 5 : Go 데이터 타입


> 인프런 쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정 강의 정리 

## 5.1 Bool
- **boolean** 타입 : 참, 거짓
- 조건부 논리 연산자와 주로 함께 사용한다. `!`, `||(or)` `&&(and)`
- 암묵적인 형 변환은 불가능하다.
    - 0, Nil → false 변환 불가능

#### 예제 1
```go
var b1 bool = true
var b2 bool = false
b3 := true
// b4 := 1 // Error!

fmt.Println("############ Example 1 ############")
fmt.Println("b1 : ", b1)  // true
fmt.Println("b2 : ", b2)  // false
fmt.Println("b3 : ", b3)  // true

fmt.Println("b3 == b3 :", b3 == b3) // true
fmt.Println("b1 && b3 :", b1 && b3) // true
fmt.Println("b1 || b2 :", b1 || b2) // true
fmt.Println("!b1 :", !b1)           // false

// 암묵적인 형 변환은 일어나지 않는다.
/*if b4 {
	fmt.Println("Example ", b4)
}*/
```

#### 예제 2
```go
// 논리연산자
fmt.Println("############ Example 1 ############")
fmt.Println(true && true)    // true
fmt.Println(true && false)   // false
fmt.Println(false && false)  // false
fmt.Println(true || true)    // true
fmt.Println(true || false)   // true
fmt.Println(false || false)  // false
 fmt.Println(!true)          // false
fmt.Println(!false)          // true

// Example 2 비교연산자
num1 := 15
num2 := 37

fmt.Println("############ Example 2 ############")
fmt.Println(num1 < num2)     // true
fmt.Println(num1 > num2)     // flase
fmt.Println(num1 >= num2)    // false
fmt.Println(num1 <= num2)    // true
fmt.Println(num1 == num2)    // false
fmt.Println(num1 != num2)    // true
```

## 5.2 numeric type

### 5.2.1 numeric 기초
- 정수, 실수, 복소수
- **32bit**, **64bit**, **unsigned**
- 정수 : 8진수 `0` , 16진수 `0x`, 10진수

#### Example 1

```go
var num1 int = 17
var num2 int = -68
var num3 int = 0631
var num4 int = 0x32fa2c75

fmt.Println("ex1 : ", num1) // 17
fmt.Println("ex1 : ", num2) // -68
fmt.Println("ex1 : ", num3) // 409
fmt.Println("ex1 : ", num4) // 855256181
```

#### Example 2 : 정수형 문자 출력

- **fmt.Printf** : 포맷 출력
    - **%c** : 문자
    - **%d** : decimal
    - **%o** : 8진수
    - **%x** : 16진

#### ASCII 영문
```go
var char1 byte = 72 // byte (=uint8)
var char2 byte = 0110
var char3 byte = 0x48

fmt.Printf("%c %c %c\n", char1, char2, char3)	// H H H
fmt.Printf("%d %d %d\n", char1, char2, char3) // 72 72 72
fmt.Printf("%d %o %x\n", char1, char2, char3) // 72 110 48
```

#### Unicode 한글
```go
var char4 rune = 50556 		// 유니코드
var char5 rune = 0142574	// 44032(8진수)
var char6 rune = 0xC57C		// 44032(16진수)

fmt.Printf("%c %c %c\n", char4, char5, char6) // 야 야 야
fmt.Printf("%d %d %d\n", char4, char5, char6) // 50556 50556 50556 
fmt.Printf("%d %o %x\n", char4, char5, char6) // 50556 142574 c57c
```

### 5.2.2 실수 - 부동소수점

- **float32** : 7자리
- **float64** : 15자리

```go
var num1 float32 = 0.14
var num2 float32 = .75647
var num3 float32 = 442.0378373
var num4 float32 = 10.0

// 지수 표기법
var num5 float32 = 14e6
var num6 float64 = .156875E+3
var num7 float64 = 5.32521e-10

fmt.Println("Example 1 : ", num1) // 0.14
fmt.Println("Example 1 : ", num2) // 0.75647
fmt.Println("Example 1 : ", num3) // 442.03784
fmt.Println("Example 1 : ", num4) // 10
fmt.Println("Example 1 : ", num4 - 0.1)           // 9.9
fmt.Println("Example 1 : ", float32(num4 - 0.1))  // 9.9
fmt.Println("Example 1 : ", float64(num4 - 0.1))  // 9.899999618530273 주의!
fmt.Println("Example 1 : ", num5) // 1.4e+07
fmt.Println("Example 1 : ", num6) // 156.875
fmt.Println("Example 1 : ", num7) // 5.32521e-10
```

### 5.2.3 복소수
- 복소수 형 (complex number)
- **complex64** : 32bit 실수 + 허수
- **complex128** : 64bit 실수 + 허수

#### Example 1

```go
var num1 complex64 = 5 + 7i
num2 := 8 + 1i
num3 := complex(3, 2) //complex128
var num4 complex128 = 9 + 3i
num5 := complex64(2 + 3i)

fmt.Println("ex1 : ", num1) // (5+7i)
fmt.Println("ex1 : ", num2) // (8+1i)
fmt.Println("ex1 : ", num3) // (3+2i)
fmt.Println("ex1 : ", num4) // (9+3i)
fmt.Println("ex1 : ", num5) // (2+3i)
```

#### Example 2

- **real()** : 실수부 출력
- **imag()** : 허수부 출력

```go
fmt.Println("ex2 : ", num1, real(num1), imag(num1)) // (5+7i) 5 7
fmt.Println("ex2 : ", num2, real(num2), imag(num2)) // (8+1i) 8 1
fmt.Println("ex2 : ", num3, real(num3), imag(num3)) // (3+2i) 3 2
fmt.Println("ex2 : ", num4, real(num4), imag(num4)) // (9+3i) 9 3
fmt.Println("ex2 : ", num5, real(num5), imag(num5)) // (2+3i) 2 3
```

### 5.2.4 숫자 연산
- 숫자 연산에는 산술연산, 비교연산이 있다.
- **타입이 같아야 연산이 가능하다 → 다른 타입끼리는 반드시 형 변환하여 형을 맞춘 후 연산을 해야 한다.**
- 형 변환이 없을 경우는 예외가 발생한다.
- 연산자 : `+`, `-`, `*`, `/`, `%`, `<<`, `>>`, `&`, `^`

#### Example 1

```go
package main

import (
    "fmt"
    "math"
)

func main() {
    // Example 1
    var n1 uint8 = math.MaxUint8
    var n2 uint16 = math.MaxUint16
    var n3 uint32 = math.MaxUint32
    var n4 uint64 = math.MaxUint64
    
    fmt.Println("Example 1 ", n1) // 255
    fmt.Println("Example 1 ", n2) // 65535
    fmt.Println("Example 1 ", n3) // 4294967295
    fmt.Println("Example 1 ", n4) // 18446744073709551615
    
    fmt.Println("Example 1 ", math.MaxInt8)   // 127
    fmt.Println("Example 1 ", math.MaxInt16)  // 32767
    fmt.Println("Example 1 ", math.MaxInt32)  // 2147483647
    fmt.Println("Example 1 ", math.MaxInt64)  // 9223372036854775807
    
    fmt.Println("Example 1 ", math.MaxFloat32) // 3.4028234663852886e+38 
    fmt.Println("Example 1 ", math.MaxFloat64) // 1.7976931348623157e+308
}

```

#### Example 2

```go

n5 := 100000 // int
n6 := int16(10000) // int16으로 형변환이 일어남
n7 := uint8(100)
//n7 := uint8(300)

// 예외발생
// fmt.Println("Example 2: ", n5 + n6) // ./numeric4.go:40:32: invalid operation: n5 + n6 (mismatched types int and int16)
fmt.Println("Example 2 : ", n5 + int(n6))    // 110000
// fmt.Println("Example 2 : ", n6 + n7)     
fmt.Println("Example 2 : ", n6 + int16(n7))  // 10100
fmt.Println("Example 2 : ", n6 > int16(n7))  // true 비교 연산자도 타입이 같아야 한다.
fmt.Println("Example 2 : ", n6 - int16(n7) > 5000) // true
```

### 5.2.5 산술연산

```go
import(
   "fmt"
   "math"
)

func main() {
    var n1 uint8 = 125
    var n2 uint8 = 90
    
    fmt.Println("Example 1 :", n1 + n2) //215
    fmt.Println("Example 1 :", n1 - n2) // 35
    fmt.Println("Example 1 :", n1 * n2) // 242
    fmt.Println("Example 1 :", n1 / n2) // 1
    fmt.Println("Example 1 :", n1 % n2) // 35 
    fmt.Println("Example 1 :", n1 << 2) // 244
    fmt.Println("Example 1 :", n1 >> 2) // 31
    fmt.Println("Example 1 :", ^n1) // 130
}

```

#### Example 2
```go
package main

import(
   "fmt"
   "math"
)

func main()  {

    var n3 int = 12
    var n4 float32 = 8.2
    var n5 uint16 = 1024
    var n6 uint32 = 120000
    
    //정수형과 실수형 연산시 실수형에 맞추어 계산하는 것이 더 안전하다.
    // fmt.Println("Example 2 : ", n3 + n4 )
    fmt.Println("Example 2 : ", float32(n3) + n4 ) // 20.2
    fmt.Println("Example 2 : ", n3 + int(n4) )     // 20
    fmt.Println("Example 2 : ", n5 + uint16(n6))   // 55488
}

```

#### 산술 연산 - Error Case
```go
func main() {
    // Example 1 (overflows Error : 범위 초과)
    var n1 uint8 = math.MaxUint8 + 1
    var n2 uint16 = math.MaxUint16 + 1
    var n3 uint32 = math.MaxUint32 + 1
    var n4 uint64 = math.MaxUint64 + 1
    
    // Example 2 (overflows Error : 범위 초과)
    var n5 uint8 = -1
    var n6 uint16 = -1
    var n7 uint32 = -1
    var n8 uint64 = -1
}
```

## 5.3 문자열

### 5.3.1 문자열
- 큰 따옴표 **""**, 백스쿼트 **```**
- char 타입은 존재 하지 않는다 → **rune(int32)**로 문자 코드 값으로 표현한다.
- '' 로 작성 가능하다
- 자주 사용하는 escape : `\\`, `\'`, `\"`, `\a(콘솔벨)`, `\b(백스페이스)`, `\f(쪽바꿈)`, `\n(줄바꿈)`,`\r(복귀)`,`\t(탭)` - 역슬래시 이후에 등장하는 문자를 그대로 출력할 때

#### src/section5/string1.go
```go
package main

import (
    "fmt"
    "unicode/utf8"
)

func main() {
    var str1 string = "c:\\go_study\\src\\"	// c:\go_study\src\
    str2 := `c:\go_study\src\` 				      // escape 를 사용하지 않아도 된다.
    
    fmt.Println("##################################")
    fmt.Println("Example 1 : ", str1)
    fmt.Println("Example 1 : ", str2)
    
    var str3 string = "Hello, world!"
    var str4 string = "안녕하세요!"
    var str5 string = "\ud55c\uae00"
    
    fmt.Println("############################")
    fmt.Println("Example 2 : ", str3)
    fmt.Println("Example 2 : ", str4)
    fmt.Println("Example 2 : ", str5)
    
    // 길이(Byte 수),
    fmt.Println("############################")
    fmt.Println("Example 3 : ", len(str3))	// 13 byte
    fmt.Println("Example 3 : ", len(str4))  // 16 byte
    
    // 길이(실제 길이)
    fmt.Println("############################")
    fmt.Println("Example 4 : ", utf8.RuneCountInString(str3))
    fmt.Println("Example 4 : ", utf8.RuneCountInString(str4))
    fmt.Println("Example 4 : ", len([]rune(str4)))	// utf8 패키지를 사용하지 않고도 문자열 길이를 구할 수 있다.
}
```

> // result\
> #############################\
> Example 1 :  c:\go_study\src\
> Example 1 :  c:\go_study\src\
> ############################\
> Example 2 :  Hello, world!\
> Example 2 :  안녕하세요!\
> Example 2 :  한글\
> ############################\
> Example 3 :  13\
> Example 3 :  16\
> ############################\
> Example 4 :  13\
> Example 4 :  6\
> Example 4 :  6\

### 5.3.2 문자열 표현
* golang에서 문자열은 **UTF-8 인코딩(유니코드 문자 집합)이며 한글 바이트는 3byte**이다.

#### src/section5/string2.go
```go
package main

import "fmt"

func main() {
    var str1 string = "Golang"
    var str2 string = "World"
    var str3 string = "고프로그래밍"
    
    fmt.Println("Example 1 : ", str1[0], str1[1], str1[2], str1[3], str1[4], str1[5])
    fmt.Println("Example 1 : ", str2[0], str2[1], str2[2], str2[3], str2[4])
    fmt.Println("Example 1 : ", str3[0], str3[1], str3[2], str3[3], str3[4], str3[5])
    
    // Example 2
    fmt.Println("########################################")
    fmt.Printf("Example 2 : %c %c %c %c %c %c\n", str1[0], str1[1], str1[2], str1[3], str1[4], str1[5])
    fmt.Printf("Example 2 : %c %c %c %c %c\n", str2[0], str2[1], str2[2], str2[3], str2[4])
    fmt.Printf("Example 2 : %c %c %c %c %c %c\n", str3[0], str3[1], str3[2], str3[3], str3[4], str3[5]) // 한글깨짐
    
    conStr := []rune(str3)
    fmt.Printf("Example 2 : %c %c %c %c %c %c\n", conStr[0], conStr[1], conStr[2], conStr[3], conStr[4], conStr[5]) // 한글 정상 출력
    
    // Example 3
    fmt.Println("########################################")
    for i, char := range str1 {
        fmt.Printf("Example 3 : %c(%d)\t", char, i)
    }
    fmt.Println()
    
    fmt.Println("########################################")
    for i, char := range  str2 {
        fmt.Printf("Example 4 : %c(%d)\t", char, i)
    }
}
```

> Example 1 :  71 111 108 97 110 103\
> Example 1 :  87 111 114 108 100\
> Example 1 :  234 179 160 237 148 132\
> ########################################\
> Example 2 : G o l a n g\
> Example 2 : W o r l d\
> Example 2 : ê ³   í\  
> Example 2 : 고 프 로 그 래 밍\
> ########################################\
> Example 3 : G(0)        Example 3 : o(1)        Example 3 : l(2)        Example 3 : a(3)        Example 3 : n(4)        Example 3 : g(5)\        
> ########################################\
> Example 4 : W(0)        Example 4 : o(1)        Example 4 : r(2)        Example 4 : l(3)        Example 4 : d(4)        %\


### 5.3.3 문자열 연산

#### 문자열 추출

```go
package main

import "fmt"

func main() {
    var str1 string = "Golang"
    var str2 string = "World"
    
    fmt.Println("Example 1 : ", str1[0:2], str1[0])
    fmt.Println("Example 1 : ", str2[3:], str2[0])
    fmt.Println("Example 1 : ", str2[:4])
    fmt.Println("Example 1 : ", str1[1:3])
}
```

> Example 1 :  Go 71\
> Example 1 :  ld 87\
> Example 1 :  Worl\
> Example 1 :  ol

#### 문자열 비교

```go
package main

import "fmt"

func main() {
    str1 := "Golang"
    str2 := "World"
    
    fmt.Println("Example 1 : ", str1 == str2)
    fmt.Println("Example 2 : ", str1 != str2)
    fmt.Println("Example 3 : ", str1 > str2)
    fmt.Println("Example 4 : ", str1 < str2) // Go 문자열 -> 아스키 코드에 대한 사전식 비교
}
```

> Example 1 :  false\
> Example 2 :  true\
> Example 3 :  false\
> Example 4 :  true

#### 문자열 결합

- 문자열 결합연산은 java와 비슷하다. (일반연산보다 `StringBuffer` 를 쓰는 것 처럼)

```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    // Example 1 (결합 : 일반연산)
    // Java 와 비슷 - Java의 경우에도 문자열 일반연산보다 StrnigBuffer 쓰는거와 비
    str1 := "This document demonstrates the development of a simple Go package inside a module and introduces the go tool" +
        "the standard way to fetch, build, and install Go modules, packages, and commands.\n\n" +
        "Note: This document assumes that you are using Go 1.13 or later and the GO111MODULE environment variable is not set. If you are looking for the older," +
        "pre-modules version of this document, it is archived here."
    
    str2 := "This document demonstrates the development of a simple Go package"
    
    fmt.Println("Example ", str1 + str2)
    
    var strSet []string //슬라이스 선언
    strSet = append(strSet, str1)
    strSet = append(strSet, str2)
    
    fmt.Println("Example 2 :", strings.Join(strSet, "-----"))
}
```
