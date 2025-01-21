---
title: Stream 사용시 주의 해야 할 점 (외부에 있는 변수 사용해도 될까?)
date: 2025-01-21
categories:
  - Spring & JAVA
tags:
  - stream
  - java
render_with_liquid: true
mermaid: true
---
#### Stream 으로 외부에 있는 변수 사용해도될까?
---
Stream은 내부가 lambda함수로 구성이 되어있다. 따라서 이때 외부의 stack 에 위치한 변수들은 캡처되어 람다식 내부에 추가가 된다. stream안에서 외부에 있는 변수를 수정하는것은 불가능하다! 따라서 요런 코드는 불가능하다
```java
int a = 5;
numbers.forEach(num -> a += num);
```
하지만 그 값이 변하지 않는 값이라면 가능하다! (effectively final 이라고 표현한다!)
```java
int multiplier = 2;
List<Integer> doubled = numbers.stream()
        .map(num -> num * multiplier) // 읽기만 하는건 괜찮아
        .collect(Collectors.toList());
```

#### 그렇다면 Heap에 선언된 친구는?
----
실제로 그 주소값만 capture되기때문에 외부 list에 add하는것은 동작은 한다 **하지만 권장되지 않는다!!**

```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);
List<Integer> result = new ArrayList<>();
numbers.stream().forEach(result::add);
```

위의 코드는 제대로 동작하지만 사실은 저렇게 작성하는 것 보다는 아래 처럼 작성해 주어야한다.
```java
List<Integer> numbers = List.of(1, 2, 3, 4, 5);
List<Integer> result = numbers.stream().collect(Collectors.toList());
```

우선 stream은 병렬로 처리되었을때에 매번 달라지는 결과값을 가져오게된다. 가끔은 어떤 값이 없을때도 있다. 또 함수형 프로그래밍에서는 외부에 있는 값을 변경하지 않는것을 java 공식 문서에서 권장하고 있다! 따라서 stream안에서는 외부의 변수에 대한 변경을 하지 않는 것이 좋다!

```
* 스트림은 요소에 직접적으로 접근하거나 조작하는 방법을 제공하지 않으며, 대신 원본 데이터 소스와 그 소스에 대해 수행될 계산 작업을 선언적으로 설명하는 데 관심이 있다.

* 간섭 방지 - 간섭 방지는 데이터 소스가 스트림 파이프라인 실행 중에 전혀 수정되지 않도록 보장하는 것을 의미한다.

* 무상태 동작 - 가장 좋은 접근 방식은 스트림 작업에 stateful한 매개변수를 완전히 피하는 것입니다.
```