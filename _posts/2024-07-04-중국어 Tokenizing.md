---
title: "중국어 텍스트 분리: Tokenizing으로 검색 성능 향상시키기"
date: 2024-07-04
categories: [Spring & JAVA]
tags: [global, chinese, token, search]
render_with_liquid: true
mermaid: true
---
#### 중국어 tokenizing
---
중국 오픈을 하면서 중국쪽에서 검색이 잘 되지 않는다는 피드백이 들어왔다. 확인해보니 역시 한글처럼 형태소 단위로 띄어쓰기가 필요했다!

pom.xml 에 추가해주고
```xml
<dependency>
    <groupId>com.hankcs</groupId>
    <artifactId>hanlp</artifactId>
</dependency>
```

tokenize 한다음 띄어쓰기로 한글 tokenize 때와 같이 이어주었다!

```java
private String tokenizeChineseString(String wholeString) {
	List<Term> termList = HanLP.segment(wholeString);
    return termList.stream()
                .map(Term::word)
                .collect(Collectors.joining(" "));
}
```

다시 중국인 분들에게 검색이 잘 되냐고 여줘봤더니 잘 된다고 하셨다! 사실 중국어를 하나도 몰라서 잘되는지... 검증이 잘 안된다 중국인 분들과 더욱 연락을 자주해야겠다
