# [buf] buf로 protobuf 사용하기



## buf
`buf`는 `IDL(Interface description language)` 중 하나인 `protobuf`를 사용하기 쉽게 여러가지 기능을 제공하고 있다. `protobuf`를 사용하면서 발생하는 문제는 다음과 같다.

- API 설계가 일관성이 없다.
- 의존성 관리가 중앙에서 이루어지지 않고 있다. 
- `protobuf`의 버전관리가 잘 이루어지지 않는다.
- `protoc`를 통한 `stub` 배포 및 관리가 어렵다
- `mock` 서버 생성, `fuzz testing`, 문서화 할 수 있는 툴들이 많이 존재하지 않는다.

## buf에서 제공하는 툴

### `buf cli`
- 발전된 protobuf 컴파일러를 제공한다.
- 좋은 API 디자인과 구조를 lint 기능을 통해 선택할 수 있다.
- 소스 코드 및 wire 레벨에서 변경 감지를 통해 호환성 적용이 가능하다.
- 구성 파일을 통해 protoc 의존 관리를 쉽게 할 수 있다.

### Buf Schema Registry (BSR)
BSR은 Protobuf API를 buf 에서 제공하는 SaaS 플랫폼을 통해 호스팅하여 관리할 수 있다. BSR를 통해 중앙에서 Protobuf API에 대한 호환성 및 의존성 관리가 가능해진다. Javascript의 `npm`, 파이썬의 `pip`이나 `cargo` 패키지 관리 툴처럼 Protobuf API의 종속성 관리를 BSR을 통해 관리할 수가 있다.

## buf 설치하기
[buf 설치하기](https://docs.buf.build/installation)
```bash
# mac
brew install bufbuild/buf/buf
```

## BSR 로그인하기
[사용자 설정](https://buf.build/settings/user) 페이지에서 저장소에 접근할 수 있는 개인 토큰을 발행하고 레지스트리에 로그인한다. 로그인을 완료하면 `$HOME/.netrc` 파일에 자격증명이 생성된다.

```bash
buf registry login

Log in with your Buf Schema Registry username. If you don't have a username, create one at https://buf.build.

Username: riley
Token:

Credentials saved to /Users/riley/.netrc.
```


