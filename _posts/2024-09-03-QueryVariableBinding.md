---
title: "DB 쿼리 성능 높이기: 변수 바인딩으로 속도 개선하기 (python, java)"
date: 2024-09-03
categories:
  - DB
tags:
  - database
  - python
  - mysql
render_with_liquid: true
mermaid: true
---
#### Query 사용시 변수는 binding 하자!
---
DB에 내가 원하는 Query 를 날릴때 보통 나는 이렇게했었다!
```mysql
select * from question where id = "12345";
```

이렇게 쿼리를 날려도 물론 아주 잘 동작 하긴 하지만 사실 이 방법보다 더 좋은 방법이 있다! 바로 변수부분을 binding 하는 것이다.

Query 를 DB에 날릴때는 해당 쿼리를 보고 `실행계획` 을 세운다 그런데 이때 원래 들어왔었던 쿼리라면 실행계획을 `library cache` 라는 곳에서 가져다가 사용 할 수 있는데 그것이 아니라면 실행계획을 다시 세워야한다. (가장 간단한 위의 쿼리로 내 컴퓨터에서 테스트해본 결과 1개의 쿼리에 0.05 초 정도의 시간이 지연이 되었다!)

다시 말해서

```
select * from question where id = "12345";
select * from question where id = "12346";
```

위의 두개의 쿼리는 다른 문장으로 인식하여 2번의 실행계획을 새로 세우게 된다! 이를 해결하기위해서는 제일 처음 말했던 variable binding 이라는 작업이 필요하고 아래처럼 변경을 하면 된다!

```
SET @question_id = "12345";
SELECT * FROM question WHERE id = @question_id;
```

정말 간단하지만 자주 사용되는 쿼리일수록 놓치기 참 쉬운 부분인거 같다!

#### 주의 할 점!
---
띄어쓰기, 대소문자, 탭 이런것도 정확하게 같지 않으면 다른 문장으로 보아 실행계획을 다시 잡을 수 있다!
`strip`을 사용하여 깔끔하게 변수를 넣던가 각 팀에서 사용하는 규칙을 정하는 것이 도움이 될 것 같다!

#### python, java 에서 사용해보자!
---
하지만 실제로 application에서는 저렇게 쿼리를 날리지 않으니 python 그리고 java에서 어떻게 variable binding 을 하는지 예제를 가져왔다!

우선 python은 다음과 같다!

```python
# 바인딩할 변수
question_id = "12345"

# SQL 쿼리 실행
query = "SELECT * FROM question WHERE id = %s"
cursor.execute(query, (question_id,))
```

그리고 java jdbc를 사용하는 예제는 다음과 같다

```java
String id = "12345";
String sql = "SELECT * FROM question WHERE id = ?";
Question question = jdbcTemplate.queryForObject(
	sql, 
	new Object[]{id}, 
	new BeanPropertyRowMapper<>(Question.class)
);
```

앞으로 hard parsing 되지 않도록 변수 바인딩을 잘 사용해야겠다!!