# [TIL & Issue Note] 20220304


## Important concepts

- 변수에 넣을 수 있는 것은 `Object`이고, 모든 `Object`는 `Class`의 인스턴스이다. numbers, functions, null 조차 객체이다. 모든 객체는 Object 클래스를 상속한다.
- Dart가 강타입 언어이긴하나, 타입을 추론 할 수 있기 때문에 명시는 선택사항이다.
- null-safety를 활성화 하면, 특별히 선언하지 않으면 변수에는 `null`을 포함할 수 없다.
    - nullable한 변수는 타입 뒤에 `?` 붙인다.
    - `int?`는 변수가 integer일 수도 있고 null일 수도 있다.
    - Dart에서 null을 평가하는 것을 동의하지 않는다면 `!`를 붙여 null이 아님을 명시할 수 있다.(?)
- 모든 타입을 허용하고 싶을 땐 `Object?` (null-safety 가 활성화) 또는 [Dynamic Type](https://dart.dev/guides/language/effective-dart/design#avoid-using-dynamic-unless-you-want-to-disable-static-checking)으로 런타임 시 타입을 체크할 수 있다.

- `List<int>`, `List<Object>`와 같은 제네릭 타입을 지원한다.
- `main()` 함수와 같은 최상위 함수, class 또는 object에 연결된 함수(각각 정적 및 인스턴스 메서드)를 지원한다. 또한 함수내에 함수 (중첩 혹은 지역 함수)를 만들 수 있다.
- 마찬가지로, Dart는 최상위 변수와 class 또는 객체에 연결된 변수(정적, 인스턴스 변수)를 지원한다. 인스턴스 변수는 `fields` 또는 `properties` 라고도 한다.
- 자바와는 달리 Dart는 접근 제어자가 없다. 식별자 이름이 `_`로 시작하면 `private` 나타낸다.
- 식별자는 `_`로 시작할 수 있고 그 뒤에 숫자와 문자를 조합해서 사용 가능하다.
- Dart는 런타임 값을 갖는 표현식`expressions`과 그렇지 않은 구문`statement` 두가지가 있다.
    - 조건부 표현식 `condition? expr1 : expr2` 는 `expr1` 또는 `expr2` 의 값을 갖는다.
    - `if-else 구문`은 값을 갖지 않는다.
    - 구문은 하나 혹은 많은 표현식을 포함하지만, 표현식은 구문을 직접적으로 포함할 수 없다.

- Dart Tool은 `warnings`와 `errors`를 보고 할 수 있다. 
    - `warnings` 코드가 작동하지 않을 수 있다는 표시
    - `errors`는 컴파일 타임 또는 런타임에 표시될 수 있다. 컴파일 타임시 오류는 코드가 실행되지 않는다. 런타임 오류는 코드가 실행되는 동안 예외가 발생한다.


## Keywords

### 예약어 목록
아래는 Dart언어가 처리하는 특별한 단어들이다.

- [Keywords](https://dart.dev/guides/language/language-tour#keywords)
- 키워드를 식별자로 사용을 피해야한다. 하지만 필요에 따라 윗첨자표시가 된 예약어는 식별자로 사용이 가능하다.
    - 윗첨자1은 문맥 키워드`contextual keywords`로, 특정 장소에만 의미가 있다. 어디에서든 식별자로 유효하다.
    - 윗첨자2는 빌트인 식별자`built-in identifiers`로, 이 키워드는 대부분에 곳에서 식별자로 유효하나 class나 타입이름, import 접두사로 사용할 수 없다.
    - 윗첨자3은 제한된 예악어로 [비동기 지원](https://dart.dev/guides/language/language-tour#asynchrony-support)과 연관이 있다. async, async*, 또는 sync*로 표기된 함수 본문안에서는 await나 yield가 식별자로 사용할 수 없다.




