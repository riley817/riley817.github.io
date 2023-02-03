# [Flutter Beginner] StatefulWidget


## Widget
`Widget`은 모두 불변의 법칙을 가지고 있다. 그러나 위젯의 값을 변경해야 할 경우가 생긴다. 변경이 필요할 경우 기존 위젯을 삭제해버리고 완전 새로운 위젯으로 대체한다.

### StatelessWidget Life Cycle
{{<image src="/posts/images/dart-flutter/statelesswidget.png">}}

- `Constructor`로 생성이되고 생성이 되자마자 build 함수가 실행된다.
- 변경이 필요하면 새로운 위젯을 만든다.
- **StatelessWidget은 라이프 사이클 동안 단 한번만 build 함수를 실행한다.**

#### StatefulWidget 생명주기
{{<image src="/posts/images/dart-flutter/statefulWidget_life_cycle.jpg">}}

1. `Construct` 
2. `createState` : State를 생성
3. `initState` : State를 초기화. State가 생성될 때 단 한번만 호출된다.
4. `didChangeDependencies`
5. `dirty` 상태 : 변경이 필요한 상태를 의미한다.
6. `build` : 위젯을 화면에 그려준다.
7. `clean` : `didUpdateWidget`, `setState` 
8. `deactivate` : 거의 사용하지 않음
9. `dispose` 
  
#### 파라미터가 변경되었을 때 생명주기
{{<image src="/posts/images/dart-flutter/statefulWidget_change_parameter.jpg">}}

1. 기존 위젯은 삭제된다. 새로운 `StatefulWidget`이 생성되고 `Constructor`가 실행
2. 기존 `State` 찾는다.
3. `didUpdateWidget` : `clean` 상태인 상태에서 실행된다.
4. `dirty` 상태로 변경한다.
5. 변경된 값을 기반으로 `build` 실행
6. `clean` 상태로 변경된다.

#### **setState**가 실행될 때 생명주기
{{<image src="/posts/images/dart-flutter/statefulWidget_lifecycle_setState.jpg">}}

1. clean인 상태에서 `setState`를 실행한다. 
2. dirty 상태로 변경되고 
3. `build`가 재실행된다.
4. clean 상태로 다시 변경



