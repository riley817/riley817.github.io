# Section 9 : Go 병행처리

> 인프런 쉽고 빠르게 끝내는 GO언어 프로그래밍 핵심 기초 입문 과정 강의 정리 

## 9.1 고루틴
### 9.1.1 GoRoutine
- **goroutine**은 go 런타임이 관리하는 lightweight 논리적(가장적) Thread이다.
- 생성 방법이 매우 간단하고 리소스를 매우 적게 사용한다. → 수 많은 고루틴을 동시에 생성 및 실행이 가능
    - Java나 Python의 스레드는 MB단위인 반면에 GO KB 단위이다.
- 비동기적 `asynchronously` 함수 루틴을 실행 한다.
- 채널을 통해 루틴간 통신이 가능하다.
- 공유 메모리를 사용시에 정확한 동기화 코딩이 필요하다.
- 싱글 루틴에 비해 항상 빠르지는 않다.

#### 멀티 스레드의 장점과 단점

**장점**

- 응답성이 향상
- 자원공유를 효율적으로 활용 가능
- 작업이 분리되어 코드 간결

**단점**

- 구현하기 어려움.
- 테스트 및 디버깅이 어렵다
- 전체 프로세스의 사이드 이펙트가 발생
- 성능 저하
- 동기화 코딩을 반드시 숙지
- 데드락(교착상태)

#### 고루틴 기초사용 예제
- 메인 루틴이 종료되면 서브루틴(exe2, exe3) 도 종료되기 때문에 메인루틴의 실행시간을 줌

```go
package main

import (
    "fmt"
    "time"
)

func exe1() {
    fmt.Println("exe1 func start", time.Now())
    time.Sleep(1 * time.Second) // 1초
    fmt.Println("exe1 func end", time.Now())
}

func exe2() {
    fmt.Println("exe2 func start", time.Now())
    time.Sleep(1 * time.Second) // 1초
    fmt.Println("exe2 func end", time.Now())
}

func exe3() {
    fmt.Println("exe3 func start", time.Now())
    time.Sleep(1 * time.Second) // 1초
    fmt.Println("exe3 func end", time.Now())
}

// 고루틴(Goroutine) 기초
func main() {
    exe1() // 가장 먼저 실행 (일반적인 실행 흐름)
    // exe2()
    // exe3()
    
    fmt.Println("Main Routine Start", time.Now())
    // 별도의 실행 흐름을 생성
    go exe2()
    go exe3()
    time.Sleep(4 * time.Second)
    fmt.Println("Main Routine End", time.Now())
}
```

- Result

    >exe1 func start 2021-04-13 11:40:12.15236 +0900 KST m=+0.000091278\
    >exe1 func end 2021-04-13 11:40:13.157639 +0900 KST m=+1.005357230\
    >Main Routine Start 2021-04-13 11:40:13.157766 +0900 KST m=+1.005484383\
    >exe3 func start 2021-04-13 11:40:13.157812 +0900 KST m=+1.005529633\
    >exe2 func start 2021-04-13 11:40:13.157855 +0900 KST m=+1.005573044\
    >exe2 func end 2021-04-13 11:40:14.162966 +0900 KST m=+2.010670709\
    >exe3 func end 2021-04-13 11:40:14.163006 +0900 KST m=+2.010710760\
    >Main Routine End 2021-04-13 11:40:17.158061 +0900 KST m=+5.005725592\
    >\
    >// exe2, exe3은 별도의 실행흐름에 따라 실행된다.

#### goroutine 반복문 예제
```go
package main

import (
	"fmt"
	"time"
)

func exe(name string) {
    fmt.Println(name, "Start : ", time.Now())
    for i := 0; i < 1000; i++ {
        fmt.Println(name, ">>>>>", i)
    }
    fmt.Println(name, "End : ", time.Now())
}

func main() {
    // 고루틴 (Goroutine)
    exe("t1")
    
    fmt.Println("Main Routine Start : ", time.Now())
    go exe("t2")
    go exe("t3")
    
    time.Sleep(4 * time.Second) // time.Second, Minute, Hour, Millisecond ...
    fmt.Println("Main Routine End : ", time.Now())
}
```

### 9.1.2 멀티 코어 (다중 CPU) 활용하기
- Go는 디폴트로 1개의 CPU를 사용한다. → 여러개의 Go 루틴을 만들더라도 1 개의 CPU에서 작업을 시분할하여 처리한다 ⇒ **동시성(concurrent)** 처리
- 만약 머신이 복수개의 CPU를 가진경우, Go 프로그램을 다중 CPU에서 **병렬처리(parallel)** 하게 할 수 있다. 병렬처리를 위해서는 `runtime.GOMAXPROCS(CPU수)` 함수를 호출해야 한다.

```go
package main

import (
    "fmt"
    "time"
    "math/rand"
    "runtime"
)

func exe(name int) {
    r := rand.Intn(100)
    fmt.Println(name, "Start : ", time.Now())
    for i := 0; i < 100; i++ {
        fmt.Println(name, ">>>>>>>", r, i)
    }
    fmt.Println(name, "End : ", time.Now())
}

func main() {
    runtime.GOMAXPROCS(runtime.NumCPU()) // 현 시스템 cpu 코어 개수 반환 후 설정
    fmt.Println("Current System CPU : ", runtime.GOMAXPROCS(0)) // 설정 값 출력
    
    // Example 1
    fmt.Println("Main Routine Start : ", time.Now())
    for i := 0; i < 100; i++ {
        go exe(i) // GO ROUTINE 100개 생성
    }
    
    time.Sleep(5 * time.Second)
    fmt.Println("Main Routine End : ", time.Now())
}
```

### 9.1.3 클로저를 고루틴으로 실행할 때 주의할 점
반복문 내에 고루틴을 삽입하여 클로저를 통해 바깥 변수를 참조할 때 유의하여 사용해야 한다.

```go
for i := 0; i < 1000; i++ {
    go func() {
        fmt.Println("Closure Test => ", i)
    }()
}
```
이렇게 사용하는 경우 출력이 i가 0부터 999 까지 출력 될 것이라 예상하지만 결과는 다르다.

- 실행 결과

    >Closure Test =>  1000\
    >Closure Test =>  1000\
    >Closure Test =>  1000\
    >Closure Test =>  1000\
    >Closure Test =>  1000\
    >Closure Test =>  1000\
    >Closure Test =>  1000\
    >...

일반적으로 반복문 클로저는 즉시 실행되지만, 고루틴으로 클로저를 사용할 때 반복문이 종료된 뒤 가장 나중에 실행된다. 그러므로 i 가 반복문을 다 돌고난 1000의 값이 나오게 된다. 고루틴 클로저를 사용할때 반복문에 의해 변경되는 값은 파라미터로 넘겨주어야만 올바른 결과를 얻을 수 있다.

#### 개선소스

```go
package main

import (
	"fmt"
	"runtime"
	"time"
)

func main() {
    runtime.GOMAXPROCS(1)
    s := "Go Routine Closure : "
    
    for i := 0; i < 1000; i++ {
        go func(n int) {
            fmt.Println(s, n, " - ", time.Now())
        }(i)
    }
    time.Sleep(5 * time.Second)
}
```

## 9.2 채널
### 9.2.1 채널
- Go 채널은 고루틴간의 데이터를 주고 받는 통로라고 할 수 있다.
- 고루틴 끼리 데이터를 주고 받는데 사용되며, 상태편이 준비될 때까지 채널에서 대기함으로서 별도의 `Lock` 을 걸지않고 데이터를 동기화하는데 사용된다.
- 채널은 `make()` 함수를 통해 미리 생성되어야 하며(=참조타입임) 동기방식으로 동작한다.
- 지정한 데이터 타입만 주고 받을 수 있다. → `interface{}` 를 통해 자료형 상관없이 전송 및 수신 가능
- 멀티프로세싱 처리에서 교착상태(경쟁상태)에 주의해야 한다.
- `<-`, `->` 을 통해 데이터를 주고 받는다.
  - **채널 ← 데이터** : 송신
  - **변수 ← 채널** : 수신

```go
package main

import "fmt"

func rangeSum(rg int, c chan int) {
  sum := 0
  for i := 1; i <= rg; i++ {
      sum += i
  }
  c <- sum
}

func main() {
  // 채널(Channel)
  c := make(chan int)
  
  go rangeSum(1000, c)
  go rangeSum(7000, c)
  go rangeSum(5000, c)
  
  result1 := <- c
  result2 := <- c
  result3 := <- c
  
  fmt.Println("Example 1 : ", result1)
  fmt.Println("Example 2 : ", result2)
  fmt.Println("Example 3 : ", result3)
}
```

- Result

    >Example 1 :  12502500\
    >Example 2 :  500500\
    >Example 3 :  24503500

### 9.2.2 Go 채널 버퍼링
- `Unbuffered Channel` ,`Buffered Channel`  두가지의 채널이 있다.

#### Unbuffered Channel
- 수신자가 데이터를 받을 때 까지 송신자가 데이터를 보내느 채널에 묶여 있게 된다.

```go
package main

import (
	"fmt"
	"time"
)

func main() {
  // Channel
  // 동기 : 버퍼 미사용
  ch := make(chan bool)
  cnt := 6
  
  go func() {
      for i := 0; i < cnt; i++ {
          ch <- true
          fmt.Println("Go => ", i)
          time.Sleep(1 * time.Second)
      }
  }()
  
  for i := 0; i < cnt; i++ {
      <- ch
      fmt.Println("Main : ", i)
  }
}
```

- Result

  ![2021-04-15_10.14.34.png](/categories/images/go/_2021-04-15_10.14.34.png)

#### Buffered Channel
- 수신자가 받을 준비가 되어 있지 않을 지라도 지정된 버퍼만큼 데이터를 보내고 계속 다른 일을 수행할 수 있다.
- `make(chan type, N)` 함수를 통해 생성할 수 있으며 N 에 사용할 버퍼의 개수를 넣는다.
- **발신** : 가득차면 대기, 비어있으면 동작
- **수신** : 비어있으면 대기, 가득차면 작동

```go
package main

import (
	"fmt"
	"runtime"
)

func main() {
  runtime.GOMAXPROCS(1)
  ch  := make(chan bool, 2)
  cnt := 12
  
  go func() {
      for i := 0; i < cnt; i++ {
          ch <- true
          fmt.Println("Go => ", i)
      }
  }()
  
  for i := 0; i < cnt; i++ {
      <- ch
      fmt.Println("Main : ", i)
  }
}
```

- Result

  ![_2021-04-15_10.18.26.png](/categories/images/go/_2021-04-15_10.18.26.png)

버퍼 채널의 경우 수신자가 당장 없더라도 최대버퍼 수까지 데이터를 보낼 수 있다.
버퍼채널을 이용하지 않는 경우 `fatal error: all goroutines are asleep - deadlock!` 에러발생

```go
package main
 
import "fmt"
 
func main() {
  ch := make(chan int, 1)
  
  //수신자가 없더라도 보낼 수 있다.
  ch <- 101
  
  fmt.Println(<-ch)
}
```

### 9.2.3 채널 닫기
- 채널을 오픈하고 데이터를 송신한 뒤 `close()` 함수를 통해 채널을 닫을 수 있다.
- 닫힌 채널에서 값을 전송할 경우 `panic` 예외가 발생한다.

#### 채널 Range

- 채널에서 송신자가 송신을 한 후, 채널을 닫을 수 있다. 그리고 수신자는 임의의 갯수의 데이터를 채널이 닫힐 때까지 계속 수신할 수 있다.
- range로 순회하면서 채널이 닫힐 때까지 값을 꺼내올 수 있다.

```go
package main

import "fmt"

func main() {
  ch := make(chan bool)
  go func() {
      for i := 0; i < 5; i++ {
          ch <- true
      }
      close(ch) // 5회 채널에 값 전송 후 채널 닫기
  }()
  
  for i := range ch { // 채널에서 값을 꺼내 온다
      fmt.Println("Example 1 : ", i)
  }
}
```

채널이 닫힌것을 파라미터를 통해 받아 올 수 있다.

```go
ch := make(chan int)

go func() {
  for i := 0; i < 3; i++ {
      ch <- 7777
  }
}()

if val1, ok := <- ch; ok {
	// ...
}
```

### 9.2.4 채널 파라미터
- 채널을 함수 파라미터를 전달할 때, 일반적으로 송수신을 모두 하는 채널을 전달하지만 특별히 해당 채널로 송신 및 수신 전용 채널을 지정할 수 있다.
- 수발신 전용 채널 설정 후 방향이 다를 경우 예외가 발생한다.
- **발신 전용** : channel ← 데이터형
- **수신 전용** : ← channel
- 채널 또한 함수의 반환 값으로 사용할 수 있다.

#### 발신 전용, 수신 전용 채널 생성
```go
package main

import (
  "fmt"
  "time"
)

// 발신 전용
func sendOnly(c chan<- int, cnt int) {
  for i := 0; i < cnt; i++ {
    c <- i
  }
  c <- 777
  // fmt.Println(<-c) // 수신 전용채널에서 발신 처리 시 예외 발생
}

// 수신 전용
func receiveOnly(c <-chan int) {
    for i := range c {
        fmt.Println("Received i : ", i)
    }
    fmt.Println(<-c)
}

func main() {
    c := make(chan int)

    go sendOnly(c, 10) // 발신 전용
    go receiveOnly(c) // 수신 전용

    time.Sleep(1 * time.Second)
}
```

#### 채널을 함수의 반환 값으로 사용

```go
package main

import "fmt"

// 수신 전용 채널을 리턴값으로 리턴
func sum(cnt int) <-chan int {
  sum := 0
  tot := make(chan int)
  
  go func() {
      for i := 1; i <= cnt; i++ {
          sum += i
      }
      tot <- sum
  }()
  return tot
}

func main() {
  c := sum(100)
  fmt.Println("Example 1 ",  <-c )
}
```

#### 반환 된 채널을 전달받아 다시 채널로 반환

```go
package main

import "fmt"

func receiveOnly(cnt int) <-chan int {
	sum := 0
	tot := make(chan int)
	go func() {
		for i := 1; i <= cnt; i++ {
			sum += i
		}
		tot <- sum
		tot <- 777
		tot <- 777
		close(tot)
	}()
	return tot
}

func total(c <-chan int) <-chan int {
	tot := make(chan int)
	go func() {
		a := 0
		for i := range c {
			a = a + i
		}
		tot <- a
		close(tot)
	}()
	return tot
}

func main() {
	// Channel

	// Example1
	c := receiveOnly(100) // 채널 반환
	output := total(c) // 채널 전달 후 반환

	fmt.Println("Example 1 : ", <-output)

}
```

### 9.2.5 채널 Select 구문

- select문은 여러 개의 case문에서 각기 다른 채널을 기다리다가 데이터가 수신되면 준비된 채널 case를 실행하게 된다.
- 일회성 구문이므로 for (반복문) 안에서 수행되어야 한다.
- 복수 채널에 신호가 오면 Go 런타임에서 랜덤하게 한개를 선택한다.
- select문에 defaut 문이 있으면 case문 채널이 준비되지 않더라도 계속 대기하지 않고 바로 default 문을 실행하므로 주의해야 한다.

```go
package main

import (
	"fmt"
	"time"
)

func main() {
	ch1 := make(chan int)
	ch2 := make(chan string)

	go func() {
		for {
			ch1 <- 77
			time.Sleep(250 * time.Millisecond)
		}
	}()

	go func() {
		for {
			ch2 <- "Golang Hi!"
			time.Sleep(500 * time.Millisecond)
		}
	}()

	go func() {
		for {
			select {
			case num := <- ch1:
				fmt.Println("Channel 1 => ", num)
			case str := <- ch2:
				fmt.Println("Channel 2 => ", str)
			// default:
				// fmt.Println("Default Test !")
			}
		}
	}()

	time.Sleep(7 * time.Second)
}
```

## 9.3 동기화
### 9.3.1 고루틴 동기화
- 다중 코어에서 어떤 작업을 분할하여 작업시 해당 작업과 다른 작업이 같은 메모리 영역을 건드릴 경우 예기치 않은 동작이나 치명적이 에러를 유발할 수 있다.
- OS 내부적으로 원자적 연산을 보장하기 위해 다양한 기법을 사용하지만 응용 프로그래머 입장에서도 멀티코어를 잘 처리하기 위해 코드안에서 동기화 처리가 매우 중요하다.

#### 동기화를 사용하지 않은 경우

```go
package main

import (
	"fmt"
	"runtime"
)

// 구조체 선언 - 여러 Go Routine 에서 접근하는 공유 데이터
type count struct {
	num int
}

func (c *count) increment() {
	c.num += 1
}

func (c *count) result() {
	fmt.Println(c.num)
}

func main() {
	// 동기화 사용하지 않은 경우 예제
	// 시스템 전체 CPU 사용
	runtime.GOMAXPROCS(runtime.NumCPU())

	c := count{num: 0}
	done := make(chan bool)

	for i := 1; i <= 10000; i++ {
		go func() {
			c.increment()
			done <- true
			runtime.Gosched() // 다른 CPU 에게 양보
		}()
	}

	for i := 1; i <= 10000; i++ {
		<- done
	}

	c.result()
}
```
- Result\
  실행할 때마다 다른 값을 반환
  ![_2021-04-15_10.56.11.png](/categories/images/go/_2021-04-15_10.56.11.png)


### 9.3.2 Mutex
- **상호 배제** → 고루틴들이 서로 running time에 영향을 주지 않게 한다. → 단독으로 실행되게 하는 기술
- 여러 고루틴에서 작업하는 공유 데이터를 보호한다.
- `sync.Mutex` 선언 후 `Lock`, `Unlock` 을 사용한다.

#### 동기화를 사용한 예제
```go
package main

import (
	"fmt"
	"runtime"
	"sync"
)

// 구조체 선언 - 여러 Go Routine 에서 접근하는 공유 데이터
type count struct {
	num int
	mutex sync.Mutex	// 구조체 내부에 있기때문에
}

// 공유 데이터가 변경이 되는 부분
func (c *count) increment() {
	// 공유 데이터 수정 전 뮤텍스로 보호
	c.mutex.Lock()
	c.num += 1
	// 공유 데이터 수정 후 보호 해제
	c.mutex.Unlock()
}

func (c *count) result() {
	fmt.Println(c.num)
}

func main() {
	// 동기화 사용한 예제
	// 시스템 전체 CPU 사용
	runtime.GOMAXPROCS(runtime.NumCPU())
	// runtime.GOMAXPROCS(1)

	c := count{num: 0}
	done := make(chan bool)

	for i := 1; i <= 10000; i++ {
		go func() {
			c.increment()
			done <- true
			runtime.Gosched() // 다른 CPU 에게 양보
		}()
	}

	for i := 1; i <= 10000; i++ {
		<- done
	}

	c.result()
}
```
- Result\
  ![_2021-04-15_10.59.06.png](/categories/images/go/_2021-04-15_10.59.06.png)


### 9.3.3 쓰기, 읽기 전용 Mutex
- `RWMutex` : 쓰기 Lock → 쓰기 시도 중에는 다른 곳에서 이전 값을 읽지 못함. 읽기 락, 쓰기 락 전부 방어(방지)
- `RMutex` : 읽기 Lock → 읽기 시도 중에 값 변경 방지. 즉 쓰기 락만 방어(방지)

#### 동기화 사용하지 않은 경우 예제
- 쓰기, 읽기 동작 순서가 일정하지 않아 잘못된 오류를 반환 할 가능성 증가

```go
package main

import (
	"fmt"
	"runtime"
	"time"
)

func main() {
	runtime.GOMAXPROCS(runtime.NumCPU())

	data := 0

	go func() {
		for i := 1; i <= 10; i++ {
			data += 1
			fmt.Println("Write : ", data)
			time.Sleep(200 * time.Millisecond)
		}
	}()

	go func() {
		for i := 1; i <= 10; i++ {
			fmt.Println("Read 1 : ", data)
			time.Sleep(1 * time.Second)
		}
	}()

	go func() {
		for i := 1; i <= 10; i++ {
			fmt.Println("Read 2 : ", data)
			time.Sleep(1 * time.Second)
		}
	}()

	time.Sleep(5 * time.Second)
}
```

#### 동기화 사용 예제
```go
package main

import (
	"fmt"
	"runtime"
	"sync"
	"time"
)

func main() {
	// 시스템 전체 cpu 사용
	runtime.GOMAXPROCS(runtime.NumCPU())

	data := 0
	mutex := new(sync.RWMutex) // var mutex = new (sync.RWMutex)

	go func() {
		for i := 1; i <= 10; i++ {
			// 쓰기 뮤텍스 잠금
			mutex.Lock()
			data += 1
			fmt.Println("Write : ", data)
			time.Sleep(200 * time.Millisecond)
			// 쓰기 뮤텍스 잠금 해제
			mutex.Unlock()
		}
	}()

	go func() {
		for i := 1; i <= 10; i++ {
			// 읽기 뮤텍스 잠금
			mutex.RLock()
			fmt.Println("Read 1 : ", data)
			time.Sleep(1 * time.Second)
			mutex.RUnlock()
			// 읽기 뮤텍스 해제
		}
	}()

	go func() {
		for i := 1; i <= 10; i++ {
			// 읽기 뮤텍스 잠금
			mutex.RLock()
			fmt.Println("Read 2 : ", data)
			time.Sleep(1 * time.Second)
			mutex.RUnlock()
			// 읽기 뮤텍스 해제
		}
	}()

	time.Sleep(10 * time.Second)
}
```

### 9.3.4 동기화 상태 메소드
- 동기화 상태(조건) 메소드
- `wait`, `signal` , `Broadcast` 사용

```go
package main

import (
	"fmt"
	"runtime"
	"sync"
	"time"
)

func main() {
	// 시스템 전체 CPU 사용
	runtime.GOMAXPROCS(runtime.NumCPU())

	var mutex = new(sync.Mutex)
	var condition = sync.NewCond(mutex)

	ch := make(chan int, 5) // 비동기 버퍼 채널

	for i := 0; i < 5; i++ {
		go func(n int) {
			mutex.Lock()
			ch <- 777
			fmt.Println("GoRoutine Wait -> ", n)
			condition.Wait() // 고 루틴 멈춤(대기)
			fmt.Println("Waiting End", n)
			mutex.Unlock()
		}(i)
	}

	for i := 0; i < 5; i++ {
		// <- ch
		fmt.Println("Received : ", <-ch )
	}

	/*for i := 0; i < 5; i++ {
		mutex.Lock()
		fmt.Println("Wake GoRoutine(Signal)", i)
		condition.Signal() // 모든 고 루틴 생성 후 한개 씩 깨우기
		mutex.Unlock()
	}*/

	mutex.Lock()
	fmt.Println("Wake Goroutine(Broadcast)")
	condition.Broadcast()
	mutex.Unlock()

	time.Sleep(2 * time.Second)
}
```

### 9.3.5 sync.Once
- 특정 함수를 한 번만 수행해야 할 때 `sync.Once` 를 사용한다.
- `sync.Once` 구조체는 다음과 같은 메서드를 제공한다.

```go
func (o *Once) Do(f func())
```

- 한 번만 수행해야 하는 함수를 `Do()` 메서드의 매개변수로 전달하여 실행하면 여러 고루틴에서 실행한다 해도 해당 함수는 한 번만 수행된다.
- 주로 초기화에 사용된다.

```go
package main

import (
	"fmt"
	"runtime"
	"sync"
	"time"
)

func onceTest() {
	// 이 부분에 한 번 실행 할 코드 작성
	// 초기화 작업 등
	fmt.Println("Once Test Execute!")
}

func main() {

	runtime.GOMAXPROCS(runtime.NumCPU())

	once := new(sync.Once) // Once 객체 생성

	for i := 0; i < 5; i++ {
		go func(n int) {
			fmt.Println("Go Routine -> ", n)
			once.Do(onceTest)
		}(i)
	}

	time.Sleep(2 * time.Second)
}
```

- Result\
  ![Untitled.png](/categories/images/go/Untitled2.png)

### 9.3.6 sync.WaitGroup
- `sync.WaitGroup` 는 모든 고루틴이 종료될 때까지 대기해야 할 때 사용한다.
- `sync.WaitGroup` 이 제공하는 메서드
  - `func (wg *WaitGroup) Add(delta int)` : WaitGroup에 대기중인 고루틴 개수 추가
  - `func (wg *WaitGroup) Done()` : 대기 중인 고루틴의 수행이 종료되는 것을 알려줌
  - `func (wg *WaitGroup) Wait()` : 모든 고루틴이 종료 될 때까지 대기

```go
package main

import (
	"fmt"
	"sync"
)

// 고루틴 동기화 고급 - 2
func main() {

	wg := new(sync.WaitGroup)

	for i := 0; i < 100; i++ {
		wg.Add(1)
		go func(n int) {
			fmt.Println("WaitGroup => ", n)
			wg.Done()
		}(i)
	}

	// Add 와 Done 횟수가 같아야한다.
	wg.Wait() // 대기 그룹이 끝날떄까지 기다린다.
	fmt.Println("WaitGroup End!")
}
```

- Result\
  ![Untitled3.png](/categories/images/go/Untitled3.png)

### 9.3.7 원자성을 보장하는 연산
- **원자성 `atomic` : 기능적으로 분할 가능한 완전 보증된 일련의 조작. 모두 성공하거나 모두 실패해야 한다.**
- 모든 조작이 완료 될 때까지 다른 프로세스 개입이 불가하다.
- `sync.atomic` 에서 원자적 연산자를 제공한다.
- 주로 공용 변수에 관한 계산에 사용한다.
- [atomic - The Go Programming Language](https://golang.org/pkg/sync/atomic)
  
#### sync/atomic 패키지가 제공하는 함수
|함수|설명|
|---|------|
|AddT|특정 포인터 변수에 값을 더함|
|CompareAndSwapT|특정 포인터 변수의 값을 주어진 값과 비교하여 같으면 새로운 값으로 대체함|
|LoadT|특정 포인터 변수의 값을 가져옴|
|StoreT|특정 포인터 변수에 값을 저장함|      
|SwapT|특정 포인터 변수에 새로운 값을 저장하고 이전 값을 가져옴|      

#### 원자성을 사용하지 않은 경우

```go
package main

import (
	"fmt"
	"runtime"
	"sync"
)

func main() {

	// 원자성을 사용하지 않은 경우 예제
	runtime.GOMAXPROCS(runtime.NumCPU())

	var cnt int64 = 0
	wg := new(sync.WaitGroup)

	for i := 0; i < 5000; i++ {
		wg.Add(1)
		go func(n int) {
			cnt += 1
			wg.Done()
		}(i)
	}

	for i := 0; i < 2000; i++ {
		wg.Add(1)
		go func(n int) {
			cnt -= 1
			wg.Done()
		}(i)
	}

	wg.Wait() // 대기 그룹이 끝날떄까지 기다린다.
	fmt.Println("WaitGroup End! cnt : ", cnt)
}
```

- Result\
  공용변수 cnt에 대해 올바르지 못한 결과값이 나옴.\
  ![Untitled.png](/categories/images/go/Untitled4.png)

#### 원자성 연산자를 사용한 경우 예제

```go
package main

import (
	"fmt"
	"runtime"
	"sync"
	"sync/atomic"
)

// 고루틴 동기화 고급 - 3
func main() {

	runtime.GOMAXPROCS(runtime.NumCPU())

	var cnt int64 = 0
	wg := new(sync.WaitGroup)

	for i := 0; i < 5000; i++ {
		wg.Add(1)
		go func(n int) {
			//cnt += 1
			atomic.AddInt64(&cnt, 1)
			wg.Done()
		}(i)
	}

	for i := 0; i < 2000; i++ {
		wg.Add(1)
		go func(n int) {
			//cnt -= 1
			atomic.AddInt64(&cnt, -1)
			wg.Done()
		}(i)
	}

	wg.Wait() // 대기 그룹이 끝날떄까지 기다린다.

	finalCnt := atomic.LoadInt64(&cnt)
	fmt.Println("WaitGroup End! cnt : ", cnt)
	fmt.Println("WaitGroup End! cnt : ", finalCnt)
}
```

- Result\
  ![Untitled.png](/categories/images/go/Untitled5.png)
