---
title: 상태 관리가 쉬워지는 State 패턴 구현 방법
date: 2024-09-02
categories:
  - Design Pattern
tags:
  - design
  - pattern
  - state
  - behavioral
render_with_liquid: true
mermaid: true
---
#### State 패턴에 대해서 알아보자!
---
State pattern 은 어떤 클래스가 여러가지의 상태를 가질때 사용하기 좋은 패턴이다! 내가 지금 글을 쓰고있는 태블릿의 상태를 클래스로 나타내어보자! 이 태블릿은 전원 버튼이 있다! 해당 전원 버튼을 누를때마다
꺼져있는 상태라면 켜지고 켜져있는 상태면 꺼지는 동작을 한번 아래처럼 구현해 보았다.


```java
class Tablet {
	public String currentState = "OFF";
	
	public setState(String state) {
		currentState = state;
	}

	public void pushPowerButton () {
		if (currentState.equals("ON")) {
			currentState = "OFF";
		} else if (currentState.equals("OFF")) {
			currentState = "ON";
		}
	}
}
```

이렇게 작성이 될 수 있다. 그런데 여기에서 만약 state가 점점 늘어난다면 `pushPowerButton` 내부의 if 문은 점점 복잡해 질것이다! 따라서 이러한 state를 따로 클래스로 빼는것이다!

#### State Pattern 을 코드로 직접 구현해보자
---
우선 아래처럼 State 를 클래스로 따로 만들어 준다. 그럼 따로 스테이트가 생기면 TabletState를 implement한 클래스만 하나더 만들어 주면 된다! 우선은 on, off 두개만 만들어보자!

```java
public interface TabletState {
	public void powerPush();
}

public static class ON implements TabletState{  
    public static PowerState powerPush(){  
		return OFF;
    }  
}  

public static class OFF implements TabletState {  
    public static PowerState pushPowerButton(){  
		return ON;
    }  
}
```

그럼 이제 원래의 tablet class는 아래처럼 수정이 될 수 있다.

```java
class Tablet {
	public static TabletState currentState = OFF;	
	public setState(TabletState state) {
		currentState = state;
	}

	public void pushPowerButton () {
		currentState = currentState.pushPowerButton();
	}
}
```

한눈에 보아도 훨씬 state 관리가 쉬워짐을 느낄수있다!!

#### 어떤 상황에 사용하면 좋을까?
---
우선 새로운 상태가 생기더라도 클래스만 하나 더 생성하고 각자의 역할을 정비해주면 된다. 따로 if 문들의 향연을 보지 않아도 된다는 큰 장점이 있다! 이렇게 또 한곳에만 모여있으면 나 뿐만 아니라 다른사람들도 코드를 이해하기 쉬워진다!

하지만 지금처럼 power button만을 누른다면 사실 if 문이면 충분할수있다 하지만 현업에서는 그럴가능성이 매우 적으니까... 또 관리할 클래스가 점점 늘어나서 힘들긴 하지만 if문이 주르륵 있는거보다는 훨씬 유지하기가 쉽다!
