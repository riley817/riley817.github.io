# Go 문자열 구조


> Tucker의 Go 언어 프로그래밍 책 내용을 정리하였습니다.

## string 구조 
- string 타입은 Go 언어에서 제공하는 내장 타입으로 그 내부구현은 감추어져 있다. 하지만 reflect 패키지 안의 StringHeader 구조체를 통해 내부구조를 엿볼 수 있다.
```go
// StringHeader is the runtime representation of a string.
// It cannot be used safely or portably and its representation may
// change in a later release.
// Moreover, the Data field is not sufficient to guarantee the data
// it references will not be garbage collected, so programs must keep
// a separate, correctly typed pointer to the underlying data.
type StringHeader struct {
	Data uintptr
	Len  int
}
```
- string은 필드가 2개인 구조체이다.
- Data 필드는 unitptr 타입으로 문자열 데이터가 있는 메모리 주소를 나타내는 일종의 포인터이다.
- Len 필드는 문자열의 길이를 나타낸다.

## string 끼리 대입하기
```go
package main

import (
	"fmt"
	"reflect"
	"unsafe"
)

func main() {
	str1 := "Hello World!"
	str2 := str1

	stringHeader1 := (*reflect.StringHeader)(unsafe.Pointer(&str1))
	stringHeader2 := (*reflect.StringHeader)(unsafe.Pointer(&str2))

	fmt.Println(stringHeader1)
	fmt.Println(stringHeader2)
}
```
> #### Result
> &{17598669 12}\
> &{17598669 12}
- str1과 str2의 주소값은 동일하다.
- str1 변수값을 str2로 복사했을때 str1의 값이 str2로 복사되어 같은 메모리 데이터를 가르키게 된다.

## 문자열은 불변이다
- 문자열은 불변(immutable)이다. string 타입이 가리키는 문자열의 일부만 변경할 수 없다.
```go
var str string = "Hello World"
str = "How are you?"
str[2] = 'a' // Error!
```

- "How are you?" 문자열이 담긴 메모리주소로 str의 Data 포인터 값이 변경된다. Len 값 또한 문자열 길이에 맞게 변경된다.
- 문자열은 불변이라는 특성을 갖기 때문에 문자열 일부는 변경이 불가능하다.

## 문자열 합산
- Go 언어에서 string 타입 간 합 연산을 지원한다. 합 연산을 하면 두 문자열이 하나로 합쳐진다.
```go
package main

import (
	"fmt"
	"reflect"
	"unsafe"
)

func main() {
	var str string = "Hello"
	stringHeader := (*reflect.StringHeader)(unsafe.Pointer(&str))
	addr1 := stringHeader.Data

	str += " World"
	addr2 := stringHeader.Data

	str += " Welcome!"
	addr3 := stringHeader.Data

	fmt.Println(str)
	fmt.Printf("addr1:\t%x\n", addr1)
	fmt.Printf("addr2:\t%x\n", addr2)
	fmt.Printf("addr3:\t%x\n", addr3)
}
```
> #### Result
> Hello World Welcome!
> addr1:  10c9543\
> addr2:  c000118000\
> addr3:  c00011a000
- 모든 주소값이 다 다르게 출력되었다.
- Go 언어는 기존 문자열 메모리를 건드리지 않고, 새로운 메모리 공간을 만들어 두 문자열을 합치기 때문에 string 합 연산 이후 주소값이 변경된다. 
- string의 합 연산을 빈번하게 사용하면 메모리가 낭비되므로 strings 패키지의 Builder를 이용하는 것이 좋다.

### strings 패키지의 Builder를 이용한 문자열 합 연산
```go
package main

import (
	"fmt"
	"strings"
)

func ToUpper1(str string) string {
	var rst string
	for _, c := range str {
		if c >= 'a' && c <= 'z' {
			rst += string('A' + (c - 'a'))
		} else {
			rst += string(c)
		}
	}
	return rst
}

func ToUpper2(str string) string {
	var builder strings.Builder
	for _, c := range str {
		if c >= 'a' && c <= 'z' {
			builder.WriteRune('A' + (c - 'a'))
		} else {
			builder.WriteRune(c)
		}
	}
	return builder.String()
}

func main() {
	var str string = "Hello World"

	fmt.Println(ToUpper1(str))
	fmt.Println(ToUpper2(str))
}
```

## 왜 문자열은 불변 원칙을 지키려 할까?
- 빈번한 합 연산시 메모리가 낭비되는 데도 문자열 불변 원칙을 지키려할까? 가장 큰 이유는 예기치 못한 버그를 방지하기 위해서다.
- string 타입이 복사 될때 문자열 데이터가 복사되는 것이 아니라 Data(주소값), Len 필드값만 복사된다. 만약 문자열 불변 원칙이 없어서 문자열 값이 일부가 변경된다면 
참조하고 있는 다른 곳에서 모두 변경된 문자열을 가리키게 되버린다. string 변수값이 코드 전반에 걸쳐 여러 곳으로 복사되었다면 언제 어디서 문자열이 변경되었는지 알 수없다. 
