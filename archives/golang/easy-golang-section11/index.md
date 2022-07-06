# Section 11 : Go 파일 입출력

> 인프런 쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정 강의 정리 

## 11.1 파일 입출력
### 11.1.1 os 패키지 - 파일 읽기, 쓰기
Go에서 파일을 읽고 쓰기 위해 Go 표준 패키지인 os 패키지를 사용할 수 있다.

- `os.Open()` : 기존 파일 열기
- `os.Create()` : 새 파일을 생성
- `os.Close()` : 파일 리소스 닫기

#### 파일 읽기 및 탐색 예제

```go
package main

import (
	"fmt"
	"os"
)

// 에러 체크 방식 1
func errCheck1(e error) {
	if e != nil {
		panic(e)
	}
}

// 에러 체크 방식 2
func errCheck2(e error) {
	if e != nil {
		fmt.Println(e)
		return
	}
}

func main() {

	// 파일 열기
	file, err := os.Open("section11/sample.txt")
	errCheck1(err)

	// 읽기 예제 1
	fileInfo, err := file.Stat() // 파일 사이즈 확인 위해 정보 획득
	errCheck2(err)
	fd1 := make([]byte, fileInfo.Size()) // 슬라이스에 읽은 내용 담는다.
	ct1, err := file.Read(fd1)

	fmt.Println("파일 정보 출력 1 => ", fileInfo, "\n")
	fmt.Println("파일 이름 1 => ", fileInfo.Name(), "\n")
	fmt.Println("파일 크기 1 => ", fileInfo.Size(), "\n")
	fmt.Println("파일 수정 시간 1 =>", fileInfo.ModTime(), "\n")
	fmt.Printf("읽기 작업 완료 1 => (%d bytes) \n\n", ct1)
	fmt.Println(string(fd1))

	// 읽기 예제 2 (탐색 : Seek(offset))
	o1, err := file.Seek(20, 0) // 0 : 처음위치, 1: 현재위치 2: 마지막위치
	errCheck2(err)

	fd2 := make([]byte, 20)
	ct2, err := file.Read(fd2)

	errCheck2(err)

	fmt.Printf("읽기 작업 완료 2 => (%d bytes) (%d ret)\n\n", ct2, o1)
	fmt.Println(string(fd2))

	// 읽기 예제 3
	o2, err := file.Seek(0, 0)
	errCheck2(err)

	fd3 := make([]byte, 50)
	ct3, err := file.ReadAt(fd3, 8) // offset
	errCheck1(err)

	fmt.Printf("읽기 작업 완료 2 => (%d bytes) (%d ret)\n\n", ct3, o2)
	fmt.Println(string(fd3))

	defer file.Close()
}
```

#### 파일 쓰기 예제

```go
package main

import (
	"fmt"
	"os"
)

// 에러 체크 방식 1
func errCheck1(e error) {
	if e != nil {
		panic(e)
	}
}

// 에러 체크 방식 2
func errCheck2(e error) {
	if e != nil {
		fmt.Println(e)
		return
	}
}

// 파일 쓰기 1
func main() {

	// 파일 쓰기 예제
	file, err := os.Create("test_write.txt")
	errCheck1(err)

	// 리소스 해제
	defer file.Close()

	// 쓰기 예제
	s1 := []byte{115, 111, 109, 101, 111}
	n1, err := file.Write([]byte(s1)) // 문자열 -> byte 슬라이스 형으로 변환 후 쓰기

	errCheck2(err)
	fmt.Printf("File Write Result : (%d bytes)\n", n1)

	file.Sync() // Write Commit (Stable)!

	// 쓰기 예제 2
	s2 := "Hello GoLang! \n File Write Test - 1 \n"
	n2, err := file.WriteString(s2)
	errCheck2(err)

	fmt.Printf("File Write Result(2) : (%d bytes)\n", n2)

	file.Sync()

	// 쓰기 예제 3
	s3 := "Test WriteAt ! - 2 \n"
	n3, err := file.WriteAt([]byte(s3), 70) // len(offset) 조절하면서 테스트
	errCheck1(err)

	fmt.Printf("File Write Result(3) : (%d bytes)\n", n3)

	file.Sync()
	// 쓰기 예제 4
	n4, err := file.WriteString("Hello GoLang! \n File Write Test! - 3\n")
	errCheck2(err)
	fmt.Printf("File Write Result(4) : (%d bytes)\n", n4)

}
```

### 11.1.2 CSV 파일 읽기
- 패키지 저장소를 통해 Excel 등 다양한 파일 형식 쓰기, 읽기가 가능하다.
- `bufio` : 파일이 용량이 클 경우 버퍼 사용을 권장한다.
- 사용패키지 : [tealeg/xlsx](https://github.com/tealeg/xlsx)

#### csv 파일 읽기
```go
package main

import (
	"bufio"
	"encoding/csv"
	"fmt"
	"os"
)

func errCheck(e error) {
	if e != nil {
		panic(e)
	}
}

func main() {

	// 파일 생성
	file, err := os.Open("section11/sample.csv")
	errCheck(err)

	// 리소스 해제
	defer file.Close()

	// CSV Reader 생성
	//rr := csv.NewReader(file)
	rr := csv.NewReader(bufio.NewReader(file)) // 권장

	// CSV 내용 읽기
	row, err := rr.Read() // 1개의 Row 단위로 읽기
	errCheck(err)
	row2, err2 := rr.Read() // 1개의 Row 단위로 읽기
	errCheck(err2)

	fmt.Println("CSV Read Example")
	fmt.Println(row[0], row[1], row[2])
	fmt.Println(row2[0], row2[1], row2[2])
	fmt.Println(row[0:5])

	fmt.Println("=================================")

	rows, err := rr.ReadAll() // 전체 Row Read
	errCheck(err)

	fmt.Println("CSV Read All Example")
	fmt.Println(rows)
	fmt.Println(rows[5][1])

	// Row 단위로 CSV 파일 읽기
	for i, row := range rows {
		// fmt.Println(i, row)
		for j := range row {
			fmt.Printf("%s    ", rows[i][j])
		}
		fmt.Println()
	}

}
```

#### csv 파일 쓰기

```go
package main

import (
	"encoding/csv"
	"fmt"
	"os"
	_ "bufio"
)

// 에러 체크 방식 1
func errCheck1(e error) {
	if e != nil {
		panic(e)
	}
}

// 에러 체크 방식 2
func errCheck2(e error) {
	if e != nil {
		fmt.Println(e)
		return
	}
}
func main() {
	// 파일 생성
	file, err := os.Create("test_write.csv")
	errCheck1(err)

	// 리소스 해제
	defer file.Close()

	// CSV Writer 생성
	wr := csv.NewWriter(file)
	// wr := csv.NewWriter(bufio.NewWriter(file))

	// csv 내용 쓰기
	wr.Write([]string{"KIM", "4.8"})
	wr.Write([]string{"LEE", "4.2"})
	wr.Write([]string{"PARK", "4.1"})
	wr.Write([]string{"CHO", "4.0"})
	wr.Write([]string{"HONG", "4.2"})

	wr.Flush() // 버퍼 -> 파일로 쓰기

	fi, err := file.Stat()
	errCheck1(err)

	fmt.Printf("CSV 쓰기 작업 후 파일 크기 (%d bytes)\n", fi.Size())
	fmt.Println("CSV 파일명 : ", fi.Name())
	fmt.Println("운영 체제 파일 권한 : ", fi.Mode())

}
```

### 11.1.3 ioutil 패키지를 활용
- ioutil 패키지를 활용하면 더욱 편리하고 직관적인 파일을 읽고 쓰기가 가능.
- `WriteFile()`, `ReadFile()` , `ReadAll()` 등을 사용한다.
- [ioutil - The Go Programming Language](https://golang.org/pkg/io/ioutil/)

#### 파일 퍼미션
- 읽기(4), 쓰기(2), 실행(1)
- 소유자, 그룹, 기타사용자 순서
- [os - The Go Programming Language](https://golang.org/pkg/os/#FileMode)

#### ioutil 활용

```go
package main

import (
	"fmt"
	"io/ioutil"
	"os"
)

func errCheck(e error) {
	if e != nil {
		panic(e)
	}
}

func main() {

	s := "Hello Golang!\n File Write Test!\n"

	// 파일 쓰기
	err := ioutil.WriteFile("test_write1.txt", []byte(s), os.FileMode(0644))
	errCheck(err)

	// 파일 읽기
	data, err := ioutil.ReadFile("section11/sample.txt")
	errCheck(err)

	fmt.Println("=============================================")
	fmt.Println(string(data))
	fmt.Println("=============================================")

}
```

### 11.1.5 파일 버퍼사용
- `bufio` 패키지를 활용하여 파일 읽고 쓰기시 버퍼를 사용할 수 있다.
- ioutil, bufio 등은 `io.Reader`, `io.Writer` 인터페이스를 구현 한다.
- [bufio - The Go Programming Language](https://golang.org/pkg/bufio)

```go
type Reader interface {
	Read(p []byte) (n int, err error)
}

type Writer interface {
	Write(p []byte) (n int, err error)
}
```

#### 버퍼의 사용

```go

상태
a ------> a
b ------> ab
c ------> abc
d ------> abcd // 버퍼가 꽉찰경우 버퍼를 비우고 파일을 쓴다.
e ------> e    -------> abcd
f ------> ef    -------> abcd
g ------> efg    -------> abcd
h ------> efgh  --------> abcd
i ------> i    -------> abcdefg

```

```go
package main

import (
	"bufio"
	"fmt"
	"os"
)

func errCheck(e error) {
	if e != nil {
		panic(e)
	}
}

func main() {

	file, err := os.OpenFile("test_write2.txt", os.O_CREATE|os.O_RDWR, os.FileMode(0777))

	// bufio 파일 쓰기 예제
	wt := bufio.NewWriter(file) // Writer 반환한다. - 버퍼사용
	wt.WriteString("Hello Golang!\nFile Write Test!\n")
	wt.Write([]byte("Hello Golang!\nFile Write Test222!\n"))

	// Error Check
	errCheck(err)

	// 버퍼 정보 출력
	fmt.Printf("사용한 Buffer Size : (%d Bytes)\n", wt.Buffered())
	fmt.Printf("남은 Buffer Size : (%d Bytes)\n", wt.Available())
	fmt.Printf("전체 Buffer Size : (%d Bytes)\n", wt.Size())

	wt.Flush() // 버퍼 비우고 디스크 반영 (버퍼의 내용을 디스크에 기록한다.)
	fmt.Println("쓰기 작업 완료 \n")
	fmt.Println("=================================================")

	rt := bufio.NewReader(file) 	// Reader 반환
	fi, err := file.Stat()
	errCheck(err)

	b := make([]byte, fi.Size())

	fmt.Println("파일 정보 출력 : ", fi)
	fmt.Println("파일 이름: ", fi.Name())
	fmt.Println("파일 크기 : ", fi.Size())
	fmt.Println("파일 수정시간 : ", fi.ModTime())
	fmt.Println("=================================================")
	fmt.Println()

	file.Seek(0, os.SEEK_SET)
	data, _ := rt.Read(b) // 읽기(ReadLine, ReadByte, ReadBytes 등)
	// rt.Read(b)

	fmt.Printf("전체 Buffer Szie : (%d Bytes)\n", rt.Size())
	fmt.Printf("읽기 작업 완료 (%d Bytes) \n", data)

	fmt.Println("=================================================")
	fmt.Println(string(b))
	fmt.Println("=================================================")

	defer file.Close()
}
```
