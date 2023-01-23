# [쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정] Section 6 : 배열, 슬라이스, 맵


> 인프런 쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정 강의 정리 

## 6.1 배열

### 6.2.1 배열
- 배열은 용량, 길이가 항상 같다.
- **cap()** : 배열, 슬라이스 용량
- **len()** : 배열, 슬라이스 길이
- 대부분은 슬라이스를 많이 사용한다.

#### 배열과 슬라이스 차이점
| |배열|슬라이스|
|------|---|---|
|길이 고정여부|길이가 고정 되어 있다.|길이가 가변이다.|
|타입 여부|값 타입|참조 타입|
|전달 방식|값을 복사 전달|참조 값 전달|
|기타|전체 비교연산자 가능|전체 비교 연산자 사용 불가|

#### 배열 선언 예제 - 1
```go
var arr1 [5]int
var arr2 [5]int = [5]int{1, 2, 3, 4, 5}
var arr3 = [5]int{1, 2, 3, 4, 5}
arr4 := [5]int{1, 2, 3, 4, 5}
arr5 := [5]int{1, 2, 3}
arr6 := [...]int{1, 2, 3, 4, 5} // 배열 길이를 확신 할 수 없을 때. 잘 사용하지 않음.
arr7 := [5][5]int{
    {1, 2, 3, 4, 5},
    {6, 7, 8, 9, 10},
}

arr1[2] = 5

fmt.Printf("%-5T %d %v\n", arr1, len(arr1), arr1) // %-5T 원 자료형
fmt.Printf("%-5T %d %v\n", arr2, len(arr2), arr2)
fmt.Printf("%-5T %d %v\n", arr3, len(arr3), arr3)
fmt.Printf("%-5T %d %v\n", arr4, len(arr4), arr4)
fmt.Printf("%-5T %d %v\n", arr5, len(arr5), arr5)
fmt.Printf("%-5T %d %v\n", arr6, len(arr6), arr6)
fmt.Printf("%-5T %d %v\n", arr7, len(arr7), arr7)
```

- Result
    >[5]int 5 [0 0 5 0 0]\
    >[5]int 5 [1 2 3 4 5]\
    >[5]int 5 [1 2 3 4 5]\
    >[5]int 5 [1 2 3 4 5]\
    >[5]int 5 [1 2 3 0 0]\
    >[5]int 5 [1 2 3 4 5]\
    >[5][5]int 5 [[1 2 3 4 5] [6 7 8 9 10] [0 0 0 0 0] [0 0 0 0 0] [0 0 0 0 0]]

#### 배열 선언 예제 - 2

```go
arr8 := [5]int{1, 2, 3, 4, 5}
arr9 := [5]int{
    1,
    2,
    3,
    4,
    5,
}
arr10 := [...]string{"kim", "lee", "park"}

fmt.Printf("%-5T %d %v\n", arr8, len(arr8), arr8)
fmt.Printf("%-5T %d %v\n", arr9, len(arr9), arr9)
fmt.Printf("%-5T %d %v\n", arr10, len(arr10), arr10)
```

- Result
    >[5]int 5 [1 2 3 4 5]\
    >[5]int 5 [1 2 3 4 5]\
    >[3]string 3 [kim lee park]

### 6.2.2 배열 순회

```go
func main() {
    // 배열 순회
    arr1 := [5]int{1, 10, 100, 1000, 10000}
    
    // len 길이 반복
    for i := 0; i < len(arr1); i++ {
        fmt.Println("Example 1 : ", arr1[i])
    }
    
    arr2 := [5]int{7, 77, 777, 7777, 77777}
    // range
    for i, v := range arr2 {
        fmt.Println("Example 2 : ", i, v)
    }
    
    for _, v := range arr2 {
        fmt.Println("Example 3 : ", v)
    }
    
    for v := range arr2 {
        fmt.Println("Example 4 : ", v) // 인덱스가 출력 된다.
    }
}
```

- Result
    >Example 1 :  1\
    >Example 1 :  10\
    >Example 1 :  100\
    >Example 1 :  1000\
    >Example 1 :  10000\
    >\
    >Example 2 :  0 7\
    >Example 2 :  1 77\
    >Example 2 :  2 777\
    >Example 2 :  3 7777\
    >Example 2 :  4 77777\
    >\
    >Example 3 :  7\
    >Example 3 :  77\
    >Example 3 :  777\
    >Example 3 :  7777\
    >Example 3 :  77777\
    >\
    >Example 4 :  0\
    >Example 4 :  1\
    >Example 4 :  2\
    >Example 4 :  3

### 6.2.3 배열 복사
- **값이 복사된다.**
- 배열은 복사하여도 서로 다른 주소값을 갖게 된다.

```go
func main() {
    // 배열 복사
    
    arr1 := [5]int{1, 10, 100, 1000, 10000}
    arr2 := arr1
    
    fmt.Println("Example 1 : ", arr1, &arr1)
    fmt.Println("Example 1 : ", arr2, &arr2)
    
    fmt.Printf("Example 1 : %p %v\n", &arr1, arr1)
    fmt.Printf("Example 1 : %p %v\n", &arr2, arr2)
}
```

- Result
    >Example 1 :  [1 10 100 1000 10000] &[1 10 100 1000 10000]\
    >Example 1 :  [1 10 100 1000 10000] &[1 10 100 1000 10000]\
    >Example 1 : 0xc00010c030 [1 10 100 1000 10000]\
    >Example 1 : 0xc00010c060 [1 10 100 1000 10000]

## 6.2 슬라이스

### 6.2.1 슬라이스
- 길이가 가변이다. → 동적으로 크키가 늘어난다.
- 레퍼런스(참조 값) 타입 이다.
- 슬라이스(길이, 용량) 크기가 동적으로 할당이 가능하다.
- 부분 배열을 발췌할 수 있다.

#### 선언 방법
1. 배열처럼 선언한다.
2. **make 함수**를 사용하여 선언한다.
    - make 함수를 이용하면 슬라이스의 길이(length)와 용량(capacity)을 임의로 지정할 수 있다.

```go
make(자료형, 길이, 용량:생략시 길이)
```

#### 슬라이스 선언 예제 - 1
```go
// 배열 처럼 선언
var slice1 []int
slice2 := []int{}
slice3 := []int{1, 2, 3, 4, 5}  // 슬라이스에 리터럴 값을 지정
slice4 := [][]int{
	{1, 2, 3, 4, 5},
	{6, 7, 8, 9, 10},
}

// slice2[0] = 1	// 길이가 가변형이기 때문에 초기화를 별도로 해야한다.
slice3[4] = 10 	// 값 수정 가능
fmt.Printf("%-5T %d %d %v\n", slice1, len(slice1), cap(slice1), slice1)
fmt.Printf("%-5T %d %d %v\n", slice2, len(slice2), cap(slice2), slice2)
fmt.Printf("%-5T %d %d %v\n", slice3, len(slice3), cap(slice3), slice3)
fmt.Printf("%-5T %d %d %v\n", slice4, len(slice4), cap(slice4), slice4)
```

- Result
    >[]int 0 0 []\
    >[]int 0 0 []\
    >[]int 5 5 [1 2 3 4 10]\
    >[][]int 2 2 [[1 2 3 4 5] [6 7 8 9 10]]

#### 슬라이스 선언 예제 - 2

```go
var slice5 []int = make([]int, 5, 10)	// 길이가 5이고 용량이 10인 슬라이스. 용량이 늘어날 경우에
var slice6 = make([]int, 5, 100)
slice7 := make([]int, 5, 100)
slice8 := make([]int, 5, 100)

slice6[2] = 7 // make로 생성시 default 값으로 초기화된다.
fmt.Printf("%-5T %d %d %v\n", slice5, len(slice5), cap(slice5), slice5)
fmt.Printf("%-5T %d %d %v\n", slice6, len(slice6), cap(slice6), slice6)
fmt.Printf("%-5T %d %d %v\n", slice7, len(slice7), cap(slice7), slice7)
fmt.Printf("%-5T %d %d %v\n", slice8, len(slice8), cap(slice8), slice8)
```

- Result
    >[]int 5 10 [0 0 0 0 0]\
    >[]int 5 100 [0 0 7 0 0]\
    >[]int 5 100 [0 0 0 0 0]\
    >[]int 5 100 [0 0 0 0 0]

### 6.2.2 Nil Slice
- 슬라이스에 별도의 길이와 용량을 지정하지 않으면 길이와 용량이 0인 슬라이스를 만든다.
- **Nil Slice**라고도 하며 nil과 비교하면 참을 리턴한다.

```go
var slice9 []int // int 슬라이스(길이와 용량이 0)
if nil == slice9 {
    fmt.Println("This is Nil Slice!")
}
```

- Result
    > This is Nil Slice!

### 6.2.3 슬라이스와 배열
- 슬라이스의 경우 참조 타입이므로 값 변경시 원본 값도 같이 변경된다.

```go
// 배열
arr1 := [3]int{1, 2, 3}
var arr2 [3]int

arr2 = arr1
arr2[0] = 7

fmt.Println("Array Example 1 : ", arr1)
fmt.Println("Array Example 1 : ", arr2)

// 슬라이스
slice1 := []int{1, 2, 3}
var slice2 []int

slice2 = slice1
slice2[0] = 7

fmt.Println("Slice Example 2 : ", slice1)
fmt.Println("Slice Example 2 : ", slice2)
```

- Result
    >Array Example 1 :  [1 2 3]\
    >Array Example 1 :  [7 2 3]\
    >\
    >// 슬라이스는 참조 타입이므로 값 변경시 원본 값도 같이 변경된다.\
    >Slice Example 2 :  [7 2 3]\
    >Slice Example 2 :  [7 2 3]

### 6.2.4 슬라이스 예외
- make 함수로 슬라이스 초기화 시 길이 만큼 초기화 된다.

```go
slice3 := make([]int, 50, 100)
fmt.Println("Example 3 : ", slice3[4])
// fmt.Println("Example 3 : ", slice3[50]) // Error ! 길이 만큼 초기화 된다.
```

### 6.2.5 부분 슬라이스(Sub-slice)
- 슬라이스에서 일부를 발췌하여 부분 슬라이스를 만들 수 있다.
- **슬라이스[처음인덱스:마지막인덱스]** : 처음인덱스는 inclusive 마지막인덱스는 exclusive

```go
slice[i:j] i -> j-1 까지 추출
slice[i:]  i -> 마지막 까지 추출
slice[:j]  처음부터 -> j-1 까지 추출
slice[:]   처음부터 -> 마지막 까지 추출
```

```go
slice1 := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}

fmt.Println("Example 1 :", slice1[:])
fmt.Println("Example 1 :", slice1[0:])
fmt.Println("Example 1 :", slice1[:5])
fmt.Println("Example 1 :", slice1[0:len(slice1)])
fmt.Println("Example 1 :", slice1[3:])
fmt.Println("Example 1 :", slice1[1:3])
```

- Result
    >Example 1 : [1 2 3 4 5 6 7 8 9 10]\
    >Example 1 : [1 2 3 4 5 6 7 8 9 10]\
    >Example 1 : [1 2 3 4 5]\
    >Example 1 : [1 2 3 4 5 6 7 8 9 10]\
    >Example 1 : [4 5 6 7 8 9 10]\
    >Example 1 : [2 3]

### 6.2.6 슬라이스 추가 append

- 배열은 고정된 크기로 그 크기 이상의 임의의 데이터를 추가할 수 없지만 슬라이스는 자유롭게 추가할 수 있다.
- 용량에 따라
    - 용량(capacity)이 아직 남아 있는 경우 :  용량 내에서 슬라이스 길이(length)를 변경하여 데이터를 추가한다.
    - 용량(capacity)을 초과하는 경우 : 현재 용량의 두 배에 해당하는 새로운 Underlying array을 생성하고 기존 배열 값들을 모두 새 배열에 복제한 후 다시 새로운 슬라이스를 할당한다.
- 슬라이스 끼리 병합할때 두번째 슬라이스 뒤에는 **...(ellipsis)** 을 붙인다.
- ellipsis는 해당 슬라이스의 컬렉션을 표현하는 것으로 두번째 슬라이스의 모든 요소들의 집합을 나타낸다.

```go
s1 := []int{1, 2, 3, 4, 5}
s2 := []int{8, 9, 10, 11, 12}
s3 := []int{13, 14, 15, 16, 17}

fmt.Printf("[Example 1] s1 => %d", cap(s1))

s1 = append(s1, 6, 7)
s2 = append(s1, s2...) 		   // 슬라이스 뒤에 슬라이스 삽입할 경우 ... 사용
s3 = append(s2, s3[0:3]...)  // 추출 후 병합
fmt.Printf("[Example 1] s1 => %d", cap(s1))

fmt.Println("[Example 1] s1 =>", s1)
fmt.Println("[Example 1] s2 =>", s2)
fmt.Println("[Example 1] s3 =>", s3)
```

- Result
  >[Example 1] s1 => 5\
  >[Example 1] s1 => [1 2 3 4 5 6 7]\
  >[Example 1] s2 => [1 2 3 4 5 6 7 8 9 10 11 12]\
  >[Example 1] s3 => [1 2 3 4 5 6 7 8 9 10 11 12 13 14 15]

```go
s4 := make([]int, 0, 5)
for i := 0; i < 15; i++ {
	s4 = append(s4, i)
	fmt.Printf("Example 2 -> len : %d, cap: %d, value : %v\n", len(s4), cap(s4), s4) // 길이 및 용량 자동 증가
}
```

- Result
    >Example 2 -> len : 1, cap: 5, value : [0]\
    >Example 2 -> len : 2, cap: 5, value : [0 1]\
    >Example 2 -> len : 3, cap: 5, value : [0 1 2]\
    >Example 2 -> len : 4, cap: 5, value : [0 1 2 3]\
    >Example 2 -> len : 5, cap: 5, value : [0 1 2 3 4]\
    >Example 2 -> len : 6, cap: 10, value : [0 1 2 3 4 5]\
    >Example 2 -> len : 7, cap: 10, value : [0 1 2 3 4 5 6]\
    >Example 2 -> len : 8, cap: 10, value : [0 1 2 3 4 5 6 7]\
    >Example 2 -> len : 9, cap: 10, value : [0 1 2 3 4 5 6 7 8]\
    >Example 2 -> len : 10, cap: 10, value : [0 1 2 3 4 5 6 7 8 9]\
    >Example 2 -> len : 11, cap: 20, value : [0 1 2 3 4 5 6 7 8 9 10]\
    >Example 2 -> len : 12, cap: 20, value : [0 1 2 3 4 5 6 7 8 9 10 11]\
    >Example 2 -> len : 13, cap: 20, value : [0 1 2 3 4 5 6 7 8 9 10 11 12]\
    >Example 2 -> len : 14, cap: 20, value : [0 1 2 3 4 5 6 7 8 9 10 11 12 13]\
    >Example 2 -> len : 15, cap: 20, value : [0 1 2 3 4 5 6 7 8 9 10 11 12 13 14]
  
### 6.2.7 슬라이스 copy()

- **copy(복사 대상, 원본)**
- 반드시 초기화를 하여 공간을 할당한 후 복사 해야 한다.
- 복사 된 슬라이스 값을 변경해도 원본에는 영향이 없다.
- 부분적으로 슬라이스 추출은 참조 타입

```go
func main() {

    // Example 1
    slice1 := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    slice2 := make([]int, 5)
    slice3 := []int{}
    
    copy(slice2, slice1)	// slice2 가 용량이 5이므로 5만 복사
    copy(slice3, slice2)	// 복사안됨
    
    fmt.Println("Example 1 :", slice2)
    fmt.Println("Example 1 :", slice3)
    
    // Example 2
    a := []int{1, 2, 3, 4, 5}
    b := make([]int, 5)
    
    copy(b, a) // 가변길이로 배열처럼 사용할 수 있다.
    
    b[0] = 7
    b[4] = 10
    
    fmt.Println("Example 2 : ", a) // 원본 값이 변경되지 않는다.
    fmt.Println("Example 2 : ", b)
    
    // Example 3
    // 주의! 부분적으로 슬라이스 추출은 참조 -> 원본 값이 변경된다.
    c := [5]int{1, 2, 3, 4, 5}
    d := c[0:3]		
    
    d[1] = 7
    fmt.Println()
    fmt.Println("Example 3 :", c)
    fmt.Println("Example 3 :", d)
    
    // Example 4
    e := []int{1, 2, 3, 4, 5, 6, 7, 8, 9, 10}
    f := e[0:5:7] // 용량 지정
    
    f[0] = 99
    
    fmt.Println("Example 4 :", e)
    fmt.Println("Example 4 :", len(f), cap(f))
    fmt.Println("Example 4 :", f)

}
```

- Result
    >Example 1 : [1 2 3 4 5]\
    >Example 1 : []\
    >\
    >Example 2 :  [1 2 3 4 5]\
    >Example 2 :  [7 2 3 4 10]\
    >\
    >Example 3 : [1 7 3 4 5]\
    >Example 3 : [1 7 3]\
    >\
    >Example 4 : [99 2 3 4 5 6 7 8 9 10]\
    >Example 4 : 5 7\
    >Example 4 : [99 2 3 4 5]

### 6.2.8 슬라이스 정렬
- sort 패키지 → [https://golang.org](https://golang.org/)/pkg/sort

```go
package main

import (
	"fmt"
	"sort"
)

func main() {

	slice2 := []int{3, 6, 10, 9, 1, 4, 5, 8, 2, 7}
	slice3 := []string{"b", "d", "f", "a", "c", "e"}

	fmt.Println("Example 2 : ", sort.IntsAreSorted(slice2)) // 정렬 확인
	sort.Ints(slice2) // 정렬
	fmt.Println("Example 2 :", slice2)

	fmt.Println()

	fmt.Println("Example 2 : ", sort.StringsAreSorted(slice3))
	sort.Strings(slice3)

	fmt.Println("Example 2 : ", sort.StringsAreSorted(slice3))
}
```

- Result
    >Example 2 :  false\
    >Example 2 :  true

## 6.3 맵 Map

### 6.3.1 Map
- 키에 대응하는 값 value으로 자료를 저장하는 해시테이블을 구현한 자료구조
    - **hashtable**, **dictionary-python**
- reference 타입으로 참조값을 전달한다.
- 참조타입은 키로 사용 불가능하고 값으로 모든 타입을 사용 가능하다.
- make 함수와 축약(리터럴)로 초기화가 가능하다.

```go
var map1 map[string]int = make(map[string]int)	// 정석
var map2 = make(map[string]int)	                // 자료형 생략
map3 := make(map[string]int)                    
map4 := map[string]int{}                        // JSON 형태

// 리터럴을 사용한 초기화
map5 := map[string]int{
    "apple" : 15,
    "banana" : 40,
    "orange" : 23,
}
```

#### 맵 선언 예제
```go
package main

import "fmt"

func main() {

	// Example 1
	var map1 map[string]int = make(map[string]int)	// 정석
	var map2 = make(map[string]int)	// 자료형 생략
	map3 := make(map[string]int)

	fmt.Println("Example 1 :", map1)
	fmt.Println("Example 1 :", map2)
	fmt.Println("Example 1 :", map3)
	fmt.Println()

	// Example 2
	map4 := map[string]int{} // JSON 형태
	map4["apple"] = 25
	map4["banana"] = 40
	map4["orange"] = 33

	map5 := map[string]int{
		"apple" : 15,
		"banana" : 40,
		"orange" : 23,
	}
	map6 := make(map[string]int, 10)
	map6["apple"] = 25
	map6["banana"] = 40
	map6["orange"] = 23

	fmt.Println("Example 2 : ", map4)
	fmt.Println("Example 2 : ", map5)
	fmt.Println("Example 2 : ", map6)
	fmt.Println("Example 2 : ", map6["apple"])
	fmt.Println("Example 2 : ", map4["banana"])
}
```

- Result
    > Example 1 : map[]\
    > Example 1 : map[]\
    > Example 1 : map[]\
    >\ 
    > Example 2 :  map[apple:25 banana:40 orange:33]\
    > Example 2 :  map[apple:15 banana:40 orange:23]\
    > Example 2 :  map[apple:25 banana:40 orange:23]\
    >\ 
    > Example 2 :  25\
    > Example 2 :  40

### 6.3.2 맵 조회 및 순회 (Iterator)
- Map이 가지고 있는 모든 요소를 출력하기 위해 for-range 루프를 사용할 수 있다.

```go
func main() {

    map1 := map[string]string {
        "daum" : "https://www.daum.net",
        "naver" : "https://www.naver.com",
        "google"  : "https://www.google.com",
    }
    
    fmt.Println("Example 1 : ", map1["google"])
    fmt.Println("Example 1 : ", map1["daum"])
    fmt.Println()
    
    // Example 2
    for k, v := range map1 {
        fmt.Println("Example 2 : ", k, v)
    }
    fmt.Println()
    
    for _, v := range map1 {
        fmt.Println("Example 2 : ", v)
    }
}
```

- Result

    >Example 1 :  https://www.google.com\
    >Example 1 :  https://www.daum.net\
    >\
    >Example 2 :  google https://www.google.com\
    >Example 2 :  daum https://www.daum.net\
    >Example 2 :  naver https://www.naver.com\
    >\
    >Example 2 :  https://www.daum.net\
    >Example 2 :  https://www.naver.com\
    >Example 2 :  https://www.google.com

### 6.3.3 맵 사용
- **맵변수명[키] = 값** 과 같이 해당 키에 값을 할당 할 수 있다.
- map안에 찾는 키가 존재하지 않는다면 참조 타입의 경우 `nil` 을 값 타입인 경우 0을 리턴한다.
- **delete(삭제할 값의 키)** 로 데이터를 삭제할 수 있다.

```go
package main

import "fmt"

func main() {
    map1 := map[string]string {
        "daum" : "https://www.daum.net",
        "naver" : "https://www.naver.com",
        "google"  : "https://www.google.com",
        "home1" : "http://127.0.0.1",
    }
    
    fmt.Println("Example 1 : ", map1)
    
    map1["home2"] = "http://localhost" // 추가
    fmt.Println("Example 1 : ", map1)
    
    map1["home2"] = "http://127.0.0.1" // 수정
    fmt.Println("Example 1 :", map1)
    
    // Example 2 - delete
    delete(map1, "home2")
    fmt.Println("Example 1 :", map1)
}
```

### 6.3.4 맵 키 체크
- Go에서는 **map변수[키]** 읽기를 수행할 때 두개의 리턴 값을 리턴한다. 첫번째는 키에 상응하는 값이고 두번째는 키가 존재하는지 아닌지를 나타내는 bool 값이다.

```go
package main

import "fmt"

func main() {
    
    // Example 1
    map1 := map[string]int {
        "apple" : 25,
        "banana" : 115,
        "orange" : 1115,
        "lemon" : 0,
    }
    
    value1 := map1["lemon"]
    value2, ok2 := map1["banana"]
    value3, ok := map1["kiwi"]
    
    fmt.Println(value1)
    fmt.Println(value2, ",", ok2)
    fmt.Println(value3, ",", ok) // 두 번째 리턴 값으로 키 존재 유무 확인
    
    // Example 2
    if value, ok := map1["kiwi"]; ok {
        fmt.Println("Example 2 : ", value)
    } else {
        fmt.Println("Example 2 : kiwi is not exists")
    }
    
    // 키가 존재하는지만 
    if _, ok := map1["kiwi"]; !ok {
        fmt.Println("kiwi is not exists")
    }
}
```

## 6.4 포인터

### 6.4.1 포인터
- Go 언어는 포인터를 지원한다.
- 변수 지역성, 연속된 메모리 참조, 힙, 스택 ...
- 주소의 값은 직접 변경이 불가능하다 → 잘못된 코딩으로 인한 버그 방지
- ***(asterisk)** 로 사용한다.

```go
var a *int // pinter nil
var b *int = new(int)
```

#### 포인터 예제 - 1
```go
package main

import "fmt"

// 자료형 포인터 1
func main() {
    
    var a *int 				      // 방법1
    var b *int  = new(int) 	// 방법2
    
    fmt.Println(a)          // nil
    fmt.Println(b)          
    
    i := 7
    
    fmt.Println("Example 1 : ", i, &i)
    
    a = &i // 주소값을 전달
    b = &i // 주소값을 전달
    
    fmt.Println("Example 1 : ", a, &i) // i의 주소값, i의 주소값
    fmt.Println("Example 1 : ", &a)	// i의 주소값이 저장되어있는 a의 주소값
    fmt.Println("Example 1 : ", *a)	// 역참조
    
    fmt.Println()
    
    fmt.Println("Example 1 : ", b, &i)
    fmt.Println("Example 1 : ", &b)	// i의 주소값이 저장되어있는 b의 주소값
    fmt.Println("Example 1 : ", *b)	// 역참조
    
    var c = &i
    d := &i
    *d = 10  // 참조하고있는 값을 변경.
    
    fmt.Println()
    fmt.Println("Example 1 : ", c, &i)
    fmt.Println("Example 1 : ", &c)	// i의 주소값이 저장되어있는 b의 주소값
    fmt.Println("Example 1 : ", *c)	// 역참조
    
    fmt.Println()
    fmt.Println("Example 1 : ", d, &i)
    fmt.Println("Example 1 : ", &d)	// i의 주소값이 저장되어있는 b의 주소값
    fmt.Println("Example 1 : ", *d)	// 역참조
}
```

#### 포인터 예제 2
```go
package main

import "fmt"

func main() {
    i := 7
    p := &i
    
    fmt.Println("Example 1 :", i, *p, &i, p)
    
    *p++	// 1증가
    fmt.Println("Example 1 :", i, *p, &i, p)
    
    *p = 7777 // 포인터 변수 역 참조 값 변경
    fmt.Println("Example 1 :", i, *p, &i, p)
    
    i = 77
    fmt.Println("Example 1 :", i, *p, &i, p)
}
```
- Result
    >Example 1 : 7 7 0xc0000140e8 0xc0000140e8\
    >Example 1 : 8 8 0xc0000140e8 0xc0000140e8\
    >Example 1 : 7777 7777 0xc0000140e8 0xc0000140e8\
    >Example 1 : 77 77 0xc0000140e8 0xc0000140e8

### 6.4.2 포인터 값 전달
- 함수, 메서드 호출시에 매개변수 값을 복사 전달 → 함수, 메서드 내에서는 원본 값 변경이 불가능하다.
- 원본 값 변경을 위해 포인터로 전달
- 크기가 큰 배열의 경우 복사시 시스템에 큰 부담 → 포인터로 전달 해결(슬라이스, 맵이 참조전달이기 때문) → 배열도 포인터 전달로 참조전달 할 수 있다.

```go
package main

import "fmt"

func rptc(n *int) {
	*n = 77
}

func vptc(n int) {
	n = 77
}

func main() {
    // Example
    var a int = 10
    var b int = 10
    
    fmt.Println("Example 1 : ", a) // 10
    fmt.Println("Example 1 : ", b) // 10
    fmt.Println()
    
    rptc(&a)
    vptc(b)
    
    fmt.Println("Example 2 : ", a) // 77
    fmt.Println("Example 2 : ", b) // 10
}
```
