# [스프링 5.0 마스터] 11.2 Spring Reactive - 1


## 1. 리액티브 스트림
### 1.1 리액티브 스트림 의존성 추가
```bash
implementation ('org.reactivestreams:reactive-streams:1.0.2')
implementation ('org.reactivestreams:reactive-streams-tck:1.0.2')
```
### 1.2 리액티브 스트림의 구성요소
+ **Publisher** : 데이터 제공자. 구독한 구독자들에게 구독 정보를 토대로 데이터를 제공한다.
+ **Subscriber** : 데이터 소모자. 제공자로부터 데이터를 받아 소모한다.
+ **Subscription** : 구독 정보. Subscriber 는 Publisher 를 구독하여 데이터(n) 요청할 수 있다. 

```java
// Publisher
package org.reactivestreams;

public interface Publisher<T> {
    void subscribe(Subscriber<? super T> var1);
}

// Subscriber
package org.reactivestreams;

public interface Subscriber<T> {
    void onSubscribe(Subscription var1);

    void onNext(T var1);

    void onError(Throwable var1);

    void onComplete();
}

// Subscription
package org.reactivestreams;

public interface Subscription {
    void request(long var1);

    void cancel();
}
```
![reactive-stream-diagram](https://grokonez.com/wp-content/uploads/2017/04/reactive-stream-flow-interface-behavior.png)

#### Publisher
+ `void subscribe(Subscriber<? super T> var1);` : **Subscriber** 객체를 매개변수로 입력받아 **Publisher** 가 **Subscriber** 관리 할 수 있다. 그리고 **Subscriber** 의 메소드를 사용할 수 있다.  

#### Subscriber
+ `onSubscribe()` : **Publisher** 를 구독하게 되면 호출된다.
+ `onNext()` : 비동기 식으로 `Executor` 를 통해 구독자에게 메세지를 게시할 수 있다.
+ `onError()` : 에러가 발생 시 호출된다.
+ `onComplete()` : 더 게시된 메세지 요소가 없을 경우 다음 작업을 수행한다.

#### Subscription
+ `request()` :  **Publisher** 에게 게시된 데이터를 요청한다.
+ `cancel()` : 데이터에 대한 요청을 취소한다.

---
#### 출처 및 참고 사이트
+ Java9 Reactive Stream API 
    - http://www.reactive-streams.org/
+ Java9 Reactive Stream 에 대한 자세한 설명 및 내용 참고 
    - http://jess-m.tistory.com/18
    - https://grokonez.com/java/java-9/java-9-flow-api-reactive-streams


## 2. 리액터
리액터는 스프링 피보탈 팀의 리액티브 프레임워크이며, 리액티브 스트림을 기반으로 한다. 

### 2.1 의존성 추가
#### build.gradle
```bash
implementation      ('io.projectreactor:reactor-core:3.2.5.RELEASE')
testImplementation  ('io.projectreactor:reactor-test:3.2.5.RELEASE')
```
```bash
plugins {
    id 'java'
    id 'war'
    id 'org.springframework.boot' version '2.1.1.RELEASE'
}

apply plugin: 'java'
apply plugin: 'io.spring.dependency-management'

group 'com.mastering.spring'
version '1.0-SNAPSHOT'

sourceCompatibility = 1.11

repositories {
    mavenCentral()
}

dependencies {

    implementation ('org.springframework.boot:spring-boot-starter')

    implementation ('org.reactivestreams:reactive-streams:1.0.2')
    implementation ('org.reactivestreams:reactive-streams-tck:1.0.2')

    implementation      ('io.projectreactor:reactor-core:3.2.5.RELEASE')
    testImplementation  ('io.projectreactor:reactor-test:3.2.5.RELEASE')

    testImplementation  ('org.springframework.boot:spring-boot-starter-test:2.1.2.RELEASE')
    testImplementation  ('junit:junit:4.12')
}
```
### 2.2 Mono 와 Flux
Mono 와 Flux 모두 Reactive Stream 의 Publisher 인터페이스를 구현하고 있으며, Reactor 에서 제공하는 여러 연산자(operators) 의 조합을 통해 스트림을 가공할 수 있다.

+ `Mono` : 요소가 아예 없거나 하나의 결과만을 처리하기 위한 Reactor 객체
+ `Flux` : 결과가 0-N 개인 여러 개의 결과를 처리하는 Reactor 객체

```java
package com.mastering.spring.reactive;

import org.junit.Test;
import reactor.core.publisher.Mono;

import java.time.Duration;

public class SpringReactiveTest {

    @Test
    public void monoExample() throws InterruptedException {

        // 5초 후에 하나의 요소를 방출한다.
        Mono<String> stubMonoWithADelay = Mono.just("Ranga").delayElement(Duration.ofSeconds(5));

        // 모노 이벤트를 수신하고 콘솔에 기록한다.
        stubMonoWithADelay.subscribe(System.out::println);

        // 테스트 실행시간을 지연시킨다.
        Thread.sleep(10000);
    }

}
```

```java
/**
 * Consumer 를 명시적으로 정의
 */
class SystemOutConsumer implements Consumer<String> {
    @Override
    public void accept(String s) {
        System.out.println("Received " + s + " at" + new Date());
    }
}

```
```java
// 모노 이벤트를 수신하고 콘솔에 기록한다.
// stubMonoWithADelay.subscribe(System.out::println);
stubMonoWithADelay.subscribe(new SystemOutConsumer());

```

```java
class WelcomeConsumer implements Consumer<String> {
    @Override
    public void accept(String s) {
        System.out.println("Welcome " + s);

    }
}
```

```java
// 모노 이벤트를 수신하고 콘솔에 기록한다.
// stubMonoWithADelay.subscribe(System.out::println);
stubMonoWithADelay.subscribe(new SystemOutConsumer());
stubMonoWithADelay.subscribe(new WelcomeConsumer());
```

```java

@Test
public void simpleFluxStream() {
        Flux<String> stubFluxStream = Flux.just("Jane", "Joe");
        stubFluxStream.subscribe(new SystemOutConsumer());
    }
}
```
