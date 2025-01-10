---
title: String, Stringbuilder, Stringbuffer 의 차이 알아보기
date: 2025-01-10
categories:
  - Spring & JAVA
tags:
  - string
  - stringbuilder
  - stringbuffer
render_with_liquid: true
mermaid: true
---
#### String, Stringbuffer, Stringbuilder 의 차이가 뭘까?
---
java 개발을 하다보면 String을 다루게 되는 경우가 정말 정말 많다! 문자열을 다룰수 있는 Class는 크게 String, Stirngbuffer, StringBuilder 가 있다! 생긴것도 비슷한데 과연 도대체 어떤 차이가 있을까?

#### String
---
우선 String은 불변의 객체이다. 
```java
String s = "Apple";
s = s + "Banana";
```
AppleBanana 를 만들면 기존에 Apple에 더해지는것이 아니라 아예 새롭게 객체가 생겨난다!
따라서 constant한 String을 사용 할 때 주로 사용해야 한다!


#### StringBuffer
---
StringBuffer는 변할수 있다. String과 달리 원래의 객체에 업데이트를한다. 따라서 String 을 더하는 것 보다 업데이트가 빠르다.
그리고 동기화를 지원한다. 내부에 메소드가 `synchroized` 로 작성이 되어있어서 여러개의 thread가 같이 접근하여도 안전하다!

따라서 아래 코드처럼 작성하게 되면 
```java
StringBuffer sb = new StringBuffer("Start");

Thread thread1 = new Thread(() -> {
	for (int i = 0; i < 5; i++) {
		sb.append("A");
		try {
			Thread.sleep(50); 
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
});

Thread thread2 = new Thread(() -> {
	for (int i = 0; i < 5; i++) {
		sb.append("B");
		try {
			Thread.sleep(50);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
});
```

결과는 A,B 가 5개씩 잘 append됨을 확인 할 수 있다.
Stringbuffer는 멀티스레드 상황에서 하나의 문자열을 다루어야할때 사용해야한다.

#### StringBuilder
---
StringBuffer와 마찬가지로 내부의 값을 직접 업데이트한다. 하지만 차이점은 동기화를 지원하지 않는다.

위의 코드를 StringBuilder로 짜게되면 아래와 같은데
```java
StringBuilder sb = new StringBuilder("Start");

Thread thread1 = new Thread(() -> {
	for (int i = 0; i < 5; i++) {
		sb.append("A");
		try {
			Thread.sleep(50); 
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
});

Thread thread2 = new Thread(() -> {
	for (int i = 0; i < 5; i++) {
		sb.append("B");
		try {
			Thread.sleep(50);
		} catch (InterruptedException e) {
			e.printStackTrace();
		}
	}
});
```

이랬을때 A, B 가 각각 5개씩 나오지 않을 확률이 있다!
동기화를 지원하지 않기때문에 StringBuffer보다는 속도가 조금 빠르다!

#### 정리하자면!
---
불변의 문자열을 쓸땐 String, 동기화를 원할때 StringBuffer, 빠르게 수정하고 싶을땐 StringBuilder 를 쓰는것이 좋겠다!

표로 정리하면 아래와 같다!

| **특징**    | **String**       | **StringBuffer**            | **StringBuilder**            |
| --------- | ---------------- | --------------------------- | ---------------------------- |
| **변화**    | 불변               | 변할 수 있음                     | 변할 수 있음                      |
| **동기화**   | 지원하지 않음          | 지원                          | 지원하지 않음                      |
| **속도**    | 느림 (새 객체 생성)     | 동기화 때문에 상대적으로 느림            | 가장 빠름 (단일 스레드에서)             |
| **사용 목적** | 문자열 값이 상수인 경우 사용 | 멀티스레드 환경에서 문자열을 자주 수정할 때 사용 | 단일 스레드 환경에서 문자열을 자주 수정할 때 사용 |
