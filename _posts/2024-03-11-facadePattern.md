---
title: "Facade 패턴으로 코드 복잡도 줄이기: 간단한 인터페이스로 복잡한 작업 처리"
date: 2024-03-11
categories:
  - Design Pattern
tags:
  - pattern
  - design
  - structural
  - facade
render_with_liquid: true
mermaid: true
---
#### Facade Pattern 이 뭘까?
---
Facade 패턴은 사용자가 아주 간단한 인터페이스만로 사용해서 복잡한 동작을 사용하게 할때 유용한 패턴이다!
예를 들어 어떤 비행기가 있다고 했을때 사용자는 단순히 `fly` 과 같은 간단한 인터페이스를 실행하는것 만으로 비행기를 날릴수 있지만 사실 그 안에는 `engine, door, wheel` 과 같은 친구들을 통해서 날수있는지 확인도 하고 엔진도 켜고 출발하고 하는 일련의 동작이 싹다 들어있는 것이다!

#### 한번 코드로 확인해보자!
---
우선 각각 engine, door, wheel 의 클래스를 작성해 보자
```java
class Engine {
	public void run() {
		// 엔진을 켜자
	}
}

class Door {
	public boolean isOpen() {
		// 문을 열려있는가?
	}
	public void close() {
		// 문을 닫자
	}
}

class Wheel {
	public boolean isOkay() {
		// 고장은 안났나?
	}
	public void run() {
		// 바퀴 달린다
	}
}
```

요런 상태라고 해보자 실제라면 client가 저 메소드들을 전부 실행을 해가면서 비행기를 날려야 겠지만 facade pattern에서는 저 모든 일련의 과정을 캡슐화 한다!

```java
class Airplane {
	public void fly() {
		Engine engine = new Engine();
		Door door = new Door();
		Wheel wheel = new Wheel();
		if (door.isOpen()) door.close();
		engine.run();
		if (wheel.isOkay()) {
			wheel.run();
		} else {
			raise Exception();
		}
	}
}
```

그럼 아주 간단하게 사용자 입장에서는 Airplane 만 가져와서 날리면 된다!

```java
Airplane airplane = new Airplane();
airplane.fly();
```


#### 어떤 장점이 있을까?
---

우선 사용자는 내부가 어떤 로직인지 크게 알 필요가 없다! 저 많은 코드를 작성하다보면 결국 어딘가에서 또 실수가 일어나고 말것이다.
그러니까 계속된 공통적인 작업에 대해 아주 간단하게 사용 할 수 있다! 보통 라이브러리를 만들때 요렇게 만든다고 한다!

이번 패턴은 클래스로 만드는 경우 뿐만 아니라 반복되는 일반적인 동작들을 메소드 하로 encapsulation 하는데에도 도움이 될거 같다.