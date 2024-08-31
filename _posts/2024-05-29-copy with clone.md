---
title: "Java에서 배열 복사 속도 비교: clone vs index (어떤 방법이 더 빠를까?)"
date: 2024-05-29
categories: [Spring & JAVA]
tags: [copy, clone]
render_with_liquid: true
mermaid: true
---
#### 주소값이 다르도록 복사하기!
---
Java에서는 배열을 주소값이 다르도록 Deep Copy 할때 2가지 방법으로 복사 할 수 있다!
우선 첫번째 방법은 그냥 복사이다. 그냥 복사는 말 그래도 하나하나 복사 하는 것이다.

```java
int[] srcArr = {1,2,3,4,5};
int[] dstArr = new int[5];
for (int i = 0; i < srcArr.length; i++) {
	dstArr[i] = srcArr[i];
}
```

두번째 방법은 clone을 이용하는 것이다
```java
int[] srcArr = {1,2,3,4,5};
int[] dstArr = new int[5];
dstArr = srcArr.clone();
```

#### 뭐가 더 빠를까 ?
---
clone 은 메모리기준으로 복사하기 때문에 사실 인덱스를 돌면서  복사하는것 보다 빠르다! 그 하나의 크기가 작을 수록 속도 차이는 크게 나게 된다.

```
char 은 10배 차이
int 는 5배 차이
long 은 3배 차이
```
