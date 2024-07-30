---
title: 🚧 영어 tokenizing 으로 검색 고도화 또 하기!
date: 2024-04-22
categories: [Spring & JAVA]
tags: [token, English]
render_with_liquid: true
mermaid: true
---
#### 영어 tokenizing
---
영어 Tokenizing 요청이 들어왔다. 사실 엄청 큰건 아니고 우리 파트에서 정기적으로 올리는 글이 있는데 제목이 DevLetter 이다.
이때 대문자 기준으로 하나씩 띄워져서 검색을 하고 싶은데 실제 Indexing 을 devletter 로 들어가 있으니까 검색이 잘 되지 않았다.

#### Regex 로 해결하기
---
물론 돌면서 UpperCase 인지 확인하는 방법도 있지만 java 의 string split 은 regex 를 input 으로 받기때문에 조금 더 간단하게 해결하였다.

```java
private String tokenizeEnglishString(String wholeString) {
    String.join(" ", wholeString.split("(?=[A-Z])")).tolowerCase();
}
```

