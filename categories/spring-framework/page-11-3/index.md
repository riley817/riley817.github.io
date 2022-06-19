# [스프링 5.0 마스터] 11.2 Spring Reactive - 2

## 11.2 Spring Reactive - 2

### 3. 스프링 웹 리액티브
스프링 웹 리액티브는 스프링 프레임워크 5 의 새로운 기능 중 하나이다. 이 기능은 웹 애플리케이션의 리액티브 기능을 제공한다. 스프링 웹 리액티브는 스프링 MVC 와 동일한 기본 프로그래밍 모델을 기반으로 한다.

![web-reactive-overview](https://docs.spring.io/spring-framework/docs/5.0.0.M1/spring-framework-reference/html/images/web-reactive-overview.png)

#### 3.1 웹 리액티브 스트림 의존성 추가
```bash
implementation ('org.springframework.boot:spring-boot-starter-webflux')
```
#### 3.2 리액티브 컨트롤러 생성
```java
@RestController
public class StockPriceEventController {

    @GetMapping("/stocks/price/{stockCode}")
    Flux<String> retrieveStockPriceHardcoded(@PathVariable("stockCode") String stockCode) {
        return Flux.interval(Duration.ofSeconds(5))
                .map(l -> getCurrentDate() +" : " + getRandomNumber(100, 125)).log();

    }

    private String getCurrentDate() {
        return (new Date()).toString();
    }

    private int getRandomNumber(int min, int max) {
        return ThreadLocalRandom.current().nextInt(min, max + 1);
    }

}
```

#### 3.3 HTML 뷰 생성
```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <p>
        <button id="subscribe-button">Get Latest IBM Price</button>
        <ul id="display"></ul>
    </p>

    <script>
        addEvent("click", document.getElementById('subscribe-button'),
            () => registerEventSourceAndAddResponseTo("/stocks/price/IBM", "display"));

        function registerEventSourceAndAddResponseTo(uri, elementId) {
            let stringEvents = document.getElementById(elementId);
            let stringEventSource = new EventSource(uri)

            stringEventSource.onmessage = (e) => {
                let newElement = document.createElement("li");
                newElement.innerHTML = e.data;
                stringEvents.appendChild(newElement);
            }
        }

        function addEvent(event, elem, func) {
            if (typeof (EventSource) !== 'undefined') {
                elem.addEventListener(event, func, false);
            } else {
                elem[event] = func;
            }
        }

    </script>
</body>
</html>
```


