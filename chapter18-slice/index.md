# [Tucker의 Go 프로그래밍] 18. 슬라이스


> Tucker의 Go 언어 프로그래밍 책 내용을 정리하였습니다.

슬라이스는 Go에서 제공하는 동적 배열 타입이다. 동적 배열은 실행 도중 배열의 사이즈가 계속 바뀔 수 있다. 

## 정적, 동적
상수와 변수의 차이를 생각하면 간단하다.
- 정적(static) : compile time, build time 시 값이 결정된다. 실행 도중에 절대 바뀔 수 없다.
- 동적(dynamic) : Runtime. 프로그램 실행 도중에 계속 값이 바뀔 수 있다.

**다른언어에서 slice와 비슷한 개념 (동일하지는 않다.)**
- **C++** : `Vector<int>`
- **Java** : `ArrayList`
- **Python** : `slice`
- **Javascript** : 기본적으로 동적 배열

## 1. 슬라이스
배열과 비슷하지만 `[]`에 배열의 개수를 적지 않고 선언한다.


### 1.1 슬라이스 선언
```go
var slice []int
```

#### `{ }` 를 이용해 초기화하기

```go
var slice1 = []int{1, 2, 3} // 대괄호 안에 길이를 넣지 않는 것을 주의
var slice2 = []int{1, 5:2, 10:3} // [1 0 0 0 0 2 0 0 0 0 3]

var array = [...]int{1, 2, 3} // 배열 선언
var slice = []int{1, 2, 3} // 슬라이스 선언
```

#### make()를 이용한 초기화
내장함수인 `make()`를 사용하여 초기화 할 수 있다.

```go
var slice = make([]int, 3) // 3개의 길이를 갖는 int 슬라이스

// 결과
[0 0 0]
```

### 1.2 슬라이스 요소 접근
슬라이스 요소에 접근하는 방법은 배열과 동일하다. 대괄호 사이에 인덱스를 써서 요소에 접근한다.

```go
var slice = make([]int, 3)
slice[1] = 5
```

### 1.3 슬라이스 순회 방법
```go
var slice = []int{1, 2, 3}

// for loop
for i := 0; i < len(slice); i++ {}

// for range
for i, v := range slice {}
```

### 1.4 슬라이스 요소 추가 - `append()`
`append()`는 첫 번째 인수로 들어온 슬라이스에 **요소가 추가된 새로운 슬라이스를 반환**한다.

```go
var slice = []int{1, 2, 3}
slice := append(slice, 4)
```
하나 이상 값도 추가가 가능하다.
```go
slice = append(slice, 3, 4, 5, 6, 7)
```

## 2. 슬라이스의 동작 원리
슬라이스는 내장 타입으로 내부 구현이 감추어져있지만 `reflect` 패키지의 `SliceHeader 구조체`를 사용해 내부 구현을 볼 수 있다.

```go
type SliceHeader struct {
	Data uintptr	// 실제 배열을 가리키는 포인터
	Len  int		// 요소 개수
	Cap  int		// 실제 배열의 길이0
}
```

{{< figure src="/categories/images/tucker-go/20220426122621.png#center" width="500" caption="[그림]  SliceHeader 구조체" >}}

### 2.1 Make() 함수를 사용해서 선언
make() 함수로 슬라이스를 생성할 때 인수값에 따라 배열길이와 배열의 최대 크기를 지정할 수 있다.

```go
var slice = make([]int 3) // len:3, cap:3 (요소개수 3, 최대길이 3)
var slice2 = make([]int, 3, 5) // len:3, cap:5 (요소개수 3, 최대길이 5)
```

### 2.2 슬라이스와 배열의 동작 차이
`SliceHeader` 구조체에서 `Data`는 실제 배열을 참조하고 있다. 그렇기 때문에 배열과 동작에서 차이가 있으며 사용법이 비슷하다고 해서 똑같이 사용했을 경우 예기치 못한 버그를 만날 수 있다.

```go
package main

import "fmt"

func changeArray(arr [5]int) {
	arr[2] = 200 // 서로 다른 메모리 공간
}

func changeSlice(slice []int) {
	slice[2] = 200
}

func main() {
	arr := [5]int{1, 2, 3, 4, 5}
	slice := []int{1, 2, 3, 4, 5}

	changeArray(arr)
	changeSlice(slice)

	fmt.Println("배열 : ", arr)
	fmt.Println("슬라이스 : ", slice)
}
```
> 결과 \
배열 :  [1 2 3 4 5] \
슬라이스 :  [1 2 200 4 5]

### 2.3 동작 차이의 원인
- Go 언어에서 대입연산은 값을 복사하는 것이다. 포인터는 포인터의 값이 메모리값 주소가 복사, 구조체는 구조체의 모든 필드 값이 복사된다.
- 함수의 매개변수는 값 복사가 발생한다.

#### `changeArray(arr [5]int)`
- 함수의 매개변수 `arr`은 배열이므로 int 5개짜리 배열의 값이 복사되어 `main()` 함수의 `arr`와는 별개의 인스턴스가 된다. (총 8byte * 5= 40 byte)

#### `changeSlice(slice []int)` 
- 함수의 매개변수 `slice []int`는 실제 배열을 가리키는 포인터, len, cap 세 개의 필드로 구성된 구조체이다. 구조체의 값이 그대로 복사되므로 실제 배열을 가리키는 포인터의 메모리 주소도 함께 복사되어 사용된다. (8byte * 3 = 24 byte)

### 2.4 append()를 사용할 때 발생할 수 있는 예기치 못한 문제 - 1

**빈 공간이 충분할 때**

`append()` 함수가 호출되면 먼저 슬라이스에 값을 추가할 수 있는 빈공간이 있는지 확인한다. 남은공간은 실제 배열 길이 cap에서 슬라이스 요소개수 len을 뺀 값을 의미한다.
> **남은공간 = cap - len**

```go
package main

import "fmt"

func main() {
	slice1 := make([]int, 3, 5)  // 1) 
	slice2 := append(slice1, 4, 5) // 2)

	fmt.Println("slice1 : ", slice1, len(slice1), cap(slice1))
	fmt.Println("slice2 : ", slice2, len(slice2), cap(slice2))

	slice1[1] = 100 // 3)

	fmt.Println("After change second element")
	fmt.Println("slice1 : ", slice1, len(slice1), cap(slice1))
	fmt.Println("slice2 : ", slice2, len(slice2), cap(slice2))

	slice1 = append(slice1, 500) // 4)

	fmt.Println("After change second element")
	fmt.Println("slice1 : ", slice1, len(slice1), cap(slice1))
	fmt.Println("slice2 : ", slice2, len(slice2), cap(slice2))

}
```
> 결과 \
slice1 :  [0 0 0] 3 5 \
slice2 :  [0 0 0 4 5] 5 5 \
After change second element \
slice1 :  [0 100 0] 3 5 \
slice2 :  [0 100 0 4 5] 5 5 \
After change second element \
slice1 :  [0 100 0 500] 4 5 \
slice2 :  [0 100 0 500 5] 5 5

- `1)` 요소개수 3, 최대길이 5의 슬라이스 구조체가 생성된다. 이때 남은 길이는 5-3 = 2개이다.
- `2)` append() 함수가 호출되면 slice1의 배열에 빈공간이 있는지 확인한다. 두 개의 빈공간이 있으므로 4, 5를 추가할 수 있다.  
	- `append()는` 빈 공간에 값을 추가하고 요소 값이 2개 증가 된 슬라이스 구조체 (slice2)를 반환한다. 
	- 같은 구조체 값이 복사되었으므로 slice1과 slice2가 참조하는 배열의 주소값은 같다.
- `3)` 같은 배열 주소를 참조하므로 변경된 값이 동일하게 적용되었다.
- `4)` append() 함수를 통해 slice1에 빈공간이 있는지 확인하고 slice1[len] 자리에 500을 쓰고 len 값을 1 증가 시킨다.

### 2.5 append()를 사용할 때 발생할 수 있는 예기치 못한 문제 - 2

**빈 공간이 충분하지 않을 때**

1. append() 함수 실행시 **빈 공간이 충분하지 않으면 더 큰 새 배열을 생성** 한다. (일반적으로 두배 크기로 마련한다.)
2. 그 다음 기존의 배열의 모든 요소를 새로운 배열에 복사한다.
3. cap은 새로운 배열의 길, len은 기존 길이에 추구한 요소 개수만큼 더한 값이 된다. 포인터는 새로 생성한 배열을 가리키는 슬라이스 구조를 갖는다.

```go
package main

import "fmt"

func main() {
	slice1 := []int{1, 2, 3} // 1)
	slice2 := append(slice1, 4, 5) // 2)

	fmt.Println("slice1 : ", slice1, len(slice1), cap(slice1))
	fmt.Println("slice2 : ", slice2, len(slice2), cap(slice2))

	slice1[1] = 100 // 3)

	fmt.Println("After change second element")
	fmt.Println("slice1 : ", slice1, len(slice1), cap(slice1))
	fmt.Println("slice2 : ", slice2, len(slice2), cap(slice2))

	slice1 = append(slice1, 500) // 4)

	fmt.Println("After change second element")
	fmt.Println("slice1 : ", slice1, len(slice1), cap(slice1))
	fmt.Println("slice2 : ", slice2, len(slice2), cap(slice2))

}
```
> 결과 \
slice1 :  [1 2 3] 3 3 \
slice2 :  [1 2 3 4 5] 5 6 \
After change second element \
slice1 :  [1 100 3] 3 3 \
slice2 :  [1 2 3 4 5] 5 6 \
After change second element \
slice1 :  [1 100 3 500] 4 6 \
slice2 :  [1 2 3 4 5] 5 6 

- `1)` 1,2,3 len:3 cap:3인 슬라이스가 생성
- `2)` append() 함수가 호출 되었을 때 4,5를 추가할 빈공간이 없으므로 새료운 배열을 생성하고 4,5를 추가 후 len:5, cap:6을 갖는 새로운 슬라이스를 반환한다.
- `3)`, `4)` slice2의 경우 capacity가 늘어난 새로운 배열을 참조하므로 slice1만 변경된다.

#### 정리
- append()는 슬라이스가 가리키고 있는 배열의 capacity가 충분하면 값을 추가하고 그 배열을 참조값을 갖는 슬라이스 구조체를 리턴한다.
- capacity가 충분하지 않으면 더 큰 배열을 생성하고 생성된 새로운 배열을 참조하는 슬라이스 구조체를 리턴한다.

## 3. 슬라이싱 `slicing`
- 슬라이싱은 배열의 일부를 집어내는 기능이다. 
- 슬라이싱하면 그 결과로 배열 일부를 가리키는 슬라이스를 반환한다. 새로운 배열이 만들어지는게 아니라 **배열의 일부를 포인터로 가리키는 슬라이스를 반환**하게 된다.

```go
array[startIndex:endIndex]
```

{{< figure src="/categories/images/tucker-go/IMG_729788D09D55-1.jpeg#center" width="500" caption="" >}}

### 3.1 슬라이싱으로 배열 일부를 가르키는 슬라이스 만들기
```go
package main

import "fmt"

func main() {
	arr := [5]int{1, 2, 3, 4, 5}

	slice := arr[1:2] // 1)

	fmt.Println("array : ", arr)
	fmt.Println("slice : ", slice, len(slice), cap(slice))

	arr[1] = 100 // 2)
	fmt.Println("After change second element")
	fmt.Println("array : ", arr)
	fmt.Println("slice : ", slice, len(slice), cap(slice))

	slice = append(slice, 500) // 3)
	fmt.Println("After append 500")
	fmt.Println("array : ", arr)
	fmt.Println("slice : ", slice, len(slice), cap(slice))
}
```

> 결과 \
array :  [1 2 3 4 5] \
slice :  [2] 1 4 \
After change second element \
array :  [1 100 3 4 5] \
slice :  [100] 1 4 \
After append 500 \
array :  [1 100 500 4 5] \
slice :  [100 500] 2 4 

- `1)` array 배열의 시작 인덱스 1 끝 인덱스 이전 인덱스 범위의만 집어낸다. (len:1, capacity:4)
- `2)` slice가 array의 두번째 값을 가리키기 때문에 array의 값이 바뀌면 slice값도 바뀐다.
- `3)` slice의 capacity가 4이므로 새로운 배열을 만들지 않고 array의 인덱스 2의 값을 변경하게 된다.

### 3.2 슬라이스를 슬라이싱 하기
슬라이싱 기능은 배열 뿐만 아니라 슬라이스 일부를 집어낼 때도 사용할 수 있다.
```go
func main() {
	arr := [100]int{1: 1, 2: 2, 99: 100}
	slice1 := arr[1:10]
	slice2 := slice1[2:99]

	fmt.Println(slice1)
	fmt.Println(slice2)
}
```

> 결과 \
[1 2 0 0 0 0 0 0 0] \
[0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 0 100] 

#### 처음부터 슬라이싱
```go
slice1 := []int{1, 2, 3, 4, 5}
slice2 := slice[0:3]

// 생략 가능
slice2 := slice[:3]
```

#### 끝까지 슬라이싱
```go
slice1 := []int{1, 2, 3, 4, 5}
slice2 := slice1[2:len(slice1)] // [3, 4, 5]

// 생략 가능
slice2 := slice1[2:] // [3, 4, 5]
```

#### 전체 슬라이싱
```go
array := [5]int{1, 2, 3, 4, 5}
slice := array[:]
```

#### 인덱스 3개로 슬라이싱하여 cap 크기를 조절
- 인덱스를 2개만 사용할 경우 cap은 `배열의 전체 길이 - 시작인덱스`를 뺀 값이 된다.
- 인덱스 3개를 사용하면 cap 값을 조절할 수 있다.

> slice[시작인덱스 : 끝인덱스 : 최대인덱스]

```go
slice1 := []int{1, 2, 3, 4, 5}
slice2 := slice1[1:3:4]
```

## 4. 유용한 슬라이싱 기능 활용

### 4.1 슬라이스 복제
- 슬라이스를 그대로 복사해서 새로운 슬라이스를 만들면 두 슬라이스가 서로 영향을 주지 않는다.
- 슬라이스를 복제하는 방법은 여러가지 이며 성능상 차이는 없기 때문에 편한 방법을 이용한다.

#### 반복문을 이용한 값 복제 
```go
func main() {
	slice1 := []int{1, 2, 3, 4, 5}
	slice2 := make([]int, len(slice1))

	for i, v := range slice1 {
		slice2[i] = v
	}

	slice2[1] = 100

	fmt.Println("slice1 : ", slice1)
	fmt.Println("slice2 : ", slice2)
}
```

#### append() 함수를 이용한 값 복제
```go
slice1 := []int{1, 2, 3, 4, 5}
slice2 := make([]int{}, slice1...)
```

#### copy() 함수를 이용한 값 복제
```go
slice1 := []int{1, 2, 3, 4, 5}
slice2 := make([]int, len(slice1))

copy(slice1, slice2)
```

### 4.2 요소 삭제하기
슬라이스 중간의 요소를 삭제하는 방법이다. 

1. 중간에 위치한 삭제 요소 이후의 값을 앞당겨서 삭제된 요소를 채운다.
2. 마지막 값을 지워준다.

```go
package main

import "fmt"

func main() {

	slice1 := []int{1, 2, 3, 4, 5, 6}
	idx := 2

	for i := idx + 1; i < len(slice1); i++ {
		slice1[i-1] = slice1[i]
	}
	slice1 = slice1[:len(slice1)-1]
	fmt.Println(slice1)
}
```

#### append() 함수 이용
- 처음 요소 부터 삭제할 요소의 이전 요소 슬라이스와 삭제할 요소 이후 부터 끝까지 슬라이싱 된 슬라이스를 append()한다.

```go
slice1 = append(slice1[:idx], slice1[idx+1:]...)
```

### 4.3 요소 추가
슬라이스 중간에 요소를 추가한다. 

```go
package main

import "fmt"

func main() {
	slice := []int{1, 2, 3, 4, 5, 6}
	idx := 2

	slice = append(slice, 0) // 1. 맨 뒤에 요소 추가

	// 2. 맨 뒤의 인덱스부터 삽입하려는 위치까지 한 칸씩 뒤로 밀어준다.
	for i := len(slice) - 2; i >= idx; i-- {
		slice[i+1] = slice[i]
	}

	// 3. 값 변경
	slice[idx] = 100

	fmt.Println(slice)
}
```

1. 슬라이스 맨 뒤에 요소를 추가한다.
2. 맨 뒤값부터 삽입하려는 위치까지 한 칸씩 뒤로 밀어준다.
3. 삽입하는 위치의 값을 바꿔준다.

#### append() 함수로 코드 개선
```go
slice = append(slice[:idx], append([]int{100}, slice[idx:]...)...)
```

#### 불필요한 메모리 사용이 없도록 코드 개선
- 위에서 append() 구문으로 생성한 [100, 3, 4, 5, 6] 슬라이스는 임시 버퍼 슬라이스 이다.
- 연산을 위해 임시로 생성되었으며 나중에 사라지기는 하나 불필요한 메모리 자원이 사용되었다.

```go
slice = append(slice, 0) // 1. 맨 뒤에 요소 추가
copy(slice[idx+1:], slice[idx:]) // 2. 하나씩 뒤로 복사
```

## 5 슬라이스 정렬

### 5.1 int 슬라이스 정렬
- `sort` 패키지의 `Ints()` 함수를 이용하여 정렬

```go
package main

import (
	"fmt"
	"sort"
)

func main() {
	slice := []int{5, 2, 6, 3, 1, 4} // 1. 정렬되지 않은 슬라이스
	sort.Ints(slice) // 2. 정렬

	fmt.Println(slice)
}
```

### 5.2 구조체 슬라이스 정렬
- `Sort()` 함수를 이용하기 위해서는 `Len()`, `Less()`, `Swap()` 메서드를 구현해야 한다. 
- 이를 이용하면 우리가 정의한 구조체도 정렬을 할 수 있다.
	- [https://pkg.go.dev/sort#Sort](https://pkg.go.dev/sort#Sort)

```go
package main

import (
	"fmt"
	"sort"
)

type Student struct {
	Name string
	Age  int
}

type Students []Student // 1)

// 2) 
func (s Students) Len() int           { return len(s) }
func (s Students) Less(i, j int) bool { return s[i].Age < s[j].Age }
func (s Students) Swap(i, j int)      { s[i], s[j] = s[j], s[i] }

func main() {
	s := []Student{
		{"화랑", 31},
		{"백두산", 52},
		{"류", 42},
		{"켄", 38},
		{"송하나", 18},
	}

	sort.Sort(Students(s)) // 3)
	fmt.Println(s)
}

```

> 결과 \
[{송하나 18} {화랑 31} {켄 38} {류 42} {백두산 52}]

- `1)` []Student 별칭 타입 Students 생성
- `2)` Len(), Less(), Swap() 메서드 구현하여 `sort.Interface`를 사용할 수 있게 해준다.
- `3)` []Student를 Students 타입으로 변환한 뒤 `sort.Sort()` 함수를 호출
	- Students는 이미 sort.Interface 메서드를 포함하고 있기 때문에 sort.Sort() 인수로 사용할 수 있다.


