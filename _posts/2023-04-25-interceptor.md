---
title: 🤙 Spring Interceptor 이해하기
date: 2023-04-25
categories: [Spring & JAVA]
tags: [spring, interceptor]
render_with_liquid: true
---
#### Spring 에서 Interceptor 를 써보자
---
Spring 은 아주 많은 곳에서 사용하는 Backend Framework 이다! 열심히 구축해서 Front에서 요청을 주면 그것에 대한 응답을 줄수가 있다! 예를 들면 이런것이다.
`/user/{id}` 로 GET 요청을 보내면 그 ID 에 해당 하는 사람의 정보를 주는것 같은것이다. 하지만 딱 이 요청만 처리하지는 않을것이다. 엄청 많은 요청을 처리해야하고 그 모든 요청에 공통적으로 들어가야 하는 부분이 있을 것이다.
예를 들면 그 요청이 처리 되기 전 Authorized 된 요청인지 확인을 한다던가
아니면 항상 response 에 추가해 줘야하는 값이 있다던가! 이런 상황이 있을 수 있는데 이때 사용 할 수 있는것이 바로 interceptor 이다.

#### 예를 들어보자!
---
```java
@GetMapping("/user/{id}")
public getUserInfo (...){
	...
}
```
여기에 요청을 보내려고 하는데 이 요청 전에 Auth를 확인 하는 작업을 추가하기 위해선 getUserInfo 에 코드를 추가 할 수도 있지만 이렇게도 할 수 있다는 것이다.


```java
@Component
public class QNARequestInterceptorimplements HandlerInterceptor {
	@Override
	public boolean preHandle (HttpServletRequest req, HttpServletResponse res, Object handler) {
		// 여기에서 Auth check
	}
  }
  ```

혹은! 모든 요청에 대해 response 에 추가해야하거나 model 에 추가해야하는 값이 있다면 ! 아래처럼 할 수 있다
```java
@Component
public class QNARequestInterceptorimplements HandlerInterceptor {
	@Override
	public void postHandle (HttpServletRequest req, HttpServletResponse res, Object handle, ModelAndView modelAndView) {
		// 여기에서 추가
	}

}
```

