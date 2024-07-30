---
title: 🐡 한글 tokenizing 으로 검색 고도화 하기
date: 2024-02-22
categories: [Spring & JAVA]
tags: [korean, token]
render_with_liquid: true
mermaid: true
---
#### 한글 검색이 잘 되지 않는다!
---
Elastic Search 를 도입했더니 영어는 정말 검색을 잘해주지만 아쉽게도 한글은 검색이 잘 안된다. 그 이유는 바로 한글에는 조사가 너무나도 많기 때문이다!

"I Like Kimbap" 이라는 영어문장과 "나는 김밥을 좋아한다" 라는 한글은 다르다. 영어는 딱딱 의미 별로 띄어쓰기가 되어있는 반면 한글을 그렇지 않다! 따라서 Token 화가 필요하다!

#### 한글 Token 화 구현하기
사실 이런 한글을 의미별로 (형태소별로) 구별해주는 패키지는 매우 다양하다. 그중에 나는 사내 mvn repository 에 존재하는 TwitterKoreanProcessor 를 선택했다.

우선 아래처럼 형태소별로 구분을 짓는다.
```java
	CharSequence normalized = TwitterKoreanProcessorJava.normalize(wholeString);
	Seq<KoreanTokenizer.KoreanToken> tokens = TwitterKoreanProcessorJava.tokenize(normalized);
```

그럼 tokens 안에는 이런 결과가 남게된다
```json
{
	"나" :"noun"  ,
	"는" : "josa"  ,
	"김밥"  :"noun"  ,
	"을" : "josa"  ,
	"좋아한" : "adjective"  ,
	"다" : "eomi",
}
```
명사 조사 형용사를 모두 구분 지어준다.! 그다음 저 token 들을 띄어쓰기로 이어준다. 사실 조사는 없어도 검색 성능에는 크게 영향을 미치지 않을것이라고 생각했지만 조사를 넣는가 안넣는가에 따라 
1. 조사를 넣으면 맥락을 검색 할 수 있다.
2. 조사를 안넣으면 간단하고 인덱싱의 크기가 감소한다
이런 장단점이 있었기에 조사까지 넣었다.

마지막으로 String join 을 이용하여 띄어쓰기로 합쳐준다
```java
String.join(" ", TwitterKoreanProcessorJava.tokensToJavaStringList(tokens));
```

한번에 합치면 다음과 같다!

```java
private String tokenizeKoreanString(String wholeString) {
	CharSequence normalized = TwitterKoreanProcessorJava.normalize(wholeString);
	Seq<KoreanTokenizer.KoreanToken> tokens = TwitterKoreanProcessorJava.tokenize(normalized);
	return String.join(" ", TwitterKoreanProcessorJava.tokensToJavaStringList(tokens));
}
```

확실히 검색이 고도화된것이 느껴진다!