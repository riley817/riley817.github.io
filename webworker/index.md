# Multi Thread


# Web worker
`Web Worker` 는 메인 스레드와 분리된 별도의 백그라운드 스레드에서 스크립트를 실행할 수 있는 기술이다. UI 스레드와는 별개의 스레드를 실행시켜 사용자 인터페이스를 방해하지않고 작업을 수행할 수 있다.

## Web worker scope
- Web worker는 메인 스레드와 별도의 worker 스레드를 갖는다.
- 메인 스레드에서 `window` 객체는 `GlobalScope` 이지만 worker 스레드에서는 별도의 스코프 범위를 갖는다.
    - [Worker 종류에 따른 Scope](https://developer.mozilla.org/en-US/docs/Web/API/Web_Workers_API#worker_global_contexts_and_functions)
- worker 스레드는 메인 스레드 `window` 객체에 액세스할 수 있는 권한이 없기 때문에 window의 메서드나 DOM을 직접적으로 제어할 수 없다. `window` 의 DOM에 접근하기 위해서는 `postMessage()` 메서드를 이용하여 메세지를 보내고 `onmessage` 이벤트 핸들러를 통해 제어할 수 있다.

## Worker 종류
- `Dedicated workers` : 단일 스크립트 환경을 이용하는 worker. 처음 worker를 생성한 context 영역에서만 사용이 가능하다.
- `Shared workers` :  worker가 같은 도메인 내에 존재하는 모든 Thread에서 사용이 가능하다. 다른 컨텍스트 (iframe, 다른 탭 등)에서도 접근이 가능하다. script에서 worker와 `port` 를 통해서 통신한다.
- `Service workers` : 브라우저가 백그라운드 영역에서 실행하는 스크립트로 Service worker의 생명주기는 웹페이지와는 별개로 작동한다.  웹페이지 또는 사용자 인터랙션이 필요하지 않는 기능을 제공하고 있다. 오프라인 환경, 푸시 알림, 백그라운드 동기화 등의 기능을 지원한다.

## Service Worker
- Web worker의 한 종류이다. 웹 페이지의 라이프 사이클과는 별개로 동작하기 때문에 웹페이지가 닫혀도 자동으로 비활성화 되지 않는다.
- 웹 애플리케이션, 브라우저 및 네트워크 사이에 proxy 서버 역할을 한다. 주로 오프라인, 푸시 알림 및 백그라운드 동기화 API를 위해 사용한다.
- 보안상의 이유로 localhost 이외에는 HTTPS 통신을 해야한다.
- `multi-threading` 의 기능을 사용하기 위해 설계된 것이 아니므로 몇가지 기능들이 제한되어 있다. ~~`EventSource` 나`WebSocket` 기능도 제한되어 있다.~~
