# [쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정] Section 8 : Go 객체지향


> 인프런 쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정 강의 정리 

## 8.1 인터페이스
### 8.1.1 인터페이스란
- 구조체가 필드들의 집합체라면 **interface는 메서드들의 집합**이다.
- 인터페이스는 타입이 구현해야 하는 메서드 원형 `prototype` 을 정의한다. → 추상화를 제공
- 디자인패턴 측면에서 client의 입장 → 인터페이스에 정의된 메소드의 동작을 보장하므로 정확한 class의 구현방법을 몰라도 된다.
- 클래스간의 결합도 감소 → 유지보수성 향상, 기능 추가 용이성, 독립적인 프로그래밍 가능
- 인터페이스를 구현하기 위해서는 해당 타입이 그 인터페이스 메서드들이 모두 구현되어야 한다.

#### 인터페이스 선언

```go
type 인터페이스명 interface {
  메소드1() 반환 값(타입 형)
  메소드2() // 반환 값이 없을 경우
}
```

### 8.1.2 Empty Interface
- 메서드를 전혀 갖지 않는 빈 인터페이스
- go에서 모든 Type을 나타내기 위해 빈 인터페이스를 사용한다. → 빈 인터페이스는 어떠한 타입도 담을 수 있는 컨테이너다.

```go
type test interface {} // 빈 인터페이스
...

var t test
fmt.Println("Example 1", t) // 빈(Empty) 인터페이스일 경우 Nil을 리턴한다.
```

```go
package main

import "fmt"

type Dog struct {
  name string
  weight int
}

type Cat struct {
  name string
  weight int
}

func printValue(s interface{}) {
  fmt.Println("Example 1 : ", s)
}

func main() {
  dog := Dog{"poll", 10}
  cat := Cat{"bob", 5}
  
  printValue(dog)
  printValue(cat)
  printValue(15)
  printValue("Animals")
  printValue(25.5)
  printValue([]Dog{})
  printValue([5]Dog{})

}
```

### 8.1.3 인터페이스 사용
#### 인터페이스 구현 예제

```go
package main

import "fmt"

type Dog struct {
  name string
  weight int
}

// bite 메소드를 구현
func (d Dog) bite() {
  fmt.Println(d.name, " bites!")
}

// 동물의 행동 인터페이스 선언
type Behavior interface {
  bite()
}

func main() {
  dog1 := Dog{"poll", 10}

  var interface1 Behavior
  interface1 = dog1
  interface1.bite()

  // Example 2
  dog2 := Dog{"marry", 12}
  interface2 := Behavior(dog2)

  interface2.bite()

  // Example 3
  interface3 := []Behavior{dog1, dog2}

  // 인덱스 형태로 실행
  for idx, _ := range interface3 {
      interface3[idx].bite()
  }

  // 값 형태로 실행(인터페이스)
  for _, val := range interface3 {
      val.bite()
  }
}
```

### 8.1.3 덕타이핑
- 구조체 및 변수의 값이나 타입은 상관하지 않고 오로지 구현된 메소드로만 판단하는 방식
- Go의 덕타이핑의 중요한 특징
  - 오리처럼 걷고, 소리내고, 헤엄 등 행동이 같으면 오리라고 볼 수 있다. 🦆
- 인터페이스의 정의된 메소드 사용을 유도한다.
- 코드의 가독성 및 유지보수

```go
package main

import "fmt"

type Dog struct {
  name string
  weight int
}

type Cat struct {
  name string
  weight int
}

// 구조체 Dog 메소드 구현
func (d Dog) bite() {
  fmt.Println(d.name, "Dog bites!")
}

func (d Dog) sounds() {
  fmt.Println(d.name, "Dog barks!")
}

func (d Dog) run() {
  fmt.Println(d.name, "Dog is running!")
}

// 구조체 Cat 메소드 구현
func (c Cat) bite() {
  fmt.Println(c.name, "Cat 할퀴다!")
}

func (c Cat) sounds() {
  fmt.Println(c.name, "Cat cries!")
}

func (c Cat) run() {
  fmt.Println(c.name, "Cat is running!")
}

// 동물의 행동 인터페이스 선언
type Behavior interface {
  bite()
  sounds()
  run()
}

// 인터페이스를 파라미터로 받는다.
func act(animal Behavior) {
  animal.bite()
  animal.sounds()
  animal.run()
}

func main() {
  // Example 1
  dog := Dog{"poll", 10}
  cat := Dog{"bob", 5}

  act(dog)
  act(cat)
}
```

### 8.1.4 익명 인터페이스 사용
```go
package main

import "fmt"

type Dog struct {
  name string
  weight int
}

type Cat struct {
  name string
  weight int
}

// 구조체 Dog 메소드 구현
func (d Dog) run() {
  fmt.Println(d.name, "Dog is running!")
}

// 구조체 Cat 메소드 구현
func (c Cat) run() {
  fmt.Println(c.name, "Cat is running!")
}

// 익명 인터페이스 (타입 정의 X)
func act(animal interface{run()}) {
  animal.run()
}

func main() {
  // Example 1
  dog := Dog{"poll", 10}
  cat := Dog{"bob", 5}

  act(dog)
  act(cat)
}
```

### 8.1.5 Type Assertion
- 실행(런타임) 시에 인터페이스에 할당한 변수는 실제 타입으로 변환 후 사용해야 하는 경우
- **인터페이스.(타입)**

```go
package main

import (
  "fmt"
  "reflect"
)

func main() {
  var a interface{} = 15
  b := a
  c := a.(int)
  // 실행시에 에러가 발생한다.
  // d := a.(float64) // panic: interface conversion: interface {} is int, not float64
  
  fmt.Println("Example 1 : ", a)
  fmt.Println("Example 1 : ", reflect.TypeOf(a))
  fmt.Println("Example 1 : ", b)
  fmt.Println("Example 1 : ", reflect.TypeOf(b))
  fmt.Println("Example 1 : ", c)
  fmt.Println("Example 1 : ", reflect.TypeOf(c))
  // fmt.Println("Example 1 : ", d)
  
  // Example 2 - 저장된 실제 타입 검사
  if v, ok := a.(int); ok {
      fmt.Println("Example 2 : ", v, ok)
  }
}
```

#### switch 문을 활용하여 형변환

```go
package main

import "fmt"

func checkType(arg interface{}) {
  // arg.(type) 을 통해서 현재 데이터형 반환
  switch arg.(type) {
  case bool:
      fmt.Println("This is a bool", arg)
  case int, int8, int16, int32, int64:
      fmt.Println("This is a int", arg)
  case float32, float64:
      fmt.Println("This is a int", arg)
  case string:
      fmt.Println("This is a string", arg)
  case nil:
      fmt.Println("This is a nil", arg)
  default:
      fmt.Println("What is this type?", arg)
  }
}

func main() {
  // 실제 타입 검사 switch 사용
  // 빈 인터페이스는 어떠한 자료형도 전달 받을 수 있으므로, 타입 체크를 통해 형 변환 후 사용 가능
  // Example 1
  checkType(true)
  checkType(1)
  checkType(22.542)
  checkType(nil)
  checkType("Hello Golang!")
}
```
