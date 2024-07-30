---
title: 🌥 Java 에서 shallow, deep Copy 의 차이 알아보기
date: 2024-05-27
categories: [Spring & JAVA]
tags: [copy, shallow, deep]
render_with_liquid: true
---
#### Copy 의 2가지 종류 Shallow, Deep
---
자바에서 객체를 복사하는데에 는 2가지 경우가 있다. 얕은 (shallow) 복사와 깊은 (deep)복사!
이걸 잘 모르고 개발하다보면 종종 실수하는 경우가 있어서 이번 기회를 통해 정리하고자 한다!

우선 Shallow Copy 는 주소값만을 복사한다. 그러니까 변수명만 다를뿐 실제 가르키는 것은 같다는 의미이다.

```java
Profile authUser = new Profile(...);
Profile copiedUser= authUser;
copiedUser.setName("BWBW");
authUser.getName() // BWBW!!!
```
이렇게 copiedUser 의 값을 바꾸었지만 authUser 의 값도 바뀐다. 왜냐면 heap 에 같은 곳을 바라보고 있기 때문이다. 이것이 바로 shallow Copy 이다!
자칫 실수해서 copiedUser 만 바뀌기를 원했다면! 버그가 생기기 아주 딱이다!

Deep copy 는 정말 새로운 곳에 복사를 하는 것을 의미한다. 그러니까 아까처럼 copiedUser 를 수정해도 authUser 는 수정 되지 않도록! 이는 보통 새롭게 객체를 생성해서 복사하는것이 제일 안전하다

```java
class Profile {
	public Profile getCopy() {
		Profile copiedUser = new Profile();
		copiedUser.setName(this.name);
		...
		return copiedUser;
	}
}

Profile authUser = new Profile(...);
Profile copiedUser= authUser.getCopy();
copiedUser.setName("BWBW");
authUser.getName() // BWBW가아니다!
```
이러면 실제 Heap 공간에 새로운 copiedUser 가 새기기에 authUser 와 무관하게 조작이 가능하다