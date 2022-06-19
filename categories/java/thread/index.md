# Multi Thread

> 자바 기본 잊지 않게 정리하기! 🤔

## 프로세스와 스레드
* **프로세스(process)** : 운영체제에서 실행 중인 하나의 애플리케이션을 의미
* **멀티 태스킹(multi tasking)**
    * 운영 체제에서 두 가지 이상의 다중 작업(프로세스)를 동시에 처리하는 것을 의미.
    * 운영 체제에서는 멀티 태스킹을 할 수 있도록 CPU 및 메모리 자원을 적절히 할당하고 병렬로 실행시킨다. 할당받은 메모리를 가지고 실행하기 때문에 독립적이다.

## 메인 스레드
* 자바 애플리케이션은 메인 스레드(main thread)가 main() 메소드를 실행 하면서 시작.
* 메인 스레드는 필요에 따라 작업 스레드를 만들어 병렬로 실행 가능하다.
* 싱글 스레드에서는 메인 스레드가 종료되면 프로세스가 종료되지만, 멀티 스레드 애플리케이션에서는 실행 중인 스레드가 하나라도 있다면 프로세스는 종료되지 않는다.

## 작업 스레드 생성과 실행
* 자바에서는 스레드도 객체(클래스)로 생성된다.

### implements runnable
* **Runnable** : 작업 스레드가 실행할 수 있는 코드를 가지고 있는 객체
* 인터페이스 타입이므로 `run()`을 재정의하여 실행 코드를 작성
```java
Thread thread = new Thread(Runnable target);
```
* `start()` : 작업스레드는 매개값으로 받은 Runnable의 run() 메소드를 실행한다.
```java
Runnable task = new Task();
Thread thread = new Thread(task);

/* Ruunable 익명 객체 */
Thread thread = new Thread(new Runnable() {
    @Override
    public void run() {
        // 스레드가 실행할 코드
    }
});

/* Java8 람다식 */
Thread thread = new Thread(() -> {
   // 스레드가 실행할 코드
});

// 작업 스레드 실행
thread.start();
```

### extends thread
* `Thread` 클래스를 상속한 후 `run` 메소드를 재정의(`overriding`)해서 스레드가 실행할 코드를 작성
```java
public class WorkerThread extends Thread {
    @Override
    public void run() {
        // 스레드가 실행할 코드
    }
}
Thread thread = new WorkerThread();

/* 익명의 자식 객체로 생성 */
Thread thread = new Thread() {
    @Override
    public void run() {
        // 스레드가 실행할 코드
    }
};
```

## implements Runnable vs extends Thread
* 자바에서는 다중 상속을 허용하지 않기 때문에 `extends Thread`의 경우 다른 클래스의 상속을 받을 수 없다.
* 인터페이스의 경우 다중 상속이 가능하기 때문에 다른 클래스를 상속 받을 수 있다.
* 그러므로 `implements Runnable`을 통해 재 정의하여 확장하는 경우가 많다.

## 스레드의 이름
* 스레드는 자신의 이름을 `setName()`로 변경한다.
* 스레드 객체의 참조가 필요한 경우 `Thread.currentThread()`로 코드를 실행하는 현재 스레드의 참조를 얻을 수 있다.
```java
Thread thread = Thread.currentThread();
```

## 스레드의 스케줄링
* **동시성(Concurrency)** : 하나의 코어에서 멀티 스레드가 번갈아가며 실행하는 성질
* **병렬성(Parallelism)** : 멀티 코어에서 개별 스레드를 동시에 실행하는 성질
* 스레드의 개수가 코어의 수보다 많을 경우, 스레드를 어떤 순서에 의해 동시성으로 실행할 것 인가를 결정해야 한다. 이것을 **스레드 스케줄링** 이라고 한다.

### 자바의 스레드 스케줄링 방식
1. 우선순위(**Priority**)방식
    * 우선순위가 높은 스레드가 실행 기회를 더 많이 갖는다.
    * 스레드 객체에 우선순위 번호를 부여할 수 있어 개발자가 코드를 제어할 수 있다.
    * 우선순위는 동시성에서만 의미가 있다고 볼 수 있다. 
2. 순환할당(**Round-Robin**)방식
    * 시간 할당량 `Time Slice`을 정해서 하나의 스레드를 정해진 시간만큼 실행한다.    

### 스레드 우선순위
* 우선순위는 1에서부터 10까지 부여. 1이 가장우선순위가 낮고 10이 가장 높음.
* `Thread` 클래스 상수로도 지정할 수 있다.
* 만약 쿼드 코어일 경우 4개의 스레드가 병렬성으로 실행 될 수 있기 때문에 4개 이하의 스레드를 실행할 경우 우선순위 방식이 크게 영향을 미치지 못함.
```java
thread.setPriority(Thread.MAX_PRIORITY); // 10
thread.setPriority(Thread.NORM_PRIORITY); // 5
thread.setPriority(Thread.MIN_PRIORITY); // 1
```

## 동기화 메소드와 동기화 블록
### 공유 객체를 사용할 때 주의할 점
* 멀티 스레드에서는 스레드들이 객체를 공유해서 작업해야 하는 경우가 있음.
* 스레드 A를 사용하던 객체가 스레드 B에 의해 상태가 변경될 수 있기 때문에 의도했던 것과 다른 결과를 산출 될 수 있다.

### synchronized
* **임계 영역(critical section)** : 멀티 스레드 프로그램에서 단 하나의 스레드만 실행할 수 있는 코드 영역
* 자바는 임계영역을 지정하기 위해 **동기화(synchronize)** 메소드와 블록을 제공.
* 스레드가 객체 내부의 동기화 메소드 또는 블록에 들어가면 즉시 객체에 잠금을 걸어 다른 스레드가 임계 영역 코드를 실행하지 못하도록 한다.
* **synchronized** 키워드는 인스턴스와 정적 메소드 어디든 붙일 수 있다.
```java
public synchronized void method() {
    // 임계 영역;
}
```
```java
public void method() {
    // 여러 스레드가 실행 가능한 영역
    ...
    synchronized(공유객체) { /*동기화 블록*/
        // 임계 영역
    }
    // 여러 스레드가 실행 가능한 영역
    ...
}
```

## 스레드 상태
* 스레드 객체를 생성하고 `start()` 메소드를 호출하면 실행 대기 상태가 된다.
* 실행 대기란 아직 스케줄링이 되지 않아 실행을 기다리고 있는 상태를 의미한다.
* 실행 대기 상태에 있는 스레드 중 스레드 스케줄링에 의해 CPU를 점유하고 `run()` 메소드를 실행한다.
* 일시 정지 상태 : **WAITING**, **TIMED_WAITING**, **BLOCKED**

### Thread.State 열거 상수
| 상태 | 열거 상수 | 설명 |
| ----------| -----------| -----------|
| 객체 생성 | NEW | 스레드 객체 생성. 아직 `start()` 메소드 호출 전.|
| 실행 대기 | RUNNABLE | 실행 상태로 언제든지 갈 수 있는 상태. |
| 일시 정지 | WAITING | 다른 스레드가 통지(Notify)할 때까지 기다리는 상태 |
| 일시 정지 | TIMED_WAITING | 주어진 시간 동안 기다리는 상태 |
| 일시 정지 | BLOCKED | 사용하고자 하는 객체의 락이 풀릴 때까지 기다리는 상태 |
| 종료 | TERMINATED | 실행을 마친 상태 |

## 스레드 상태 제어
* **스레드 상태 제어** : 실행 중인 스레드의 상태를 변경하는 것
* 멀티 스레드 환경에서는 정교한 스레드 상태 제어가 필요하며, 스레드의 상태 변화를 가져오는 메소드를 파악해야 한다.
