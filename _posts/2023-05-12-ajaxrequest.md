---
title: 🖐 spring에서 내가 보낸 ajaxRequest 거르기
date: 2023-05-12
categories: [Spring & JAVA]
tags: [emptylist, ajax, request]
render_with_liquid: true
---
#### ajaxRequest 거르기
---
우리 플랫폼은 Global variable 같은 것을 model 에 담기 위해서 interceptor 의 posthandle 에서 많은 값들을 읽어서 넣어 주고 있다. 
하지만 우리는 가끔 ajax request 로 우리 서버에 요청을 가끔 하기도 한다! 예를 듦녀 async 로 가져오고 싶은 modal 창 내부 정보 이런것들이 있다! 하지만 ajax request 를 할 때는 model 에 값들을 전혀 담아 주지 않아도 된다! 왜냐면 view 를 return 하지 않아도 되니까! 따라서 Ajax 로 부터 온 reqeust 인지 아닌지를 판단해야한다.

#### ajaxRequest 에는 꼭 포함되는게 있다. 
---
ajax Request 는 X-Requested-With 헤더에 XMLHttpRequest 값을 추가한다! 이렇게나 간단 하다니! 이거 하나도 ajax 의 return 속도를 훨씬 빠르게 만들 수 있다!

```java
public boolean isAjax(HttpServletRequest request) {
	return request.getHeader("X-Requested-With") != null;
}
```
이제 Client js 에서 나에게 오는 요청을 잡아 낼 수 있게 되었다!