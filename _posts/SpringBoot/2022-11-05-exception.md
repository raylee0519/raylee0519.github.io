---
layout: single
title: "예외 및 에러 처리 방법"

categories:
 - Springboot
---
## Spring Boot 예외, 에러 처리

스프링 부트에서 exception 을 처리하는 방법을 알아보자. <br>
스프링부트에서 에러가 발생할 경우 보통 Whie Label페이지를 보여준다. 이는 원인을 파악하는데 충분하지 않을 수 있다. <br>


예외처리만 담당하는 패키지를 만들어서 아래 순서에 맞는 클래스를 작성해두자. 여기서 각각 다른 표현의 3가지 클래스를 만들어보겠다. <br> <br>

### enum(열거형) 클래스 작성
예상되는 발생할 에러코드를 정리한 enum 클래스를 작성한다. <br>
예시로 ErrorCode라는 이름의 enum 클래스를 작성하였다. <br>

```java
import lombok.AllArgsConstructor;
import lombok.Getter;

@AllArgsConstructor
@Getter
public enum ErrorCode {
    NOT_FOUND(404,"COMMON-ERR-404","페이지가 작동하지 않네요."),
    INTER_SERVER_ERROR(500,"COMMON-ERR-500","해당 ID는 존재하지 않습니다."),
    ;

    private int status;
    private String errorCode;
    private String message;
}
```

### exception 발생시 응답하는 에러 정보 클래스 작성
어디 부분에서 예외가 발생했는지 표현해주는 클래스이다. <br>
ErrorResponse 라는 이름의 클래스로 작성하였다. <br>

```java
import lombok.Getter;
import lombok.Setter;

@Getter
@Setter
public class ErrorResponse {
    private int status;
    private String message;
    private String code;

    public ErrorResponse(ErrorCode errorCode){
        this.status = errorCode.getStatus();
        this.message = errorCode.getMessage();
        this.code = errorCode.getErrorCode();
    }
}
```

### exception 발생시 전역으로 처리할 exception handler 작성
전역으로 관리할 클래스이다 <br>
GlobalExceptionHandler이라는 클래스를 생성하였다. <br> 
```java
import lombok.extern.slf4j.Slf4j;
import org.springframework.http.HttpStatus;
import org.springframework.http.ResponseEntity;
import org.springframework.web.bind.annotation.ExceptionHandler;
import org.springframework.web.bind.annotation.RestControllerAdvice;

@Slf4j
@RestControllerAdvice
public class GlobalExceptionHandler {


    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleException(Exception ex){
        log.error("handleException",ex);
        ErrorResponse response = new ErrorResponse(ErrorCode.INTER_SERVER_ERROR);
        return new ResponseEntity<>(response, HttpStatus.INTERNAL_SERVER_ERROR);
    }
}
```
이 세 가지 방법으로 스프링부트에서 예외나 에러를 처리할 수 있다. <br>
여기서 Slf4j라는 것을 import하여 사용하고 있는데 이 부분은 따로 정리할 예정이다. <br>
