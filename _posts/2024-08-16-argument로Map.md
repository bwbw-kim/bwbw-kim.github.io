---
title: ⚠️ Argument로 Map 을 주는 것은 바람직하지 않다!
date: 2024-08-16
categories:
  - Spring & JAVA
tags:
  - cleancode
  - readability
render_with_liquid: true
mermaid: true
---
#### Method 의 Argument 로 Map 을 주는 것은 바람직 하지 않아!!
---
오늘 클린코드, 코드리뷰 세미나가 있었는데 굉장히 가슴 깊이 와닿는 lesson 이 있어서 짧지만 정리해보려 한다.
가끔 개발을 하다보면 다음과 같은 코드와 마주하는 경우가 있다. 혹은 내가 그렇게 작성했을 수 있다.

```java
public String updateSpaceOption(Map<String, String> allParams) {
	String id = allParam.get("id");
	String name = allParam.get("name");
	...
}
```

코드의 내용은 사실 간단하다. `allParams` 에는 id 와 name 이 key 로 들어있어야하고 그것을 사용하는 함수일 것이다! 하지만 이렇게 작성되면 사용하는 사람 입장에서 쉽지가 않다.

실제 method 를 보지 않으면 전혀 무엇이 필요한지 알 수가 없기 때문이다. 따라서 위의 코드는 다음과 같이 수정되는것이 바람직하다

```java
public String updateSpaceOption(String id, String name, ...) {
	...
}
```

이번에는 어떠한가! method 선언문만 보아도 vscode에서 함수명에 커서만 가져다 놓아도 어떤것이 필요한지 알 수 있다!

### 또 다른 장점은 뭐가 있을까 ??
---
그래서 저런식으로 우리 Q&A 플랫폼의 코드를 수정을 했다. admin 기능중에 저런 문제가 있는 코드가 좀 있었고 4개정도의 method 를 수정했다. 그러고 나니까 Unit Test 가 Fail이 나서 수정을 했는데 Unit Test 코드 또한 훨씬 알아보기 쉽도록 바뀌었다. 저렇게 수정을 하면 Unit Test 도 만들기 쉬워지는 장점이 있다!