---
title: Spring으로 Json Response 로 주기
date: 2023-04-18
categories: [Spring & JAVA]
tags: [spring, basic, json]
render_with_liquid: false
---
#### Spring Repsonse 로 View 가 아닌 Json 주기
---
요즘은 사실 frontend 에서 정보를 요청하면 json 으로 return 을 하는 방식이 제일 많이 사용하는 방식 일 것이다.
우리도 플랫폼 내에서 API 를 제공하는 경우 혹은 client 측에서 ajax 로 request 를 보내와서 json 으로 리턴하고 싶은 경우가 종종 존재했다!
따라서 @Controller 에서 json 으로 응답을 해야했다.

#### @ResponseBody 사용하기
@Controller 가 붙어있는 Class 의 경우에는 method 에 @ResponseBody 를 annotation 으로 붙여주면 된다.

이것이 원래 그냥 view 를 return 하는 방법이다

```java
@Controller
public class TestController {
    @GetMapping("index")
    public String getMainPage(Model model) {
        return "index";
    }
}
```

반면에 이것은 정말 있는 그대로를 return 하는 방법이다!

```java
@Controller
public class TestController {
    @ResponseBody
    @GetMapping("index")
    public String getMainPage(Model model) {
        return "index";
    }
}
```

@ResponseBody 를 붙이면 index.html 이 렌더링 되는것이 아닌 정말 String index 가 나오는 것이다.

#### json은 어떻게 리턴할까?
json 은 key value 형식이기 때문에 java 에서는 hashmap 과 비슷하지 않은가! 바로 map 을 리턴하면 된다!

```java
@Controller
public class TestController {
    @ResponseBody
    @GetMapping("index")
    public Map<String, String> getMainPage(Model model) {
        Map<String, String> res = new HashMap<String, String>();
        res.put("body", "success");
        return res;
    }
}
```

#### RestController 는 뭘까?
RestController 는 보통 API 설계를 할 때 사용하는것 같다. @Controller 그리고 @ResponseBody 를 합친것과 같은 역할을 한다.
따라서 모든 method 가 responsebody 를 return 하고 싶다면 @RestController 를 class 에 붙이면 된다!

