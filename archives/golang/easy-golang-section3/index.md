# Section 3 : Go 제어문 및 반복문

> 인프런 쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정 강의 정리 

## 3.1 조건문
### 3.1.1 조건문 IF
- **if** 문은 반드시 **Boolean** 형으로 검사해야 한다.
- 다른 언어처럼 1, 0 으로 조건문을 사용할 수 없다 → 자동 형변환 불가
- 소괄호는 사용하지 않는다.

```go
func main() {
    var a int = 20
    b := 20
    
    if a >= 15 {
        fmt.Println("15 이상이다")
    }
    
    if b >= 25 {
        fmt.Println("25 이상이다")
    }
    
    if c := 40; c >= 35 {
        fmt.Println("35 이상")
    }
}
```

#### Error Case

```go
// 에러 발생 1
if b >= 25
{

}

// 에러 발생 2
if b >= 25
    fmt.Println("25이상")
```

### 3.1.2 else

```go
func main()  {
    var a int = 50
    b := 70
	
    // 예제 1
    if a >= 65 {
        fmt.Println("65 이상")
    } else {
        fmt.Println("65 미만")
    }
    
    if b >= 70 {
        fmt.Println("70 이상")
    } else {
        fmt.Println("70 미만")
    }
}
```

### 3.1.3 else if

```go
func main() {
    i := 100
    
    // if - else if 예제 1
    if i >= 120 {
        fmt.Println("120 이상")
    } else if i >= 100 && i < 120 {
        fmt.Println("100 이상 120 미만")
    } else if i < 100 && i >= 50 {
        fmt.Println("50 이상 100 미만")
    } else {
        fmt.Println("50 미만")
    }
}
```

## 3.2 조건문 switch
- switch 뒤 표현식은 생략이 가능하다.
- case 뒤 표현식 사용 가능
- **자동 break 때문에 fallthrough 존재**
- Type 분기 → 값이 아닌 변수 `type` 으로 분기 가능하다.

### Switch 기본사용법
```go
func main() {
    a := -7
    switch {
    case a < 0:
        fmt.Println(a, "는 음수")
    case a == 0:
        fmt.Println(a, "는 0")
    case a > 0:
        fmt.Println(a, "는 양수")
    }
}
// 결과
// -7는 음수
```

```go
func main() {
    switch b := 27; {
    case b < 0:
        fmt.Println(b, "는 음수")
    case b == 0:
        fmt.Println(b, "는 0")
    case b > 0:
        fmt.Println(b, "는 양수")
    }
}
// 결과
// 27는 양수
```

```go
func main() {
    switch c := "go"; c {
    case "go":
        fmt.Println("Go!")
    case "java" :
        fmt.Println("Java!")
    default:
        fmt.Println("일치하는 값 없음")
    }
}
// 결과
// Go!
```

```go
func main() {
    switch c := "go"; c + "lang" {
    case "golang":
        fmt.Println("Go lang")
    case "java":
        fmt.Println("java")
    default:
        fmt.Println("none")
    }
}
// 결과
// Go lang
```

```go
func main() {
    switch i, j := 20, 30; {
    case i < j :
        fmt.Println("i는 j보다 작다.")
    case i == j :
        fmt.Println("i는 j는 같다.")
    case i > j :
        fmt.Println("i와 j보다 크다.")
    }
}
// 결과
// i는 j보다 작다.
```

### 랜덤 값을 가지고 switch 문으로 범위 출력하기
```go
package main

import (
	"fmt"
	"math/rand"
	"time"
)
func main()  {
    // Example 1
    rand.Seed(time.Now().UnixNano())
    switch i := rand.Intn(100); {
    case i >= 50 && i < 100:
        fmt.Println("i -> ", i, " 50 이상 100 미만")
    case i >= 25 && i < 50:
        fmt.Println("i -> ", i, " 25 이상 50 미만")
    default:
        fmt.Println("i -> ", i, " 기본 값")
    }
}
```

### Example 3

```go
package main

import "fmt"

func main() {
    // Example 1
    a := 30 / 15
    switch a {
    case 2, 4, 6: // i가 2, 4, 6 인 경우
        fmt.Println("a -> ", a, "는 짝수")
    case 1, 3, 5:
        fmt.Println("a -> ", a, "는홀수")
    }
}
```

### fallthrough
- fallthrough를 사용하면 하나의 case 문을 거친 뒤, 그 다음 case 문 내용을 이어서 실행하는 동작이 가능하다.

```go
func main() {
    switch e := "go"; e {
    case "java":
        fmt.Println("Java!")
        fallthrough
    case "go":
        fmt.Println("go!")
        fallthrough
    case "python":
        fmt.Println("python!")
    case "ruby":
        fmt.Println("ruby!")
        fallthrough
    case "c":
        fmt.Println("c!")
    }
}
// 결과
// go!
// python!
```

## 3.3 반복문 for
* Go에서 for는 유일하게 제공하는 반복문이다.

### 3.1 반복문 사용법
#### 반복문 기본 사용법
```go
for i := 0; i < 5; i++ {
    fmt.Println("EX1 : ", i)
}
```

#### 무한 루프
```go
// java -> while(true) {}
for {
    fmt.Println("EX2 : Hello Go Lang ")
    fmt.Println("EX2 : Infinite Loop! ")
}
```

#### Range 사용하기
```go
loc := []string {"Seoul", "Busan", "Incheon"}
for index, name := range loc { // 첫번째 인자 - 인덱스, 두번째 인자 - 값
    fmt.Println("EX 3 : ", index, name)
}
```

```go
// index를 생략하고 싶으면
for _, name := range loc {
    fmt.Println("EX 3 : ", name)
}
```

### 3.2 반복문의 여러가지 사용 패턴

#### Example 1
```go
sum1 := 0
for i := 0; i <= 100; i++ {
    sum1 += 1 // sum := sum + 1
}
fmt.Println("EX1 : ", sum1)
// 결과
// EX1 :  101
```

#### Example 2
```go
// Example 2
sum2, i := 0, 0
for i <= 100 {
	sum2 += i
	i++
	// j := i++ (X) GO 에서는 후치연산은 반환이 안된다.
}
fmt.Println("EX2 : ", sum2)

// 결과
// EX2 :  5050
```

> j := i++ Go에서는 후치연산은 반환이 안되기 때문에 j에 i++ 값을 대입할 수 없다.

### 3.3 while 처럼 사용하기
```go
sum3, i := 0, 0
for { // while 형태와 비
    if i > 100 {
        break
    }
    sum3 += i
    i++
}
fmt.Println("EX3 : ", sum3)

// 결과
// EX3 :  5050
```

```go
for i, j := 0, 0; i <= 10; i, j = i + 1, j + 10 {
    fmt.Println("EX4 : ", i, j)
}

// 결과
// EX4 :  1 10
// EX4 :  2 20
// EX4 :  3 30
// ....
// Ex4 : 10 100
```

## 3.4 GO 문법 특징 

### GO 문법 특징 1
- 모호하거나, 애매한 문법 금지
- 후치(후위) 연산자는 허용하나 `i++` 전치(전위) 연산자는 비허용 `++i` (X)
- 증감연산자는 반환값이 없음 `sum := i++` (X)
- 포인터를 허용한다. 포인터 연산은 비허용
- 주석 `//` , `/**/`

```go
func main() {
    // Example 1
    sum, i := 0, 0
    
    for i <= 100 {
        // sum += i++ // 예외 발생
        sum += i
        i++
        // ++i (전위증감 비허용)
    }
    fmt.Println("Example 1 : ", sum)
}
```

### GO 문법 특징 2
- 문장 끝 세미콜론 ; 주의
- 자동으로 Go 컴파일러가 끝에 세미콜론을 삽입
- 두 문장을 한 문장으로 표현할 경우 명시적으로 세미콜론을 사용 - (권장하지 않는다.)
- 반복문 및 제어문(조건문) if, for를 사용할 경우 주의 해야 한다.

```go
func main()  {
    // Example 1
    for i := 0; i <= 10; i++ {
        // fmt.Println("Example 1 : ", i);fmt.Println("i")
        fmt.Print("Example 1 : ")
        fmt.Println(i)
    }

    // Example 2
    for j := 10; j >= 0; j-- {
        fmt.Println("Example 2 : ", j)
    }
}
```

### GO 문법 특징 3
- 코드 서식을 지정해주는 유틸을 내장하고 있다.
- 한 사람이 코딩한 것 같은 일관성을 유지해준다.
- 코드 스타일을 유지해준다.

```bash
# 사용법
gofmt -h 
# 원본 파일에 코드 스타일을 반영
gofmt -w
```
