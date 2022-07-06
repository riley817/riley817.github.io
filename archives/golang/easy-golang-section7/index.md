# Section 7 : Go 함수

> 인프런 쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정 강의 정리 

## 7.1 함수 기초
### 7.1.1 함수

- 여러 문장을 묶어서 실행하는 코드 블럭 단위
- `func` 키워드를 사용하여 정의한다.
- 타 언어와 달리 변환 값 `return value` 다수 가능

```go
func 함수명(매개변수) (반환타입 or 반환 값 변수명) : 반환 값 존재
func 함수명() (반환타입 or 반환 값 변수명) : 매개변수 없음, 반환 값 존재
func 함수명(매개변수) : 매개변수 존재, 반환 값 없음
func 함수명() : 매개변수 없음, 반환값 없음
```

#### 함수 사용 예제

```go
package main

import (
    "fmt"
    "strconv"
)

// 함수 선언 위치는 어느 곳이 든 가능
func helloGoLang() { // 매개변수 X, 반환값 x
    fmt.Println("Example 1 : Hello, GoLang")
}

func say_one(m string) {
    fmt.Println("Example 2 : ", m)
}

func sum(x int, y int) int {
    return x + y
}

// 함수 기초
func main() {

    // Example 1
    helloGoLang()
    
    // Example 2
    say_one("Hello World!")
    
    // Example 3
    result := sum(5, 5)
    fmt.Println("Example 3 : ", result)
    fmt.Println("Example 3 : ", sum(5, 5))
    fmt.Println("Example 3 : ", strconv.Itoa(sum(5,5)))
}
```

- Result

    >Example 1 : Hello, GoLang\
    >Example 2 :  Hello World!\
    >Example 3 :  10\
    >Example 3 :  10\
    >Example 3 :  10

### 7.1.2 매개변수 전달
- Go에서 파라미터를 전달하는 방식
    - 함수(콜백)전달
    - 값 전달`call by value`
    - 참조 전달`call by reference` 로 나뉜다.

```go
package main

import "fmt"

// 함수(콜백)전달
func sum(i int, f func(int, int)) {
    f(i, 10)
}

func add(a, b int) {
    fmt.Println("Example 1 : ", a + b)
}

// 값 전달call by value 
func multi_value(i int) {
    i = i * 10
}

// 참조 전달call by reference
func multi_reference(i *int) {
    *i *= 10 // *i = *i * 10
}

func main() {

    sum(100, add) // 매개변수로 함수를 전달
    
    // Example 2
    a := 100
    multi_value(a)
    fmt.Println("Example 2 : ", a)
    
    // Example 3
    b := 100
    multi_reference(&b)
    fmt.Println("Example 3 :", b)
}
```

- Result
    >Example 1 :  110\
    >Example 2 :  100\
    >Example 3 : 1000

### 7.1.3 함수의 리턴 값
- Go에서는 리턴값은 복수 개도 지원한다.

#### Multi Return Value 예제

```go
package main

import "fmt"

func multiply(x, y int) (int, int) {
    return x * 10, y * 10
}

func arrayMultiply(a, b, c, d, e int) (int, int, int, int, int) {
    return a * 1, b * 2, c * 3, d * 4, e * 5
}

func main() {
    // 다중 값 반환
    a, b := multiply(10, 5)
    // c := multiply(10 , 5)
    c, _ := multiply(10, 5)
    _, d := multiply(10, 5)
    
    fmt.Println("Example 1 :", a, b)
    fmt.Println("Example 1 :", c)
    fmt.Println("Example 1 :", d)
    
    // Example 2
    x1, x2, x3, x4, x5 := arrayMultiply(1, 2, 3, 4, 5)
    y1, _, y3, _, y5 := arrayMultiply(1, 2, 3, 4, 5)
    
    fmt.Println("Example 2 : ", x1, x2, x3, x4, x5)
    fmt.Println("Example 2 : ", y1, y3, y5)
}
```
- Go에서는 리턴되는 값들을 (함수에 정의된) 리턴 파라미터들에 할당할 수 있다.

#### Named Return Parameter 예제

```go
package main

import "fmt"

func multiply(x, y int) (r1, r2 int) {
    r1 = x * 10
    r2 = y * 20
    return r1, r2
}

func multiply2(x, y int) (int, int) {
    return x * 10, y * 20
}

func main() {
    // 리턴 값 변수 사용
    a, b := multiply(10, 5)
    fmt.Println("Example 1 : ", a, b)
    
    // Example 2
    c, d := multiply2(10, 5)
    fmt.Println("Example 2 : ", c, d)
}
```

### 7.1.4 가변 인자 함수 Variadic Function
- 함수에 고정된 수의 파라미터를 전달하지 않고 다양한 숫자의 파라미터를 전달하고자 할때 사용
- **...** 예약어를 사용한다.

```go
package main

import "fmt"

func multiply(n ...int) int {
    tot := 1
    for _, value := range n {
        tot *= value
    }
    return tot
}

func sum(n ...int) int {
    tot := 0
    for _, value := range n {
        tot += value
    }
    return tot
}

func prtWord(msg ...string) {
    for _, value := range msg {
        fmt.Println("Print Word -> ", value)
    }
}

func main() {
    
    // 함수 고급
    // 가변 인자 실습 : 매개 변수 개수가 동적으로 변할 때 - 정해져 있지 않음.
    
    // Example 1
    x := multiply(5, 6, 7, 8, 9, 10)
    y := sum(1,2, 3, 4, 5, 6, 7, 8, 9, 10, 11, 12, 13)
    fmt.Println("x -> ", x)
    fmt.Println("y -> ", y)
    fmt.Println()
    
    // Example 2
    prtWord("A", "F", "test", "golang", "seoul")
    
    // Example 3
    a := []int{5, 6, 7, 8, 9, 10}
    
    m := multiply(a...)
    n := sum(a...)
    
    fmt.Println()
    fmt.Println("Slice Parameter -> ", m)
    fmt.Println("Slice Parameter ->", n)
}
```

- Result

    >x ->  151200\
    >y ->  91\
    >\
    >Print Word -> A\
    >Print Word -> F\
    >Print Word -> test\
    >Print Word -> golang\
    >Print Word -> seoul\
    >\
    >Slice Parameter -> 151200\
    >Slice Parameter -> 45

#### 함수를 변수에 할당

```go
package main

import "fmt"

func multiply(x, y int) (r int) {
	r = x * y
	return r
}

func sum(x, y int) (r int) {
	r = x + y
	return r
}

func main() {

    // Example 1 - 슬라이스에 할당
    f := []func(int, int) int {multiply, sum}
    
    a := f[0](10, 10)
    b := f[1](10, 10)
    
    fmt.Println("슬라이스에 함수 할당 ->", a, f[0](10,10))
    fmt.Println("슬라이스에 함수 할당 ->", b, f[1](10,10))
    fmt.Println()
    
    // Example 2 - 변수에 할당
    var f1 func(int, int) int = multiply
    f2 := sum
    
    fmt.Println("변수에 함수 할당 ->", f1(10, 10))
    fmt.Println("변수에 함수 할당 ->", f2(10, 10))
    fmt.Println()
    
    // Example 3 - 맵에 할당
    m := map[string] func(int, int) int{
        "mul_func" : multiply,
        "sum_func" : sum,
    }
    
    fmt.Println("Map에 함수 할당", m["mul_func"](10, 10))
    fmt.Println("Map에 함수 할당", m["sum_func"](10, 10))
}
```

- Result
    >슬라이스에 함수 할당 -> 100 100\
    >슬라이스에 함수 할당 -> 20 20\
    >\
    >변수에 함수 할당 -> 100\
    >변수에 함수 할당 -> 20\
    >\
    >Map에 함수 할당 100\
    >Map에 함수 할당 20

### 7.1.5 재귀 함수 Recursion
- 장점 : 프로그램이 보기 쉽고, 코드가 간결, 오류 수정이 용이하다.
- 단점 : 코드를 이해하기 어렵고, 기억공간을 많이 사용, 무한루프 가능성이 있다.

#### 팩토리얼 재귀함수 예제

```go
package main

import "fmt"

func fact(n int) int {
    if n == 0 {
        return 1
    }
    return n * fact(n - 1)
}

func prtHello(n int) {
    if n == 0 {
        return
    }
    fmt.Println("Example 2 : (", n, ")", " hi!")
    prtHello(n - 1)
}

func main() {
    x := fact(7)
    fmt.Println("Example 1 : ", x)
    
    // Example 2
    prtHello(10)
}
```

- Result

    >Example 1 :  5040\
    >Example 2 : ( 10 )  hi!\
    >Example 2 : ( 9 )  hi!\
    >Example 2 : ( 8 )  hi!\
    >Example 2 : ( 7 )  hi!\
    >Example 2 : ( 6 )  hi!\
    >Example 2 : ( 5 )  hi!\
    >Example 2 : ( 4 )  hi!\
    >Example 2 : ( 3 )  hi!\
    >Example 2 : ( 2 )  hi!\
    >Example 2 : ( 1 )  hi!

### 7.1.6 익명함수 Anonymous Functions
- 선언과 동시에 즉시 실행

```go
package main

import "fmt"

func main() {
    // Example 1
    func() {
        fmt.Println("Example 1 : Anonymous Function !")
    }()
    
    // Example 2
    msg := "Hello GoLang!"
    
    func(m string) {
        fmt.Println("Example 2 : ", m)
    }(msg)
    
    // Example 3
    func(x, y int) {
        fmt.Println("Example 3 :", x + y)
    }(10, 20)
    
    // Example 4
    r := func(x, y int) int {
        return x * y
    }(10, 100)
    
    fmt.Println("Example 4 : ", r)
}
```

- Result

    >Example 1 : Anonymous Function !\
    >Example 2 :  Hello GoLang!\
    >Example 3 : 30\
    >Example 4 :  1000

## 7.2 defer
### 7.2.1 지연실행 defer
- **defer** 키워드는 특정 문장 혹은 함수를 나중에 (**defer를 호출하는 함수가 리턴하기 직전**) 실행한다.
- 일반적으로 `C#`, `java` 같은 언어에서의 `finally` 블럭처럼 마지막에 `clean-up` 작업을 위해 사용된다.
- 리소스 반환, 열린 파일 닫기, Mutex 잠금 해제 등 사용

```go
package main

import "fmt"

func ex_f2() {
    fmt.Println("f2 : called!")
}

func ex_f1() {
    fmt.Println("f1 : start!")
    defer ex_f2() // 마지막에 호출 된다.
    fmt.Println("f1 : end!")
}

func main() {
    ex_f1()
}
```

- Result
    >f1 : start!\
    >f1 : end!\
    >f2 : called!

#### 익명함수 defer 예제
```go
package main

import "fmt"

func sayHello(msg string) {
    defer func() {
        fmt.Println(msg)
    }()
    
    func() {
        fmt.Println("Hi !")
    }()
}

func main() {
    // Example 1
    sayHello("GoLang")
}
```

- Result
    > Hi !\
    > GoLang

### 7.2.2 스택
- `defer` 키워드를 사용하면 함수가 역순으로 실행되는 것을 볼 수 있다.
- 자료구조의 `stack` 과 동일하며, 제일 나중에 지연 호출한 함수가 제일 먼저 힐행된다. `후입선출`

```go
package main

import "fmt"

func stack() {
    for i := 1; i <= 10; i++ {
        defer fmt.Println("Example 1 : ", i)
    }
}

func main() {
    // Example 1.
    stack()
}
```

- Result

    >Example 1 :  10\
    >Example 1 :  9\
    >Example 1 :  8\
    >Example 1 :  7\
    >Example 1 :  6\
    >Example 1 :  5\
    >Example 1 :  4\
    >Example 1 :  3\
    >Example 1 :  2\
    >Example 1 :  1

#### 중첩 함수에 defer 키워드를 사용할 경우 주의 사항

```go
package main

import "fmt"

func start(t string) string {
    fmt.Println("start : ", t)
    return t
}
func end(t string) {
    fmt.Println("end : ", t)
}

func a() {
    defer end(start("b")) // defer 문에 있는 end()만 적용됨. 중첩 함수 주의 ! -> 웬만하면 사용하지 말것.
    fmt.Println("in a")
}

func main() {
    // Example 1
    a()
}
```

- Result
    >start :  b\
    >in a\
    >end :  b

## 7.3 Closure
### 7.3 Closure란
- 익명함수는 함수를 변수에 할당할 수 있다.
- 함수 안에서 함수를 선언 및 정의 가능하며 이 때 외부 함수에 선언된 변수에 접근 가능하다.
- 함수가 선언되는 순간에 함수가 실행 될 때 실체의 외부 변수에 접근하기 위한 스냅샷(객체)이다.
- 함수를 호출 할 때 이전에 존재했던 값을 유지하기 위해 사용한다
    - 비동기, 누적카운트, 무분별한 전역변수 남발을 피하기 위해 사용
    - 전역변수 남발로 인해 메모리 부족, 오버플로우 현상, 리소스 남용을 방지
- 클로저를 정확하게 이해하고 사용하는 것이 중요

```go
package main

import "fmt"

func main() {

    // Example 1
    multiply := func(x, y int) int { // 익명함
        return x * y
    }
    
    r1 := multiply(5, 10)
    fmt.Println("익명 함수 변수에 할당 ->", r1)
    
    // Example 2
    m, n := 5, 10 // 지역변수 - 변수가 캡쳐
    sum := func(c int) int { // 익명함수 변수 할당
        return m + n + c // 지역 변수 소멸되지 않는다. (함수 호출 시 마다 사용 가능)
    }
    
    r2 := sum(10)
    fmt.Println("Closure 예제 ->", r2)

}
```

- Result
    >익명 함수 변수에 할당 -> 50\
    >Closure 예제 -> 25

#### 카운팅 예제

```go
package main

import "fmt"

func main() {
    // Example 1
    cnt := increaseCnt()
    
    fmt.Println("Count ->", cnt())
    fmt.Println("Count ->", cnt())
    fmt.Println("Count ->", cnt())
    fmt.Println("Count ->", cnt())
    fmt.Println("Count ->", cnt())
    
    anotherCnt := increaseCnt()
    
    fmt.Println("함수를 새로운 변수에 할당 시 초기화")
    fmt.Println("Another Count ->", anotherCnt())
    fmt.Println("Another Count ->", anotherCnt())
    fmt.Println("Another Count ->", anotherCnt())
}

func increaseCnt() func() int {
    n := 0 	// 지역변수(캡쳐됨)
    return func() int {
        n += 1
        return n
    }
}
```

- Result

    >Count -> 1\
    >Count -> 2\
    >Count -> 3\
    >Count -> 4\
    >Count -> 5\
    >함수를 새로운 변수에 할당 시 초기화\
    >Another Count -> 1\
    >Another Count -> 2\
    >Another Count -> 3
