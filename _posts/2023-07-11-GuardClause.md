---
title: 🌿 Guard Clause 로 Clean Code 만들어 보기
date: 2023-07-11
categories: [misc]
tags: [if, guard clause, clean code]
render_with_liquid: true
mermaid: true
---
#### CleanCode를 위하여!
---
Clean Code 책을 읽다가 제일 기억에 남는 부분이 있어서 메모로 남기려고 한다!

지금 우리의 Admin 기능을 담당하고 있는 Controller 는 대부분 이렇게 만들어져 있다

```java
public String updateTopPosts(HttpServletRequest request,
            HttpServletResponse response) {
    if(utils.isAdmin(utils.getAuthUser(request))) {
        동작 수행
    }
}
```

대부분 이렇게 만들어져 있다보니 모든 method 의 indent 가 하나씩 늘어나 있다. 이렇게 indent 가 늘어나면 화살촉 코드가 되고 보기에 매우 안좋다!

하지만 `if(utils.isAdmin(utils.getAuthUser(request)))` 이부분을 다음과 같이 바꾸게 되면 indent 가 하나 줄게 된다

```java
public String updateTopPosts(HttpServletRequest request,
            HttpServletResponse response) {
    if(!utils.isAdmin(utils.getAuthUser(request))) return "";
    
    동작 수행
}
```

이렇게 시작하자마다 권한을 확인하고 권한에 맞지않으면 return 혹은 raise Exception 을 해버리면 indent 도 하나줄고 보기에 아주 편하다!

#### 단 주의할 점이 있다!
---
if 안에 들어가는 내용은 보통 긍정인 경우 개발자들이 읽기 쉬워한다고 한다! 따라서 같이 협업하는 사람들끼리 이야기하고 서로 스타일을 맞추어야할 필요가 있을 거 같다!