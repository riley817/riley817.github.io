# Section 12 : 패키지 고급

> 인프런 쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정 강의 정리 

## 12.1 사용자 패키지 제작 및 문서화
### 12.1.1 사용자 패키지 작성 및 문서화
- go 에서 패키지는 폴더명(디렉토리명)으로 접근한다. → 그러므로 명확하게 지정해야 한다.
- 사용자 패키지를 문서화 할 때 main 패키지를 제외하고 package 문서에 등록된다.
- 기본적으로 `GOROOT` 의 패키지에서 검색 → 없으면 `GOPATH` 패키지를 검색한다.
- `go install` 명령어 : `GOPATH/pkg` 에 패키지 등록
- `godoc -http:=6060` 으로 본인 패키지 메소드 및 주석을 확인 할 수 있다.

```go
// 사용자 패키지 작성 및 문서화 예제
package main

import (
	"fmt"
	oper "section12/arithmetic" // alias 사용 (패키지 중복 또는 약자로 사용)
)

func main() {
	// 패키지 사용 예제(사칙연산)
	nums := oper.Numbers{ X : 100, Y : 10}
	fmt.Println("Package Used(1) : ", nums.Plus())
	fmt.Println("Package Used(2) : ", nums.Minus())
	fmt.Println("Package Used(3) : ", nums.Mulit())
	fmt.Println("Package Used(4) : ", nums.Divide())
	fmt.Println("Package Used(5) : ", nums.SquarePlus())
	fmt.Println("Package Used(6) : ", nums.SquareMinus())
}
```

## 12.2 외부 저장소 패키지 설치 및 사용
### 12.2.1 사용자 패키지 설치 및 활용
- 외부 저장소에 저장된 패키지를 설치하여 사용할 수 있다.
1. `import` 선언 후 폴더로 이동하여 `go get` 명령어로 설치한다.
2. `go get 패키지 주소` 로 설치한다.

```go
package main

import (
	"github.com/tealeg/xlsx"
)

func main() {

	xfile := "section12/sample.xlsx"

	xlFile, err := xlsx.OpenFile(xfile)
	if err != nil {
		panic("Excel Loads Error!")
	}

	for _, sheet := range xlFile.Sheets {
		for _, row := range sheet.Cols {
			for _, cell := range row.Cells {
				text := cell.st
			}
		}
	}
}
```
