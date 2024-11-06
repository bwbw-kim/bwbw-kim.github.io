---
title: Controller Advice, exceptionHandler 로 Spring에서 에러 발생시 이동하는 페이지 만들어 보기
date: 2024-11-01
categories:
  - Spring & JAVA
tags:
  - spring
  - controllerAdvice
render_with_liquid: true
mermaid: true
---
#### 스프링에서 에러 발생시 이동하는 페이지를 만들어보자
---
개발을 하다보면 사용자는 항상 내가 생각하는 방향으로 움직이지 않는다! 절대로 내가 생각하지 못한 방식으로 행동하고 그것을 모두 커버 하기는 불가능이라고 생각한다! 

우리 플랫폼도 에러가 발생했을때 white label page가 뜨면서 spring 자체 코드의 stacktrace가 보이게 되는 문제가 있어서 이를 해결해기 위해 에러 페이지를 마련하려고 했다!

우리가 생각하지 못했던 에러가 발생했을때 내가 원하는 Error Page로 이동하도록 하기위해서 여러가지 방법이 있겠지만 오늘은 ControllerAdvice 그리고 exceptionHandler를 통해서 만들어보려고한다!

#### MyCustomController 에서 에러 발생했을때 방법!
---
내가 원하는 특정 컨트롤러에서 에러가 발생했을때 에러 페이지로 이동하는 방법은 그 컨트롤러에 `@ExceptionHandler` 를 붙인 메소드를 넣어주면된다! 코드는 아래와 같이 작성하면 된다!
```java

@Controller
public class MyCustomController {
    @ExceptionHandler(Exception.class)
    public String handleQuestionException(Exception ex, Model model) {
        return "error";
    }
}
```

#### 전체적으로 에러가 발생했을때 사용 할 수 있는 방법은?
---
하지만 위의 방법으로는 오직 하나의 Controller 만 커버할수 있는데 전체 Controller, service 등등에서 에러가 발생했을때를 커버하고 싶다면 @ControllerAdvice를 사용하면 된다!

```java
@ControllerAdvice
public class GlobalExceptionHandler {
    @ExceptionHandler(Exception.class)
    public String handleException(Exception ex, Model model) {
        return "error";
    }
}
```

ExceptionHandler에 원하는 에러 exception class를 지정해서 404, 500 에러에 대한 다른 대처 페이지도 마련을 할 수 있다! 