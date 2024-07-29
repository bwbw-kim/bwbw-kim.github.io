---
title: 🍽️ emptyList 라는건 언제 쓰는걸까 ?
date: 2023-05-03
categories: [Spring & JAVA]
tags: [emptylist, collections]
render_with_liquid: false
---
#### EmptyList 가 리턴된다
---
List 를 뱉는 method를 사용해서 그 List 가 비어있으면 다른 item 을 하나 넣어주려고 했는데Collections.EmptyList 가 튀어나왔다. 
그 리스트에 넣으려고 하는데 unSupportedOpertionException 을 맞이해버렸다...
과연 얘를 왜 사용하는지 너무 궁금했는데 이유를 듣고 나니 아주 좋은 이유 같아서 앞으로 나도 메소드 구성을 할때 빈 list 를 return해야 할 때 상황을 보고 사용하겠다고 다짐했다.

##### 왜 사용할까?
1. 우선 Emptylist 는 변환이 불가능하다.
아까 나처럼add 하려고 하면 바로 Exception을 마주하게 된다. 이는 나중에 버그를 예방하는데 큰 도움을 준다
2. 성능
그리고 제일 중요한 성능이 우수하다. 불필요한 메모리를 줄일 수 있다. 예전에 메모리가 조금씩 세면서 Q&A플랫폼이 죽었었는데 그 이후로 메모리를 줄일 수 있다고 하면 너무 감사하고 바로 적용하게 된다. 이 친구는 불변이고 또 전체 시스템에서 오직 한개만 선언이 되어있으니 분명 메모리 사용량에 도움이 될 것이다. 유용하게 사용해야겠다고 다짐하는 순간이었다.

```java
public List<String> getXXXXList() {
    // some error occurs
    return Collections.emptyList();
}
```

애용하도록 하자!

