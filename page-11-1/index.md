# [Mastering Spring 5.0] 11.1 Reactive Programming


## 리액티브 시스템

새로운 디바이스 (모바일, 태블릿 등) real-time data 에 대한 수요 증가

- **대량의 프로세스 처리 로드 발생**
- **데이터 볼륨이 기하급수적으로 증가**
- **인프라 유지 보수 비용 증가**

### Reactive 시스템 특징

`Reactive manifesto` : https://www.reactivemanifesto.org/ko

{{<figure src="/posts/images/spring/reactive-traits-ko.svg">}}


Reative Manifesto 는 다음 네 가지 핵심 원칙에 따라 `Reactive System` 의 특성을 개략적으로 설명하고 있다.

+ `반응성 (Responsive)` : 모든 응답은 적시에 빠르고 일관된 대응을 제공하며 신뢰할수 있으며 일관된 서비스 품질을 제공한다.
    
+ `회복력 (Resilient)` : 각각의 구성요소 들이 분리되어 있기 때문에 구성요소 중 하나에 문제가 발생하더라도 전체 시스템이 다운되는 것을 방지하고 복구 할 수 있도록 보장한다. 또한 장애의 발생이 예외적인 현상이 아닌 정상적인 기능의 일부로 처리한다.

+ `유연성 (Elastic)` : 요청을 처리하기 위해 할당된 리소스를 늘리거나 줄임으로써 요청 로드의 변화에 대응할 수 있다. ( auto scale )

+ `메시지 기반(Message-driven)` : 시스템의 구성요소들이 메세지(또는 이벤트) 를 통해 이루어진다. 여기에서 구성요소들은 컴포넌트, 서비스, 객체, API 무엇이라도 될 수 있으며 과거처럼 메서드 호출이나 RPC 같은 블로킹 방식으로 의사소통 하지 않고, 보내고 잊는(fire-and-forget) 방식으로 메시지를 주고 받으며 소통한다. 

### Reactive Keyworld

+ A reactive stream should be `non-blocking`
+ It should be `a stream of data`
+ It should work `asynchronously`
+ And it should be able to handle `back pressure`.

## Non-Bloking

### Bloking
블로킹은 요청이 발생하고 작업을 진행하는 동안 프로그램의 진행을 **멈추고(block)** 완료될 때까지 모든 일을 중단한 상태로 대기해야 하는 것을 블로킹 방식이라고 한다.

### Blocking Java Socket 통신 예제
클라이언트가 접속할 때 까지 `accept()` 메서드는 항상 블로킹 된다.

```java
Socket clientSocket = serverSocket.accept();
...
String request, response;
while ((request = in.readLine()) != null) {
    response = processRequest(request);
    out.println(response);
    if ("Done".equals(request)) {
        break;
    }
}
```
+ 또한 클라이언트가 보낸 스트림이 `완료` 또는 `입력 끝 문자` ( Ctrl + C 입력 시) 를 보낼 때 까지 발생한다.
+ 클라이언트가 많을 수록 스레드 수가 증가하고 서버에 심각한 성능저하 발생 -> 대안으로 `스레드 풀`을 사용

### Non-Blocking

어떤 스레드에서 오류가 발생하거나 멈추었을 때 **다른 쓰레드에게 영향을 끼치지 않도록** 하는 방법이다. 요청한 작업(스레드) 이 진행 되는 동안 즉시 다음 작업을 처리함으로써 시스템 자원을 더 효율적으로 활욜이 가능하다. 그러나 요청한 작업 이후 후속 작업을 이어서 진행할 수 있도록 **별도의 약속(Polling, Callback Function 등)** 이 필요하다. 

```java

this.selector = Selector.open();
ServerSocketChannel serverChannel = ServerSocketChannel.open();
serverChannel.configureBlocking(false);

// bind server socket channel to port
serverChannel.socket().bind(listenAddress);
serverChannel.register(this.selector, SelectionKey.OP_ACCEPT);    
```

`accept()` 메서드가 블로킹 되지 않고 바로 리턴되기 때문에, 클라이언트가 연결 요청을 보내기 전까지 while 블록 코드가 쉴새 없이 반복되어 CPU가 과도하게 소비되는 문제점이 발생한다. 그래서 넌블로킹은 이벤트 리스너 역할을 하는 `셀렉터(Selector)` 를 사용한다. 이 Selector 를 넌블로킹 채널에 등록해 놓으면 클라이언트의 연결 요청이 들어오거나 데이터가 도착할 경우 채널은 Selector 에 통보한다. Selector는 통보한 채널들을 선택해 작업 스레드가 accept() 또는 read() 메소드를 실행한다.


```java
while (true) {
    int readyCount = selector.select();
        if (readyCount == 0) {
            continue;
        }
}
```

## Backpressure

### 비동기식 데이터 스트림 처리에 대한 이슈
+ 너무 큰 데이터 스트림 -> `busy waiting`
+ 너무 빠른 데이터 스트림 전송 속도 -> `out of memory exception`

Backpressure 는 안정적으로 처리하기에 너무 큰 데이터 혹은 데이터를 신뢰 있게 처리 할 수 있는 속도로 구독자에게 제공하는 방법이다. Reactive 데이터 스트림의  Push 와 Pull 을 관리하는 Buffer 를 사용하여 구독자는 특정 양의 데이터를 요청하고 소스는 구성된 데이터를 해당 데이터로 유동적으로 Push 와 Pull 을 하는 방식이다.  

{{<figure src="/posts/images/spring/backpressure-handling.png">}}


## 출처 및 참고 사이트
+ https://homoefficio.github.io/2017/02/19/Blocking-NonBlocking-Synchronous-Asynchronous/
+ http://palpit.tistory.com/645
+ https://tech.peoplefund.co.kr/2017/08/02/non-blocking-asynchronous-concurrency.html
+ https://medium.com/coderscorner/tale-of-client-server-and-socket-a6ef54a74763
+ http://www.zdnet.co.kr/view/?no=20161010104628
+ https://www.slideshare.net/dnnddane/why-reactivereactive-programming-spring-5
+ https://www.youtube.com/watch?v=UIrwrW5A2co
+ ReactiveX History : https://ahea.wordpress.com/2017/02/03/reactive-history/
+ https://www.slideshare.net/ktoso/reactive-stream-processing-with-akka-streams
+ https://www.e4developer.com/2018/04/28/springs-webflux-reactor-parallelism-and-backpressure/







