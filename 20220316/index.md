# [TIL & Issue Note] 20220316


# Getting Started

## What is React ?
- 사용자 인터페이스를 구축하기 위한 자바스크립트 라이브러리
- 모바일 앱처럼 상호작용과 반응성이 높은 사용자 환경을 제공

### 모바일 앱
- 화면 전환이 빠르고 사용자 반응성이 높음

### 웹
- 전통적인 웹사이트의 전환은 1) 링크,버튼 등 요청 2) 서버에서는 응답을 받고 새로운 HTML을 그림
- 서버와 요청과 응답 작업으로 인해 상호작용이 투박함
- 서버에서 처리한 HTML이 로드되는 것을 기다려야 함

### 자바스크립트
- 사용자가 보는 것을 조작할 수 있는 프로그래밍 언어
- DOM에 접근하고 조작할 수 있다.
- HTML 페이지를 fetch하지 않아도 사용자가 보는 것을 바꿀 수 있다.


## 왜 React를 사용해야 하는가?
### Javascript
- 모든 단계를 일일이 지정해야 한다.
- 명령형 접근

### React
- 사용자 지정 HTML 구성요소(컴포넌트)
- 애플리케이션은 작은 블록 단위 즉 컴포넌트로 단위로 분해하여 관리함.
- 높은 수준의 Syntax 제공 

## Building Single-Page-Application (SPAs)

## React Alternative

### React
- 리액트의 경우 라이브러리
- 특정 기능이 필요하면 서드 파티 라이브러리를 추가해야 함

### Angular
- 프론트엔드 프레임워크
- 리액트 보다 더 많은 기능을 가지고 있음
- 타입스크립트 내장

### Vue.js
- Angular, React를 조금씩 섞어놓은 버전
- 라우팅 등 일부 기능 제공

# JavaScript Refresher

## let & const
- 둘 다 변수를 선언할 때 사용
- 이전에는 `var` 키워드를 사용하여 선언함
- `let` : 가변변수 선언시 사용
- `const` : 처음 값을 할당한 후 바꾸지 않을 경우 사용. 상수 선언시
- 주로 `const`를 위주로 사용함

## Arrow Function
함수 선언 방법
```javascript
function myFunc() {
    // ...
}
```

함수 선언 방법 - Arrow Function
```javascript
const myFunc = () => {
    // ...
}
```
- Arrow Function은 `this`가 들어가서 발생하는 문제를 해결한다.
- `this`가 항상 원하는 객체를 참조 하지 않는 문제를 해결


