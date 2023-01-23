# [쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정] Section 2 : Go 기초 문법


> 인프런 쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정 강의 정리 

## 2.1 변수 및 상수
### 변수 선언
- GO 언어는 자료형을 정적으로 검사하므로 변수에 자료형이 정해져 있다.
- 정적 자료형을 지원하지만 자료형 선언 할당하는 번거로움을 벗어나게 해주는 **자료형 추론 기능**이 있다.
- 정수 타입 0, 실수 0.0, 문자열 "", boolean (true, false)
- 변수 명의 첫 글자는 숫자로 시작해서는 안된다.
- 대소문자는 구분하며 문자, 숫자, 밑줄, 특수기호로 변수명 선언이 가능하다.

```go
var a int
var b string
var c, d, e int
var f, g, h int = 1, 2, 3
var i float32 = 11.4
var j string = "Hi! Golang!"
var k = 4.74 // 선언과 동시 초기화
var l = "Hi Seoul!"
var m = true

a = 4
b = "Hello Go!"
e = 77
```

### 여러 변수 동시 선언
```go
// 변수 여러개를 선언
var (
    name      string = "machine"
    height    int32
    weight    float32
    isRunning bool
)

height = 250
weight = 350.56
isRunning = true
```

### 자료형 추론(짧은 선언)
- Go 언어에서는 자료형이 무엇인지 알 수 있는 경우에는 자료형을 쓰지 않아도 된다.
- 함수 내에서만 사용 가능하다 → **전역으로는 사용 불가**
- **주로 제한된 범위의 함수에서 사용할 경우 코드의 가독성을 높일 수 있다.**

```go
func main() {
	shortVar1 := 3
	shortVar2 := "Test"
	shortVar3 := false

	// shortVar :=3 true // 예외 발생
}
```

### IF 문에서 짧은 선언 사용시

```go
// Example
if i := 10; i < 11 {
	fmt.Println("Short Variable Test Success")
}
```

## 상수
- 상수는 변수와 달리 한 번 선언 후에는 값을 변경할 수 없다.
- 고정 된 값을 관리할 때 사용한다.
- **상수는 선언과 동시에 할당이 되어야 한다.**

### 상수를 선언하는 방법

```go
const a string = "Test1"
const b = "Test2"
const c int32 = 10 * 10
// const d = getHeight() // 함수 결과값을 할당하는 경우 예외 발생. 함수 사용할 수 없다.
const e = 35.6
const f = false
/*
   에러 발생이 되는 경우
   const g string
   g = "Test3" // 상수는 선언과 동시에 할당이 되어야 한다.
*/
```

### 여러 상수 동시 선언
```go
const a, b int = 10, 99
const c, d, e = true, 0.84, "test"
const (
	x, y int16 = 50, 90
	i, k       = "Data", 7776
)
```

## 열거형 - Enumeration
열거형은 상수를 사용하는 일정한 규칙에 따라 숫자를 계산 및 증가시키는 묶음이다.

```go
func main() {
	const (
		Jan = 1
		Feb = 2
		Mar = 3
		Apr = 4
		May = 5
		Jun = 6
	)

	fmt.Println(Jan, Feb, Mar, Apr, May, Jun)
	// Output
	// 1 2 3 4 5 6
}
```

### iota 키워드
- **iota**는 상수 선언에서 사용할 수 있는 예약어로 연속적인 정수 상수 0, 1, 2, ... 를 나타낸다.
- 시작값은 0이고 이후부터는 +1 증가된 값으로 선언된다.

```go
func main()  {
    const (
        A = iota * 10
        B
        C
    )
    fmt.Println(A, B, C)
    // Output:
    // 0 10 20
}
```

### 중간 값 스킵하기
- 중간 값을 스킵하려면 `_` 를 사용하여 중간 값을 스킵할 수 있다.

```go
func main() {
    const (
        _ = iota
        A
        _
        C
    )

    fmt.Println(A, C)
    // Output 
    // 1 3
    
    const (
        _ = iota + 0.75 * 2
        DEFAULT
        SILVER
        _
        PLATINUM
    )
    
    fmt.Println("D : ", DEFAULT)
    fmt.Println("S : ", SILVER)
    // fmt.Println("G : ", GOLD) 	// 사용하는 코드는 수정해야 한다.
    fmt.Println("P : ", PLATINUM)
    
    // Output
    // D :  2.5
    // S :  3.5
    // P :  5.5
}
```
