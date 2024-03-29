# [Tucker의 Go 프로그래밍] 실수 오차


> Tucker의 Go 언어 프로그래밍 책 내용을 참고 및 정리해서 작성하였습니다.

실수 끼리의 비교연산에서 예기치 않은 결과가 나올 때가 있다. 0.1 + 0.2 = 0.3의 결과를 예상하였지만 실제 출력값은 0.30000000000000004가 출력되었다.

```go
package main

import "fmt"

func main() {
    var a float64 = 0.1
    var b float64 = 0.2
    var c float64 = 0.3
    
    fmt.Printf("%f + %f == %f : %v\n", a, b, c, a+b == c)
    fmt.Println(a+b)
}
```
> #### Result
> 0.100000 + 0.200000 == 0.300000 : false\
> 0.30000000000000004

## 실수 오차 
컴퓨터에서는 실수값을 표현할 때 지수부와 소수부로 나누어서 표현한다. 컴퓨터는 지수부와 소수부가 10진수 기준이 아니라 2진수 기준으로 되어 있다. 그렇기 때문에 10진수를 정확히 표현하기 어려운 문제가 있다.
> 예를들어 0.375는 0.3 + 0.07 + 0.005로 나타낼 수 있고, 십진수로 표현하면 다음과 같다.
> $3 * 10^{-1} + 7 * 10^{-2} + 5 * 10^{-3} $\
> \
> 컴퓨터의 경우 2진수 숫자 체계를 다루므로, $1 * 2^{-2} + 1 * 2^{-3}$ 로 나타낼 수 있다.\
> ($2^{-1} = 0.5, 2^{-2} = 0.25, 2^{-3} = 0.125$)

대부분의 소수점 이하 숫자들은 2의 음의 승수료 표현하기가 어렵다. 
그래서 0.376값은 float32 타입으로 최대한 가까운 근사값인 0.375999987125396728515625로 표현이 된다.
이렇게 실수를 근사값으로 표현하면서 발생되는 문제를 **부동 소수점 반올림 오차(rounding error)** 라고도 한다.

두 실수끼리 같은지 다른지 비교하기 위해서는 어떻게 할 수 있을까?

### 1. 작은 오차 무시하기
- 아주 작은 오차는 무시하는 방법으로 비교해보자. 무시할 오차 한계를 정의하여 그 값을 무시하고 비교한다.
```go
package main

import "fmt"

const epsilon = 0.00001 // 매우 작은 상수값을 선언. 무시할 오차 한계를 정의

// 두 값의 차이가 epsilon 비교해서 작을 경우 두 값이 같다고 간주한다.
func equal(a, b float64) bool {
    if a > b {
        if a - b <= epsilon {
            return true
        } else {
            return false
        }
    } else {
        if b - a <= epsilon {
            return true
        } else {
            return false
        }
    }
}

func main() {
    var a float64 = 0.1
    var b float64 = 0.2
    var c float64 = 0.3
    
    fmt.Printf("%0.18f + %0.18f = %0.18f\n", a, b, a + b)
    fmt.Printf("%0.18f == %0.18f : %v\n", c, a + b, equal((a + b), c))
    
    a = 0.0000000000004
    b = 0.0000000000002
    c = 0.0000000000007
    
    fmt.Printf("%g == %g : %v\n", c, a+b, equal((a+b), c))
}
```
> #### Result
> 0.100000000000000006 + 0.200000000000000011 = 0.300000000000000044 \
> 0.299999999999999989 == 0.300000000000000044 : true\
> 7e-13 == 6.000000000000001e-13 : true

#### 문제점
- 오차의 기준을 명확히 정할 수 없다. float64의 경우 $10^{-308}$ ~ $10^{308}$ 까지 매우 큰 값의 범위를 갖는데, 앞서 사용한 상수 epsilon의 경우 0.0000234에 비하면 무시할 만큼 크게 작지 않은 값이다.
- a,b,c의 값을 매우 작은 값으로 바꾸게 되면 `7e-13 == 6.000000000000001e-13 : true` 결과처럼 서로 같다는 문제가 발생한다.

### 2. 최소 비트 차이 비교

```go
package main

import (
	"fmt"
	"strings"
)

func main() {

	var a float64 = 0.1
	var b float64 = 0.2
	var c float64 = 0.3

	fmt.Printf("float64 a : %0.60f\n", a)
	fmt.Printf("float64 b : %0.60f\n", b)

	fmt.Println(strings.Repeat("-", 100))
	fmt.Printf("float64 a+b : %0.60f\n", a+b)
	fmt.Printf("float64 0.3 : %0.60f\n", c)

}
```
> #### Result
> float64 a : 0.100000000000000005551115123125782702118158340454101562500000\
> float64 b : 0.200000000000000011102230246251565404236316680908203125000000\
> ----------------------------------------------------------------------------------------------------\
> float64 a+b : 0.300000000000000044408920985006261616945266723632812500000000\
> float64 0.3 : 0.299999999999999988897769753748434595763683319091796875000000

- 실수 0.1의 근사값(float64) 0.1000000000000000055511151231257827021181583404541015625과 실수 0.2의 근사값(float64) 0.200000000000000011102230246251565404236316680908203125이 덧셈 계산이 발생하여 0.3000000000000000444089209850062616169452667236328125 이라는 결과가 출력되었다. 
- 실수 0.3의 근사값(float64) 0.299999999999999988897769753748434595763683319091796875으로 출력되었다.
- 두 값 모두 0.3과 정확히 같지는 않지만 0.3보다 아주 작고 혹은 0.3 보다 아주 크다. 두 값은 2진수로 표현했을 때 마지막 1 비트 차이밖에 나지 않는다. 즉 0.3을 표현할 수 있는 실수 타입 범위에서 가장 작은 차이다. 
- 만약 어떤 값이 이 두값의 사이라면 0.3과 같다고 간주할 수 있다.

- Go lang에서는 math 패키지에 **Nextafter()** 함수를 통해 x에서 y를 향한 1비트만 조정한 값을 반환한다.
> func Nextafter(x, y float64) (r float64)  
>  + x>y : x보다 1비트 감소시킨 값을 반환
>  + x<y : x보다 1비트 증가시킨 값을 반환
>  + x=y : x 

즉 가장 작은 오차만큼을 y를 향해서 1비트를 조정하여 실수값의 대소를 비교할 수 있다.

#### Nextafter 함수를 이용한 실수값 대소비교
```go
package main

import (
	"fmt"
	"math"
)

func equal(a, b float64) bool {
	return math.Nextafter(a, b) == b
}

func main() {
	var a float64 = 0.1
	var b float64 = 0.2
	var c float64 = 0.3

	fmt.Printf("%0.18f + %0.18f = %0.18f\n", a, b, a + b)
	fmt.Printf("%0.18f == %0.18f : %v\n", c, a+b, equal(a+b, c))

	a = 0.0000000000004
	b = 0.0000000000002
	c = 0.0000000000007

	fmt.Printf("%g == %g : %v\n", c, a+b, equal(a+b, c))
}
```
> #### Result
> 0.100000000000000006 + 0.200000000000000011 = 0.300000000000000044\
> 0.299999999999999989 == 0.300000000000000044 : true\
> 7e-13 == 6.000000000000001e-13 : false

### 3. math/big 패키지의 Float 객체 이용
- 제작하는 프로그램이 금융 프로그램이라면 math/big 패키지에서 제공하는 Float 객체를 사용해야 한다.
- 정밀도를 직접 조절 할 수 있기 때문에 정밀도를 높여 더 정확한 수치 계산이 가능하다.

#### math/big 패키지의 Float를 이용하여 실수 비교 예제
```go
package main

import (
	"fmt"
	"math/big"
)

func main() {
	a, _ := new(big.Float).SetString("0.1")
	b, _ := new(big.Float).SetString("0.2")
	c, _ := new(big.Float).SetString("0.3")

	d := new(big.Float).Add(a, b)
	fmt.Println(a, b, c, d)
	fmt.Println(c.Cmp(d))
}
```
> #### Result
> 0.1 0.2 0.3 0.3\
> 0



