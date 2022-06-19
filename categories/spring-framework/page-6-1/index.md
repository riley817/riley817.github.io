# [스프링 5.0 마스터] 6.1 예외처리


_스프링 5.0 마스터 스터디 학습 내용 정리입니다._

### 1. 스프링 부트의 기본 예외처리
+ 스프링부트의 기본 예외 형식은 throw 된 예외 메세지 와 함께 **JSON 형태로 에러를 리턴**한다.
+ 브라우저경우 **기본 오류 페이지 (Whilelabel Error Page)** 를 출력한다.

```json
{
    "timestamp": "2018-12-25T05:01:26.483+0000",
    "status": 500,
    "error": "Internal Server Error",
    "message": "Some Exception Occurred",
    "path": "/users/dummy-service"
}
```
### 2. 스프링 사용자 정의 예외처리
스프링에서는 오류에 대한 응답을 사용자가 정의하는 여러 옵션을 제공한다.

#### 2.1 응답 메세지 사용자 정의 하기
+ 주어진 ID 가 가지고 있는 todo 가 발견되지 않았을 때 발생시킬 사용자 정의 **TodoNotFoundException** 와 메세지 처리를 할 **ExceptionResponse** 객체를 생성한다.

##### TodoNotFoundException.java
```java
package com.mastering.spring.springboot.exception;

public class TodoNotFoundException extends RuntimeException {
    public TodoNotFoundException(String msg) {
        super(msg);
    }
}
```

##### ExceptionResponse.java
```java
package com.mastering.spring.springboot.bean;

import java.util.Date;

public class ExceptionResponse {

    private Date timestamp = new Date();
    private String message;
    private String details;

    public ExceptionResponse(String message, String details) {
        super();
        this.message = message;
        this.details = details;
    }

    public Date getTimestamp() {
        return timestamp;
    }

    public void setTimestamp(Date timestamp) {
        this.timestamp = timestamp;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }

    public String getDetails() {
        return details;
    }

    public void setDetails(String details) {
        this.details = details;
    }
}
```

+ **TodoNotFoundException** 이 발생하면 **ExceptionResponse** Bean 을 사용해 응답을 반환할 컨트롤러를 생성한다.

```java
@ControllerAdvice
@RestController
public class RestResponseEntityExceptionHandler extends ResponseEntityExceptionHandler {
    @ExceptionHandler(TodoNotFoundException.class)
    public final ResponseEntity<ExceptionResponse> todoNotFound(TodoNotFoundException ex) {
        ExceptionResponse exceptionResponse = new ExceptionResponse(ex.getMessage(), "Any details you would want to add.");
        return new ResponseEntity<ExceptionResponse>(exceptionResponse, new HttpHeaders(), HttpStatus.NOT_FOUND);
    }
}
```

#### 2.2 모든 예외에 사용자 정의 예외처리 정의하기
아래 코드는 사용자 정의 예외 이외에 모든 예외에 대해 사용자 정의 예외 메세지를 응답하도록 설정한다.

```java
@ExceptionHandler(Exception.class)
public ResponseEntity<ExceptionResponse> defaultError(Exception ex) {
    ExceptionResponse exceptionResponse = new ExceptionResponse(ex.getMessage(), "알수없는 에러...");
    return new ResponseEntity<ExceptionResponse>(exceptionResponse, new HttpHeaders(), HttpStatus.INTERNAL_SERVER_ERROR);
}
```

+ **@ControllerAdvice** 를 이용하여 **@ExceptionHandler** 를 모든 패키지 및 컨트롤러에서 전역적으로 사용할 수 있도록 정의한다. 
+ **@ExceptionHandler(TodoNotFoundException.class)** 가 특정 에러 유형(클래스) 에 대해 예외처리 하도록 정의한다. 사용자 예외 처리가 되어 있지 않은 다른 예외는 스프링부트의 기본 예외처리 형태를 따른다.
+ **@RestController**을 사용하여 클라이언트에게 응답처리를 할 수 있게 한다.

#### 2.3 사용자 정의 에러에 특정 HTTP 응답 상태 지정하기
**@ResponseStatus** 어노테이션을 사용하여 커스텀 에러에 특정 HTTP 응답 상태를 지정할 수 있다.

```java
@ResponseStatus(HttpStatus.NOT_FOUND)
public class TodoNotFoundException extends RuntimeException {
    public TodoNotFoundException(String msg) {
        super(msg);
    }
}
```

### 3. HTTP 응답 상태 코드
+ 상세 응답코드 정의 : https://developer.mozilla.org/ko/docs/Web/HTTP/Status

| 응답 상태 | 상황 |
| ----------| -----------|
| 400 - BAD REQUEST | 요청본문이 API 스펙을 충족하지 못하여 서버가 요청을 이해할 수 없음을 의미. |
| 401 - UNAUTHORIZED  | 인증 또는 권한 부여 실패 |
| 403 - RESOURCE FORBIDDEN  | 클라이언트가 컨텐츠에 접근할 권리를 갖고 있지 않음. 401 과 다른 점은 클라이언트가 누구인지 알고 있음. |
| 404 - RESOURCE NOT FOUND | 요청한 리소스가 존재하지 않음 |
| 405 - METHOD NOT ALLOWED | 지원되지 않는 오퍼레이션 (ex : GET 요청한 허용되는 리소스에 POST 요청을 시도) |
| 500 - INTERNAL SERVER ERROR | 서버에서 에러가 발생하여, 소비자는 이 문제를 해결 할 수 없음. |

### 4. ResponseStatusException (Spring 5 and Above)

+ https://www.baeldung.com/exception-handling-for-rest-with-spring

