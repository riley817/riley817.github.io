# [Dart] Dart 문법 정리


{{<admonition success "😊">}}
Dart 기본 문법의 헷갈리는 부분과 몰랐던 부분 정리! 🐸🐸
{{</admonition>}}

## `Final` 과 `const`
`final`과 `const` 키워드를 사용하여 선언하면 할당한 값을 변경할 수 없다. 둘다 상수를 선언할 때 사용하지만 차이점은 `const`는 `complie-time`에 상수를 설정하며 `final`은 `runtime`시에 결정되는 값도 상수로 설정할 수 있다.

```dart
// error 
const DateTime now1 = DateTime.now(); // error! const는 runtime시에 값이 결정되는 값은 설정할 수 없다.
 
final DateTime now2 = DateTime.now(); // final은 runtime시에 결정되는 값도 설정 가능하다.

```

{{<admonition note>}}
[Instance variables](https://dart.dev/guides/language/language-tour#instance-variables)는 `final` 일 수 있지만 `const`는 사용할 수 없음 
{{</admonition>}}

`const`는 **complie-time**에 사용할 상수를 선언한다. 만약 클래스 레벨에서 `const`를 사용할 때는 `static const`를 사용면 된다. const로 선언한 상수에 대한 산술연산 등의 결과 또한 const 상수여야 한다.

```dart
const bar = 1000000; // Unit of pressure (dynes/cm2)
const double atm = 1.01325 * bar; // Standard atmosphere
```

`const` 키워드는 상수 변수를 선언하는 것 뿐만 아니라 상수 값을 생성하는 생성자 선언에도 사용할 수 있다. 

```dart
var foo = const [];
final bar = const [];
const baz = []; // Equivalent to `const []`
```

`final` 키워드의 경우 개체는 수정할 수 없지만 해당 필드는 변경할 수 있다. 그러나 `const`의 경우 개체와 필드를 변경 할 수 없다.

```dart
final List<String> cats = ["개냥이", "물속성냥이"];
cats.add("냥냥펀치냥이");
print(cats);
  
const List<String> dogs = ["강형욱도 못 말리는 개", "강형욱도 무는 개"];
dogs[0] = "우리집개"; // error!

```

## Typedefs
`typedef`는 타입에 대한 별칭을 나타내는 키워드이며 타입을 참조할 수 있는 방법이다. 아래는 `IntList`라는 타입 별칭을 선언하고 사용하는 예제이다.
```dart
typedef IntList = List<int>;
IntList il = [1, 2, 3];
```

타입 별칭은 타입 파라미터와 함께 사용 할 수 있다.
```dart
typedef ListMapper<X> = Map<X, List<X>>;
ListMapper<String> m = {}; // Map<String, List<String>>
```

타입 별칭 대신 [inline function types](https://dart.dev/guides/language/effective-dart/design#prefer-inline-function-types-over-typedefs)을 사용하는 것을 권장한다고 한다.
```dart
typedef Compare<T> = int Function(T a, T b);

int sort(int a, int b) => a - b;

void main() {
  assert(sort is Compare<int>); // True!
}
```

## Asynchronous programming
### Future
`Future`는 비동기 작업의 결과를 나타내며 완료되지 않음 또는 완료됨의 두 가지 상태를 가질 수 있다.

- [Future<T>](https://api.dart.dev/stable/2.19.0/dart-async/Future-class.html)는 타입파라미터를 갖는 인스턴스를 생성한다.
- `Future`에서 리턴값이 없을 경우 타입은 `Future<void>` 이다.
- future의 상태 값은 `uncompleted` 또는 `completed` 두 가지 중 하나 이다.
- future 값을 반환하는 함수를 호출하면 수행할 작업을 대기열에 넣고 `uncompleted` 된 결과값을 반환한다.
- future 작업이 완료되면 future 혹은 오류와 함께 완료된다.

```dart
void main() {
  addNumbers(1, 1);
  addNumbers(2, 2);
}

void addNumbers(int number1, int number2) {
  print('계산시작 : $number1 + $number2');

  // 서버 시뮬레이션
  Future.delayed(Duration(seconds: 2), (){
    print('계산완료 : $number1 + $number2 = ${number1 + number2}');
  });
  
  print('함수 완료');  
}
```

> Console \
> 계산시작 : 1 + 1 \
> 함수 완료 \
> 계산시작 : 2 + 2 \
> 함수 완료 \
> 계산완료 : 1 + 1 = 2 \
> 계산완료 : 2 + 2 = 4 

#### Working with futures: async and await 
async 함수는 첫 번째 `await` 키워드까지 동기적으로 실행된다. async 함수 내에 첫번째 `await` 키워드 앞의 모든 코드는 즉시 실행된다.
- **async** : 함수 앞에 `async` 키워드를 사용하여 비동기 함수로 표현할 수 있다.
- **awiat** : `await` 키워드를 사용하여 비동기 함수의 완료된 결과를 가져올 수 있다. `await` 키워드는 `async` 함수 내에서만 동작한다.

```dart
void main() {
  addNumbers(1, 1);
  addNumbers(2, 2);
}

Future<void> addNumbers(int number1, int number2) async {
  print('계산시작 : $number1 + $number2');

  // 서버 시뮬레이션
  await Future.delayed(Duration(seconds: 2), (){
    print('계산완료 : $number1 + $number2 = ${number1 + number2}');
  });
  
  print('함수 완료');  
}
```
> Console \
> 계산시작 : 1 + 1 \
> 계산시작 : 2 + 2 \
> 계산완료 : 1 + 1 = 2 \
> 함수 완료 \
> 계산완료 : 2 + 2 = 4 \
> 함수 완료

### Stream
Dart에서는 비동기 프로그래밍을 위한 [Future](https://api.dart.dev/stable/2.19.0/dart-async/Future-class.html)와 [Stream](https://api.dart.dev/stable/2.19.0/dart-async/Stream-class.html) 두 가지 클래스를 제공한다.

- stream은 비동기적으로 연속적인 데이터를 제공한다.
- `Stream API`에서 `await for` 또는 `listen()`을 사용하여 스트림을 처리 할 수 있다.
- 스트림에는 single subscription 과 broadcast 두가지 종류를 제공한다.

```dart
import 'dart:async';

void main() {
  final controller = StreamController();
  
  final stream = controller.stream.asBroadcastStream();
  
  // 짝수만 출력하기
  final streamListener1 = stream.where((val) => val % 2 == 0).listen((val) {
    print('Listener 1 : $val');
  });
  
  // 홀수만 출력하기
  final streamListener2 = stream.where((val) => val % 2 == 1).listen((val){
    print('Listener 2 : $val');
  });
  
  controller.sink.add(1);
  controller.sink.add(2);
  controller.sink.add(3);
  controller.sink.add(4);
  controller.sink.add(5);
}
```

> Console \
> Listener 2 : 1 \
> Listener 1 : 2 \
> Listener 2 : 3 \
> Listener 1 : 4 \
> Listener 2 : 5

```dart
import 'dart:async';

void main() {
  calculate(2).listen((val) {
    print('calculate(2) : $val');
  });
  calculate(4).listen((val) {
    print('calculate(4) : $val');
  });
}

Stream<int> calculate(int number) async* {
  for(int i = 0; i < 5; i++) {
    yield i * number;   
    await Future.delayed(Duration(seconds: 1));
  }
}
```

> Console \
> calculate(2) : 0 \
> calculate(4) : 0 \
> calculate(2) : 2 \
> calculate(4) : 4 \
> calculate(2) : 4 \
> calculate(4) : 8 \
> calculate(2) : 6 \
> calculate(4) : 12 \
> calculate(2) : 8 \
> calculate(4) : 16

## 참고
- [코드팩토리 :: Dart 언어 4시간만에 완전정복 강의](https://www.inflearn.com/course/dart-%EC%96%B8%EC%96%B4-%EC%9E%85%EB%AC%B8/dashboard)
- [Dart Document :: Asynchronous programming: futures, async, await](https://dart.dev/codelabs/async-await)
- [Dart Document :: Asynchronous programming: Streams](https://dart.dev/tutorials/language/streams)
