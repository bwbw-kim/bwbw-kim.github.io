---
title: "DB 쿼리 성능 높이기: 변수 바인딩으로 속도 개선하기 (python, java)"
date: 2024-09-04
categories:
  - Design Pattern
tags:
  - pattern
  - design
  - structural
  - decorator
render_with_liquid: true
mermaid: true
---
#### Decorator Pattern 이 뭘까?
---
Decorator 패턴이란 어떤 A 라는 클래스에 B 의 기능을 추가, 꾸며주고 싶을때 사용하는 패턴이다!
예를 들어 Phone 이라는 interface 가 있다고 해보자! 그리고 Galaxy24 class가 그친구를 implement 한다!

```java
public interface Phone {
	public void getDescription();
}

public class Galaxy24 implements Phone {
	@Override
	public void getDescription(){
		System.out.println("Galaxy24");
	}
}
```

이상황에서 케이스가 씌워진 Galaxy24 class를 만들고 싶다면 현재는 이렇게 만들어야한다!
```java
public class Galaxy24WithCase implements Phone {
	@Override
	public void getDescription(){
		System.out.println("Galaxy24");
		System.out.println("with case");
	}
}
```

근데 이렇게 만들다보면 열쇠고리 달린 galaxy24, 뭐 하얀 galaxy24.... 등등 클래스가 무수히 많아지고 galaxy25가 나오는 순간 전체 클래스가 2배가 되어버린다! 따라서 `열쇠고리, 케이스, 하얀색` 이런 친구들을 decorate class로 바꿔버린다! 아래처럼 작성을 한다

```java
public abstract class PhoneDecorator implements Phone {
    private Phone phone;

    public PhoneDecorator(Phone phone) {
		this.phone = phone;
    }

    @Override
    public void getDescription() {
        return phone.getDescription();
    }
}

public class CaseDecorator extends PhoneDecorator {
    private Phone phone;

    public CaseDecorator(Phone phone) {
		super(phone);
    }

    @Override
    public void getDescription() {
        phone.getDescription();
		System.out.println("with case");
    }
}
```

이렇게 하면 Decorate가 생길때마다 decorate만 추가해주면 된다! client에서 사용은 다음과 같이 하면되겠다!

```java
Phone galaxy24 = new Galaxy24();
Phone galaxy24withcase = new CaseDecorator(galaxy24);
galaxy24withcase.getDescription();
```

#### 어떤 장점이 있을까?
---
데코레이터와 기본 클래스가 분리되어 훨씬 관리하기가 수월해진다! 그러면서 단일 책임 원칙 이런 clean code원칙들을 잘 지킬수 있게 된다! 또 decorator 의 decorator 이렇게도 가능해지기 때문에 확장성이 상당히 커진다!

하지만 클래스 초기설정들이 길어 질 수 있어보이니까 너무 많은 decorator를 중첩하여 사용하는것은 자제해야할것같다!