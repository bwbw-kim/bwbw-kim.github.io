---
title: 🔐 Sql Injection 보안 문제 해결 할 수 있을까?
date: 2023-03-06
categories: [misc]
tags: [SqlInjection, Search]
render_with_liquid: true
mermaid: true
---
#### 검색할때 발생한 Sql injection 보안 문제 해결해보자!
---
초기 우리 검색 서비스는 다음과 같았다. Lucene 이라는 검색 라이브러리를 사용했는데 사용 문법은 다음과 같았다. 찾고 싶은 data 의 구조가 다음과 같다고 생각해보자
```yaml
title : ...
content : ...
timestamp : ...
tags : [],
author :
```
이런 식으로 구성이 되어있었고 검색은 제목과 content 기반으로만 진행을 하려고 했었다. 따라서 다음과 같이 lucene 검색 라이브러리를 사용했다.

```java
properties.title:{검색한 내용} or properties.content: {검색한 내용}
```

여기에서 검색한 내용에 다음과 같이 검색을 할 경우 
예를 들면 `apple OR properties.author : 찾고싶은사람` 이렇게 검색을하게되면 사람 기준 검색도 가능해저 버린다.

#### 어떻게 해결 해야 할까 ??
---
해결방법에는 정말 다양한 방법이 있었지만 지금 빠르게 적용 할 수 있는것은 허용된 문자만 검색 할 수 있도록 하는 것이었다. 우선 중요 정보에는 접근을 할 수 없어서 (중요 정보가 있는 table 은 아예 분리가 되어있었기 때문!)아주 큰 문제는 없었고 저렇게 lucene 문법을 사용하기 위해서는 colon 사용이 필수였기 때문에 lucene 에서 사용하는 특수문자를 사용자가 입력하면 아예 따옴표로 감싸버렸다.

#### 추가 해결
---
그 이후에는 검색을 위한 elastic Search DB 를 아예 도입을 해버렸다. 아예 검색결과 db 를 분리시켜서 보안 문제도 해결하고 Request 를 보낼때 query 로 string 을 보내는것이 아닌 Object 를 보내고 있어서 Sql injection 의 두려움으로 부터 약간은 벗어 날 수 있었다!