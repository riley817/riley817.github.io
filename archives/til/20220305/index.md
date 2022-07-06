# [TIL & Issue Note] 20220305


## 변수
변수를 만들고 초기화 하는 코드는 다음과 같다.

```js
// 변수를 생성하고 초기화 한다.
var name = 'Bob';
```
변수는 참조를 저장한다. name으로 불리는 변수는 "Bob"이라는 문자열을 갖는 **String 객체의 참조**를 포함한다.

name의 변수 타입은 **String**으로 유추할 수 있지만 해당 유형을 명시적으로 변경할 수 있다. 객체가 단일 타입으로 제한되지 않은 경우 `Object` 타입(필요하면 dynamic)으로 지정할 수 있다.

```js
Object name = 'Bob';
```
유추가능한 명시적인 타입으로 지정할 수도 있다.

```js
String name = 'Bob';
```

### 기본값
- `nullable` 타입의 초기값은 `null`이다. (null-safety를 선택하지 않으면 모든 변수는 nullable 타입)
- 숫자 타입도 초기값은 null이다. (숫자 타입도 다트에서는 Object이기 때문)

```js
int? lineCount;
assert(lineCount == null); 
```

null 안전을 활성화 한 경우 null을 허용하지 않는 변수 값을 사용하기 전에 초기화해야 한다.

```js
int lineCount = 0;
```
선언된 로컬 변수를 초기화 할 필요는 없지만 사용하기 전에 값을 할당해야 한다. 다음 코드는 Dart가 `print()`를 전달할 때 null이 아닌 `lineCount`를 감지하기 때문에 유효하다.

```js
int lineCount;

if (weLikeToCount) {
    lineCount = countLines();
} else {
    lineCount = 0;
}

print(lineCount);

```
최상위 및 클래스 변수는 느리게 초기화 되며, 초기화 코드는 변수가 처음 사용될 때 실행된다.

### Late 변수
다트 2.12에서 `late` 변수가 추가되었고 아래 두 가지 경우 사용한다.

1. non-nullable 변수를 선언할 때 초기화하지 않고 선언 후에 초기화 할 때
2. 게으른 초기화`lazily initializing`를 위해서

다트의 제어 흐름 분석은 non-nullable 변수가 사용하기 전에 null이 아닌 값으로 설정된 경우를 탐지할 수 있지만 가끔 분석에 실패한다. 실패하는 두가지 일반적인 경우는 최상위 변수와 인스턴스 변수이다. 다트에서는 변수 값의 설정 여부를 확인할 수 없기 때문에 에러가 발생한다.


변수를 사용하기 전에 값을 설정했지만 다트에서 동의하지 않는 경우 변수에 `late` 지시자를 통해 다음과 같이 오류를 수정할 수 있다.
```js
late String description;

void main() {
  description = 'Feijoada!';
  print(description);
}
```

변수에 `late`를 선언하면 변수가 처음 사용될 때 초기화가 실행된다. 이렇게 게으른 초기화는 다음과 같은 경우에 유용하다.

- 변수가 필요하지 않을 수도 있으며 초기화하는데는 많은 비용이 든다.
- 인스턴스 변수를 초기화 하거나 또는 인스턴스 변수가 this에 접근해야 할때

아래 예제에서 `temperature` 변수는 사용되지 않을 수 있다. 아래처럼 선언하면 `temperature`라는 변수가 사용될 때 초기화 된다.
```js
// This is the program's only call to _readThermometer().
late String temperature = _readThermometer(); // Lazily initialized.
```

### Final과 const
변수를 변경할 생각이 없는 경우 `var` 대신 `final`과 `const`를 사용한다.  
- final 변수는 한번만 값을 설정 할 수 있다.
- const 변수는 컴파일 타임 상수로 컴파일 하는 동안에 값이 정해진다. const 변수는 암시적으로 final 이다.(?)

아래는 `final` 변수의 예제이다. `final` 변수는 값을 변경 할 수 없다.
```js
final name = 'Bob'; // Without a type annotation
final String nickname = 'Bobby'; 

name = 'Alice'; // Error: a final variable can only be set once.
```

`const` 변수는 **컴파일 타임 상수(compile-time constants)** 이다. 컴파일 시에 값이 정해진다. const 변수가 클래스 레벨에 있는 경우 `static const` 라고 한다. 변수를 선언하는 곳에서 숫자나 문자열 리터럴과 같은 상수 값이나 산술 연산에 대한 결과 값을 설정한다.

```js
const bar = 1000000; // Unit of pressure (dynes/cm2)
const double atm = 1.01325 * bar; // Standard atmosphere
```

`const` 키워드는 상수 변수를 선언하고 작성할 뿐만 아니라 상수 값을 생성하는 생성자를 선언하는 에도 사용할 수 있다. 또한 모든 변수는 상수 값을 가질 수 있다.

```js
var foo = const [];
final bar = const [];
const baz = []; // `const []` 동등
```
위의 예제에서 `baz`와 같이 `const` 선언 초기화 표현식에 `const`를 생략할 수 있다. 자세한 내용은 [DON'T use const redundantly](https://dart.dev/guides/language/effective-dart/usage#dont-use-const-redundantly) 참고한다. 

non-final, non-const 변수의 값이라도 const 값을 사용하는 동안 값을 변경 할 수 있다.
```js
foo = [1, 2, 3]; // Was const []
```

아래 const 변수는 값을 바꿀 수 없다.
```js
baz = [42]; // Error: Constant variables can't be assigned a value.
```

{{< figure src="/categories/images/dart-flutter/const-variables.png#center" >}}

[타입 검사와 캐스트](https://dart.dev/guides/language/language-tour#type-test-operators) (`is`와 `as`)와 [collection if](https://dart.dev/guides/language/language-tour#collection-operators), [스프레드 연산자](https://dart.dev/guides/language/language-tour#spread-operator)(... and ...?)를 사용하여 상수를 정의할 수 있다.

```js
const Object i = 3; // Where i is a const Object with an int value...
const list = [i as int]; // Use a typecast.
const map = { if (i is int) i: 'int' } // Use is and collection if.
const set = { if (list is List<int>) ...list} // ...and a spread.
```

### Built-in types
다트 언어의 내장형 타입이다.
- [Numbers](https://dart.dev/guides/language/language-tour#numbers) (`int`, `double`)
- [Strings](https://dart.dev/guides/language/language-tour#strings) (`String`)
- [Booleans](https://dart.dev/guides/language/language-tour#booleans) (`bool`)
- [Lists](https://dart.dev/guides/language/language-tour#lists) (`List`, also known as *arrays*)
- [Sets](https://dart.dev/guides/language/language-tour#sets) (`Set`)
- [Maps](https://dart.dev/guides/language/language-tour#maps) (`Map`)
- [Runes](https://dart.dev/guides/language/language-tour#characters) (`Runes`; often replaced by the `characters` API)
- [Symbols](https://dart.dev/guides/language/language-tour#symbols) (`Symbol`)
- The value `null` (`Null`)

이러한 내장타입은 리터럴을 사용하여 Object로 만들 수 있는 기능을 포함한다. 예를들어 `this is a string` 은 문자열 리터럴이며 `true`는 boolean 리터럴이다.

다트의 모든 변수는 Object를 참조하기 때문에 생성자를 사용하여 변수를 초기화 할 수 있다. 내장형 타입 중 일부는 자체 생성자가 있다. 예를들면, `Map()` 생성자를 사용하여 Map 타입을 작성할 수 있다.

일부 타입을 또한 다트 언어에서 특별한 역할을 가지고 있다.

* `Object`: `Null`을 제외한 모든 다트 클래스
* `Future` and `Stream`: [비동기 지원](https://dart.dev/guides/language/language-tour#asynchrony-support)
* `Iterable`: [for-in loops](https://dart.dev/guides/libraries/library-tour#iteration) and
  in synchronous [generator functions](https://dart.dev/guides/language/language-tour#asynchrony-support#generator).
* `Never`: 표현식이 성공적으로 계산을 마칠 수 없음을 나타낸다. 항상 예외를 발생시키는 함수에 가장 많이 사용
* `dynamic`: 정적 검사를 실행 중지함을 나타낸다. 일반적으로 `Object` 또는 `Object?` 대신 사용한다.
* `void`: 값이 사용되지 않음을 나타낸다. 반환 유형으로 자주 사용된다.

