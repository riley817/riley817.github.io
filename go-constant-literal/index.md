# [Tucker의 Go 프로그래밍] Go 상수와 리터럴



> Tucker의 Go 언어 프로그래밍 책 내용을 정리하였습니다.

## 상수와 리터럴
### 상수 (Constant)
- 상수는 변하지 않는 값을 의미한다. 한번 초기화 하면 값을 바꿀 수 없다.
- 정수, 실수, 문자열 등 기본 타입값들로만 상수로 선언될 수 있다. 구조체, 배열 등 기본 타입이 아닌 타입에는 상수를 사용할 수 없다.
- 상수로 사용될 수 있는 타입
  * 불리언
  * 룬
  * 정수
  * 실수
  * 복소수
  * 문자열
- 상수는 값으로만 동작한다. 변수가 값, 이름, 타입, 메모리 주소 4가지 속성을 가지는 반면 상수는 값, 이름, 타입 3가지 속성만 가진다.
> const C int = 10\
> fmt.Println(&C) // 에러 상수는 값으로만 동작. 주소값에 접근할 수 없다.

### 리터럴 (Literal)
- 리터럴은 고정된 값, 데이터 그 자체를 의미한다.
```go
var str string = "Hello World"
var i int = 0
i = 30
```
- "Hello World", 0, 30 처럼 고정된 값 자체로 쓰인 데이터가 바로 리터럴이다.

### Go에서 상수와 리터럴
- Go 언어에서는 상수는 리터럴과 같이 취급한다. 그래서 컴파일 될 때 상수는 리터럴로 변환되어 실행 파일에 쓰인다.
- 상수 표현식 역시 컴파일 타임에 실제 결과값 리터럴로 변하기 때문에 상수 표현식 계산에 CPU 자원을 사용하지 않는다.
```go
const PI = 3.14
var a int = PI * 100
```
위의 구문은 컴파일 타임에 아래와 같이 변환된다.
```go
var a int = 314
```
- 상수의 메모리 주소값에 접근할 수 없는 이유 역시 컴파일 타임에 리터럴로 전환되어서 실행 파일에 값 형태로 쓰이기 때문이다.
- 그래서 동적 할당 메모리 영역을 사용하지 않는다.
