---
title: 🍯 Spring Response 다양한 방식으로 주기
date: 2024-07-24
categories: [Spring & JAVA]
tags: [spring, response]
render_with_liquid: false
---
#### Spring Response 하나의 method 에서 view 와 json 을 모두 리턴하기
---
발견한 하나의 꼼수다.
솔직히 하나의 method 에서 두개를 리턴하는것은 좋은 않은 방법이라고 생각한다. 만일 그래야한다면 따로 method 를 나눠야 하지 않을까..
하지만 어쩔수 없이 그런 상황이 온다면 이런 방법을 사용하면 된다!

원래라면 view 를 리턴하는 건 아래와 같다
```java
@Controller
public class TestController {
    @GetMapping("index")
    public String getMainPage(HttpServletRequest request, HttpServletResponse response) {
        return "index";
    }
}
```

이 안에서 view 를 리턴하지 않기 위해서는 response 의 content type 을 변경하면 된다!

```java
@Controller
public class TestController {
    @GetMapping("index")
    public String getMainPage(HttpServletRequest request, HttpServletResponse response) {
		if (!isAjaxRequest(request)) return "index";
		res.setStatus(200);
		res.setContentType("application/json");
		try {
			res.getWriter().println("{\"key\":\"value\"}");
		} catch (IOException ex) {

		}
        return "blank";
    }
}
```

이런식으로 우리는 하나의 method 에서 ajax 에서 들어온 request 라면 view 가 아닌 json 을 리턴하도록 하는 부분이 있다... 하지만 좀 더 이쁘게 깔끔하게 하고 싶은것은 사실이다...