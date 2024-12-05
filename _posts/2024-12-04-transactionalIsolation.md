---
title: Spring Transactional 의 Isolation 옵션 (격리수준) 알아보기
date: 2024-12-04
categories:
  - Spring & JAVA
tags:
  - jpa
  - transactional
  - isolation
render_with_liquid: true
mermaid: true
---
#### Transaction 이 여러개가 되면 생기는 문제!
---
하나의 transaction은 atomic을 보장한다. 하지만 그게 2개 3개가 늘어나서 동시에 실행이 되다보면 간혹 문제가 되는 경우가 있을 것이다! 대표적으로 3가지의 경우가 있다!

1. Dirty Read
Dirty Read 의 경우는 커밋되지 않은 값을 읽게되는 경우를 의미한다.
```
Transaction1 : X는 원래 1--> X의 값을 2로 바꾼다 ----------> 에러 발생 rollback
Transaction2 : ---------------------------> X의 값을 읽는다 --> 2를 읽음 (실제론1)
```

2. Non-Repeatable Read
non-repeatable read 는 하나의 transaction 안에서 같은 값을 2번 읽었을때 다른 경우를 의미한다.

```
Transaction1 : X는 원래 1----------> X의 값을 2로 바꾼다 --> Commit!
Transaction2 : X의 값을 읽는다(1읽음) ------------------> X의 값을 읽는다(2읽음)
```

3. Phantom Read
phantom read 는 원래는 없었던 정보를 한 transaction안에서 얻게되는 경우를 의미한다.
```
Transaction1 : X 에 1 넣음 ---------> Y 에 1 넣음 --> Commit!
Transaction2 : 값 1인것 모으기(X만 나옴) --------------> 값 1인것 모으기(X,Y 나옴)
```

Dirty Read, Non-Repeatable Read, Phantom Read 순으로 방지하려면 높은 수준의 isolation level을 요구한다! 

#### Transaction Isolaion Level
---
spring transactional 에서는 5개의 level을 제공한다. 각각의 레벨에 따른 정의는 아래와 같다!

| Isolation level      | 설명                                             |
| -------------------- | ---------------------------------------------- |
| **DEFAULT**          | DB 의 기본값 (대부분 Read Committed 라고한다)             |
| **READ UNCOMMITTED** | 다른 트랜잭션에서 아직 커밋되지 않은 값도 읽을 수 있다                |
| **READ COMMITTED**   | 다른 트랜잭션에서 커밋한 값만 읽을 수 있다. 읽으려고 하는게 커밋중이라면 기다린다 |
| **REPEATABLE READ**  | 한 트랜잭션에서는 읽는 값이 무조건 동일하도록 보장한다                 |
| **SERIALIZABLE**     | 완벽한 트랜잭션을 제공한다               
따라서 발생할수 있는 문제를 방지할수있는지 여부는 아래와 같이 정리가 된다!

| **격리 수준**            | **Dirty Read** 방지 | **Non-Repeatable Read** 방지 | **Phantom Read** 방지 |
| -------------------- | ----------------- | -------------------------- | ------------------- |
| **READ UNCOMMITTED** | X                 | X                          | X                   |
| **READ COMMITTED**   | O                 | X                          | X                   |
| **REPEATABLE READ**  | O                 | O                          | X                   |
| **SERIALIZABLE**     | O                 | O                          | O                   |

내려갈수록 완벽한 트랜잭션을 제공하지만 아쉽게도 아래로 내려갈수록 성능은 조금씩 느려진다! 따라서 트랜잭션으로 감쌀 비즈니스 로직을 잘 보고 적당한 친구를 골라주어야한다!

#### Spring Code
---
개념은 어렵지만 코드는 아래와 같다!

```
@Transactional(isolation = Isolation.REPEATABLE_READ)
public void logic() {
    // 로직
}
```

isolation 말고도 여러가지 옵션이 있는데 차차 알아가도록 하겠다!