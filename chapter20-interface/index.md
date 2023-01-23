# [Tucker의 Go 프로그래밍] 20. 인터페이스


> Tucker의 Go 언어 프로그래밍 책 내용을 정리하였습니다.

## 1. 인터페이스 정의
### 인터페이스 선언
- 인터페이스 선언은 type을 쓴 뒤 인터페이스 명을 쓰고 interface 키워드를 쓴다. 그런 뒤 {} 블록 안에 인터페이스에 포함된 메서드 집합을 써준다.
```go
type DuckInterface interface {
	Fly()
	Walk(distance int) int
}
```
#### 유의사항
1. 메서드는 반드시 메서드명이 있어야한다.
2. 매개변수와 반환이 다르더라도 이름이 같은 메서드는 있을 수 없다.
3. 인터페이스에서는 메서드 구현을 포함하지 않는다.
#### 인터페이스 선언 예제
```go
package main

import "fmt"

type Stringer interface {
	String() string
}

type Student struct {
	Name string
	Age int
}

func (s Student) String() string {
	return fmt.Sprintf("안녕! 나는 %d살 %s라고 해.", s.Age, s.Name)
}

func main() {
	student := Student{"쳘수", 5}

	var stringer Stringer
	stringer = student

	fmt.Printf("%s\n", stringer.String())
}
```

## 2. 인터페이스를 사용하는 이유
- 인터페이스는 객체 지향 프로그래밍에서 아주 중요한 역할을 한다.
- 인터페이스를 이용하면 구체화된 객체가 아닌 인터페이스만 가지고 메서드를 호출할 수 있기 때문에 큰 코드 수정 없이 필요에 따라 구체화 된 객체로 바꿔 사용할 수 있다.

### 추상화 계층
- **추상화(abstraction)** : 내부 동작을 감춰서 서비스를 제공하는 쪽과 사용하는 쪽 모두에게 자유를 주는 방식. 
- 인터페이스는 추상화를 제공하는 **추상화 계층(abstraction layer)** 이다.
- 인터페이스를 사용하여 서로 결합하고 의존성을 줄이는 것을 **디커플링(decoupling)** 이라고 한다. 구체화된 타입으로 상호작용하는게 아니라 추상화된 관계로 상호작용함으로써 결합도를 낮추고 의존관계를 줄이므로써 이후 유지 보수를 용이하 하고 유연성을 확보할 수 있다.

## 3. 덕 타이핑
- Go 언어에서는 어떤 타입이 인터페이스를 포함하고 있는지 여부를 결정할 때 **덕 타이핑(duck typing)** 방식을 사용한다.
- 덕 타이핑 방식은 타입 선언 시 인터페이스 구현 여부를 명시적으로 나타낼 필요 없이 인터페이스에 정의한 메서드 포함 여부로만 결정하는 방식이다.

##### Stringer 인터페이스를 정의
```go
type Stringer interface {
	String() string
}
```
##### Stringer 인터페이스 포함 여부를 명시적으로 나타내지 않아도 String() 메서드를 포함하는 것 만으로 Stringer 인터페이스를 사용할 수 있다.
```go
type Student struct {
	...
}

func (s *Student) String() string {
    ...
}
```

##### 만약 Go 언어가 덕 타이핑을 지원하지 않았더라면, implements 와 같은 키워드를 써서 명시적으로 Stringer 구현 여부를 표시해야 했을 것이다.
```go
type Student struct implements Stringer {
	...
}
```

### 덕 타이핑의 장점
- 서비스 사용자 중심의 코딩을 할 수 있다.
- 덕 타이핑은 인터페이스 구현 여부를 타입 선언에서 하는게 아니라 사용될 때 해당 타입이 인터페이스에 정의된 메서드를 포함했는지 여부로 결정한다.
- 따라서 서비스 제공자가 인터페이스를 정의 할 필요 없이 구체화된 객체만 제공하고 서비스 이용자가 필요에 따라 그때그때 인터페이스를 정의해서 사용할 수 있다.

## 4. 인터페이스 기능 더 알기
* 포함된 인터페이스
* 빈 인터페이스
* 인터페이스 기본값

### 인터페이스를 포함하는 인터페이스
- 구조체에서 다른 구조체를 포함된 필드로 가질 수 있듯이 인터페이스도 다른 인터페이스를 포함할 수 있다. 이를 포함된 인터페이스라고 부른다.

```go
// 1) Read()와 Close() 메서드를 포함한 Reader 인터페이스
type Reader interface {
	Read() (n int, err error)
	Close() error
}

// 2) Write() 메서드와 Close() 메서드를 포함한 Writer 인터페이스
type Writer interface {
	Write() (n int, err error)
	Close() error
}

// 3) Reader, Writer 인터페이스의 메서드 집합을 모두 포함한 ReadWriter 인터페이스
type ReadWriter interface {
	Reader // Reader의 메서드 집합을 포함
	Writer // Writer의 메서드 집합을 포함
}
```

### 빈 인터페이스
- `interface{}`는 메서드를 가지고 있지 않은 빈 인터페이이다.
- 가지고 있어야 할 메서드가 하나도 없기 때문에 모든 타입이 빈 인터페이스로 쓰일 수 있다.
- 빈 인터페이스는 어떤 값이든 받을 수 있는 함수, 메서드, 변수값을 만들 때 사용한다.
```go
package main

import "fmt"

func PrintVal(v interface{}) {
	switch t := v.(type) {
	case int:
		fmt.Printf("v is int %d\n", int(t))
	case float64:
		fmt.Printf("v is float64 %f\n", t)
	case string:
		fmt.Printf("v is string %s\n", string(t))
	default:
		fmt.Printf("Not supported Type : %T: %v", t, t)
	}
}

type Student struct {
	Age int
}

func main() {
	PrintVal(10)
	PrintVal(3.14)
	PrintVal("Hello")
	PrintVal(Student{15})
}
```

### 인터페이스 기본값 nil
- 인터페이스 변수의 기본값은 nil이다.
```go
package main

type Attacker interface {
	Attack()
}

func main() {
	var att Attacker
	att.Attack() // Error! att의 초기값이 없기 때문에 기본값인 nil
}
```
- `invalid memory address`로 비정상적인 메모리 주소에 접근해서 프로그램 실행중에 **런타임 에러가(runtime error)** 발생한다.


