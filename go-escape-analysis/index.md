# [Tucker의 Go 프로그래밍] Go 스택 메모리와 힙 메모리


> 음.. 완벽하게 이해하지는 못했다 😇😇😇 

## 스택 메모리와 힙 메모리
- 대부분의 프로그래밍 언어는 메모리를 할당할 때 스택 메모리 영역 또는 힙 메모리 영역을 사용한다.
- 함수 내부에서만 사용되는 값은 스택 메모리에 할당된다.
- 함수 외부로 공개되는 메모리 공간은 힙 메모리 영역에 할당된다.
- C/C++ 언어에서는 malloc() 함수를 직접 호출해서 힙 메모리 공간을 할당한다.
- 자바의 경우 클래스 타입을 힙에, 기본 타입을 스택에 할당한다.
- Go 언어는 **이스케이프 분석(escape analysis)** 을 해서 어느 메모리에 할당할지 결정한다.

### 이스케이프 분석(escape analysis)
- GC 컴파일러는 함수와 패키지를 넘어 전역적으로 탈출 검사를 수행한다.
- 빌드 시 아래 옵션을 사용하면 이스케이프 분석 정보를 확인할 수 있다.
> **go build -gcflags="-m"**

```go
package main

import "fmt"

type User struct {
	Name 	string
	Age 	int
}

func stayOnStack() User {
	u := User{
		Name : "chunsik",
		Age : 5,
	}
	return u
}

func escapeToHeap() *User {
	u := User{
		Name : "chunsik",
		Age : 5,
	}
	// 탈출 분석으로 u 메모리가 사라지지 않음.
	return &u
}

func main() {
	fmt.Println(stayOnStack())
	fmt.Println(escapeToHeap())
}
```
- stayOnStack()의 변수 u는 객체의 사본이 리턴되었다. 컴파일 할 때 구조체 u의 크기를 알 수 있기에 컴파일러는 u를 스택에 저장한다.
- escapeToHeap()는 값을 리턴하는 것이 아니라 값의 포인터(주소)를 리턴한다. u변수의 인스턴스가 함수 외부로 공개되는 것을 분석해내에 u를 스택 메모리가 아닌 힙 메모리에  할당한다. 

## 참고
- Tucker의 Go 언어 프로그래밍
- [The Ultimate Go Study Guide](https://ultimate-go-korean.github.io/translation/#%EC%9D%B4%EC%8A%A4%EC%BC%80%EC%9D%B4%ED%94%84-%EB%B6%84%EC%84%9Descape-analysis)
