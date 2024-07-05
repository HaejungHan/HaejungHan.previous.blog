---
layout: post
title:  "TIL(20240617) [Service Integration Test, GlobalExceptionHandler]"
date:  2024-06-17
categories: TIL Spring 코딩테스트
---



---------------------------------------------------------------------



# 📌 Spring

## 💡 Service 통합테스트 


### 🚩 통합테스트와 단위테스트의 차이점
- 통합테스트는 모듈을 통합하는 과정에서 모듈간의 호환성을 확인하기 위한 테스트이며, 단위테스트는 하나의 모듈(하나의 기능)을 독립적으로 진행하여 확인하는 테스트 이다. (모듈: 애플리케이션에서 작동하는 기능 또는 메서드를 의미함)

### 🚩 통합테스트와 단위테스트의 장/단점

단위테스트와 통합테스트 코드를 작성하면서 개인적인
견해가 담긴 장/단점 입니다. 

- 단위테스트의 장점: 테스트 시간 절감/ 자신이 작성한 코드에 대한 확인검증 가능/ 리팩토링 시 코드의 안정성 향상/ 개별적인 코드 단위라 테스트시 디버깅과 문제해결이 쉬움
단위테스트의 단점: 가짜객체(Mock)를 사용하기 때문에 실제 운영환경에서 제대로 동작할 수 있을지 의문이 생김.
통합테스트 장점: springbootContainer을 직접띄워서 테스트하기 때문에 운영환경과 가장 유사한 테스트를 할 수 있다.
통합테스트 단점:  장점으로 인해서 모든 Bean을 로드하기 때문에 시간이 오래걸리고 단위테스트에 비해 무거움, 테스트 단위가 크기때문에 어디서 문제가 발생했는지 찾기가 어려움

### 💡 레이어별로 나누어서 Slice Test 를 하는 이유는?

- 슬라이스 테스트는 톰캣을 실행하지 않고 대상이 되는 계층에서 필요한 bean들만 등록해 테스트하기 때문에 실행속도가 빠르며, 특정계층의 기능에 집중해 그 기능에 대해 세밀하게 파악할 수 있기 때문이다.

[스프링 테스트 어노테이션](https://hstory0208.tistory.com/entry/Spring-%EC%8A%A4%ED%94%84%EB%A7%81-%ED%85%8C%EC%8A%A4%ED%8A%B8-%EC%96%B4%EB%85%B8%ED%85%8C%EC%9D%B4%EC%85%98-%EC%95%8C%EC%95%84%EB%B3%B4%EA%B8%B0-feat-%EC%8A%AC%EB%9D%BC%EC%9D%B4%EC%8A%A4-%ED%85%8C%EC%8A%A4%ED%8A%B8)

### 💡 테스트 코드를 직접 짜보고 나서 느낀 테스트 필요성
- 장단점에서 작성했던 것 처럼 단위테스트의 경우에는 "이게 실제환경에서 작동할까..?"라는 의문이 가장 많이 들었고, Service단위테스트를 작성한 후 통합테스트를 작성하니 단위,통합테스트는 꼭 필요한 테스트코드 작성이라고 생각했다. 그리고 무엇보다 테스트코드를 작성하면서 핵심기능들의 코드에 대해서 유심히 살펴보고 리팩토링을 하는 부분들이 생겼기에 이전보다 코드가 더 깔끔해졌다라는 걸 느낄 수 있었다. 그리고 어플리케이션의 전체적인 흐름을 깊이 알 수 있는 계기가 되어 너무 좋았다.

<br>


---------------------------------------------------------------

# 📌 GlobalExceptionHandler

과제와 프로젝트를 하면서 가장 아쉬웠던 부분이 예외처리를 잘 하지 못했던 부분들이다.
응답 메세지가 중구난방이거나, 오류가 발생했을 경우 어떤 부분에서 정확히 잘못되었는지에 대한 확인 어려웠다. 팀 프로젝트에 들어가기 앞서서 응답메세지를 통일시키고, 어떤 부분에서 잘못되었는지, Validation까지도 잘 처리해보고 싶다는 마음이 생겨서 GlobalExceptionHandler대해 알아보고 내가 진행했던 개인프로젝트에 리팩토링 해보고자 한다. 

## 전역 에러 핸들러(GlobalExceptionHandler)란?

- @ControllerAdvice 와 @RestControllerAdvice, @ExceptionHandler 애너테이션 기반으로 Controller 내에서 발생하는 에러에 대해 해당 핸들러에서 캐치하여 오류를 발생시키지 않고 응답메세지로 클라이언트에게 전달해주는 기능을 말한다.

1) @ControllerAdvice : @Controller로 선언한 지점에서 발생한 에러를 도중에 
@ControllerAdvice로 선언한 클래스 내에서 이를 잡아 Controller내에서 발생할 수 있는 에러를 처리할 수 있도록 하는 애너테이션이다. 
2) @ContorllerAdvice는 Spring AOP를 이용한 애너테이션이다.


```

AOP에 대해서 이전에도 개념을 정리했었지만 계속 정리하다보면 내 것이 될거라고 생각하고 또 해보자!

*Spring AOP : 관점 지향 프로그밍

- 어떤 로직을 기준으로 핵심적인 관점(종단관심사)과 부가적인 관점(횡단관심사)로 나누어서 그 관점을 모듈화하는 것이다. 
- 즉, 핵심기능과 부가기능을 분리시키고 부가기능을 모듈화 하는 것

- 장점? 코드의 가독성 🔼 중복 🔽 유지보수 🔼


```

## @ControllerAdvice와 @RestControllerAdvice의 차이점?
- 이건 마치 애너테이션만 봐도 미리 예측할 수 있듯이, 응답을 JSON형태로 제공해준다는 차이점이다.

```

@Controller 
-> View 반환

@RestController
-> JSON형태 반환

```

![alt text](image-10.png)

요렇게 디렉토리를 한번 만들어보았다.

```java

@Getter
@AllArgsConstructor
public enum ErrorCode {

  // Http status 200 OK 
  SUCCESS(200, "OK", "요청에 성공하였습니다."),

  // Http status 400 BAD_REQUEST 
  INVALID_INPUT_VALUE(400, "BAD_REQUEST", "입력값이 올바르지 않습니다."),
  BAD_REQUEST(400, "BAD_REQUEST","잘못된 요청입니다."),

  // Http status 401 UNAUTHORIZED : 인증되지 않은 사용자 
  UNAUTHENTICATED_USERS(401, "UNAUTHORIZED","인증이 필요합니다."),

  // Http status 403 FORBIDDEN : 접근권한 없음 */
  ACCESS_DENIED(403, "FORBIDDEN","접근이 거부되었습니다."),

  // Http status 404 NOT_FOUND : Resource 를 찾을 수 없음 
  POST_NOT_FOUND(404, "NOT_FOUND","해당 게시물을 찾을 수 없습니다."),
  COMMENT_NOT_FOUND(404, "NOT_FOUND","해당 댓글을 찾을 수 없습니다."),
  RESOURCE_NOT_FOUND(404, "NOT_FOUND","해당 정보를 찾을 수 없습니다."),

  // Http status 405 METHOD_NOT_ALLOWED : 지원하지 않는 HTTP Method 
  METHOD_NOT_ALLOWED(405, "METHOD_NOT_ALLOWED","허용되지 않은 요청입니다."),

  // Http status 409 CONFLICT : 데이터 중복 
  DUPLICATE_RESOURCE(409, "CONFLICT","데이터가 이미 존재합니다"),

  // Http status 500 INTERNAL_SERVER_ERROR 
  SERVER_ERROR(500, "INTERNAL_SERVER_ERROR", "예기치 못한 오류가 발생하였습니다.");

  private int status;
  private final String code;
  private final String message;

}


```

에러코드와 에러메세지를 편리하게 관리하기 위해 Enum클래스를 만들었다. 


```java

@Getter
@AllArgsConstructor
public class CustomException extends RuntimeException {

  private final ErrorCode errorCode;

}

```

CustomException 클래스는 RuntimeException을 상속받아 uncheckedeException으로 활용하고 비지니스로직에서 실패할 경우 CustomException 예외를 발생시켜 응답을 GlobalExceptionHandler에서 처리하도록 사용하려고 한다.


```java

@Getter
public class ErrorResponse {

  private final LocalDateTime timestamp = LocalDateTime.now();
  private String message;
  private int status;
  public Map<String, String> errors;
  private String code;

  @Getter
  @Setter
  @NoArgsConstructor
  public static class FieldError {
    private String field;
    private String message;
  }

  public ErrorResponse(ErrorCode errorCode) {
    this.message = errorCode.getMessage();
    this.status = errorCode.getStatus();
    this.code = errorCode.getCode();
  }

  public ErrorResponse(ErrorCode errorCode, Map<String, String> errors) {
      this.message = errorCode.getMessage();
      this.status = errorCode.getStatus();
      this.code = errorCode.getCode();
      this.errors = errors;
  }

}

```

ErrorResponse 클래스는 예외 발생 시 공통된 형식으로 반환하기 위해 만든 DTO클래스이다.

```java

@Slf4j
@RestControllerAdvice
public class GlobalExceptionHandler {

    
    // Controller에서 @Valid 유효성 검증 실패시 해당 exception 발생
  @ExceptionHandler({MethodArgumentNotValidException.class})
  public ResponseEntity<ErrorResponse> handleMethodArgumentNotValidException(
      MethodArgumentNotValidException e) {
    Map<String, String> errors = new HashMap<>();
    for (FieldError fieldError : e.getBindingResult().getFieldErrors()) {
      log.error("name: {} , message: {}", fieldError.getField(), fieldError.getDefaultMessage());
      FieldError error = (FieldError) fieldError;
      String fieldName = error.getField();
      String message = error.getDefaultMessage();
      errors.put(fieldName, message);
    }
    ErrorResponse response = new ErrorResponse(ErrorCode.BAD_REQUEST, errors);
    return ResponseEntity.status(response.getStatus()).body(response);
  }

    // CustomException
    @ExceptionHandler(CustomException.class)
    public ResponseEntity<ErrorResponse> handleCustomException(CustomException e){
        log.error("handleCustomException: {} ", e.getErrorCode());
        ErrorResponse response = new ErrorResponse(e.getErrorCode());
        return ResponseEntity.status(response.getStatus()).body(response);
    }


    // HttpStatus 405 Exception 지원하지 않는 메서드 호출 시 발생
    @ExceptionHandler(HttpRequestMethodNotSupportedException.class)
    public ResponseEntity<ErrorResponse> handleHttpRequestMethodNotSupported(HttpRequestMethodNotSupportedException e){
        log.error("handleHttpRequestMethodNotSupported: {} ", e.getMessage());
        ErrorResponse response = new ErrorResponse(ErrorCode.METHOD_NOT_ALLOWED);
        return ResponseEntity.status(response.getStatus()).body(response);
    }

    // HttpStatus 500 Exception
    @ExceptionHandler(Exception.class)
    public ResponseEntity<ErrorResponse> handleException(Exception e){
        log.error("handleException : {} ", e.getMessage());
        ErrorResponse response = new ErrorResponse(ErrorCode.SERVER_ERROR);
        return ResponseEntity.status(response.getStatus()).body(response);
    }

}


```

@RestContorllerAdvice : RestController 전역에서 발생하는 모든 예외를 처리하는 애너테이션
@ExceptionHandler : 특정 예외를 지정하여 하나의 메소드에서 공통 처리할 수 있도록 하는 애너테이션


### Validation 예외처리

![alt text](image-16.png)
![alt text](image-11.png)

### URL 잘못된 경우

![alt text](image-12.png)

### Pathvariable 입력값이 잘못된 경우

![alt text](image-13.png)

![alt text](image-14.png)


[Global Exception 이해하고 구성하기 : Controller Exception](https://adjh54.tistory.com/79#google_vignette)
[전역 예외 처리(Global Exception Handling)](https://congsong.tistory.com/53) 

---------------------------------------------------------------------


# 📌 코딩테스트1️⃣ : 이상한 문자 만들기

### 🔒 문제 : 문자열 s는 한 개 이상의 단어로 구성되어 있습니다. 각 단어는 하나 이상의 공백문자로 구분되어 있습니다. 각 단어의 짝수번째 알파벳은 대문자로, 홀수번째 알파벳은 소문자로 바꾼 문자열을 리턴하는 함수, solution을 완성하세요.

### 🚫 조건 : 문자열 전체의 짝/홀수 인덱스가 아니라, 단어(공백을 기준)별로 짝/홀수 인덱스를 판단해야합니다.첫 번째 글자는 0번째 인덱스로 보아 짝수번째 알파벳으로 처리해야 합니다.

## 🔓 문제풀이

```java

class Solution {
    public String solution(String s) {
        String answer = "";
        String[] sArr = s.split(""); // 문자열 잘라서 배열에 넣기
        int index = 0; // 문자배열의 인덱스번호 

        for(int i=0; i<sArr.length; i++){
            if(sArr[i].equals(" ")){  
                index = 0;
            } else if(index % 2 == 0){ // 짝수 - 대문자
                sArr[i] = sArr[i].toUpperCase();
                index++;    
            } else if(index % 2 != 0){ // 홀수 - 소문자
                sArr[i] = sArr[i].toLowerCase();
                index++;
            } answer += sArr[i]; 
        } return answer;
    }
}

```


