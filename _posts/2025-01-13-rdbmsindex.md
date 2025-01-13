---
title: RDBMS mysql column 언제 Index를 설정하는게 좋을까?
date: 2025-01-13
categories:
  - DB
tags:
  - mysql
  - index
  - database
render_with_liquid: true
mermaid: true
---
#### RDBMS 에서 언제 index를 지정하는것이 좋을까?
---
RDBMS에서 index 지정이란 데이터베이스에서 검색 속도를 빠르게 하기위해 설정하는 것으로 B+ tree 자료 구조를 사용하여 구현되기에 binary search를 활용할수 있게 한다. 어떤 경우에 index로 설정하는 것이 유리한지 한번 쭉 정리하면 나만의 기준도 생길 것 같아서 오늘 정리해보고자 한다.

#### Where 절에 들어가는 경우 (단 검색되는 개수가 적을수록 좋다)
---
```sql
select id from table where email = "bwbw.kim@naver.com";
```
email은 딱 봐도 1명밖에 없을 것이다! 이렇게  검색되는 개수가 적은데 where절에 들어간다면 사용하는것이 좋다. 하지만 극단적인 예로 `2000년생 이전` 이런 식이라면 차라리 사용하지 않는것이 좋다

#### orderby, groupby
---
jpa 에서는 `findBySystemOrderByCreatedTimestampDesc` 같은 orderby 같은 경우다
이 경우에도 인덱스를 사용하면 도움이 된다.

#### Cardinality 가 높은 컬럼
---
위에서도 언급 한것과 비슷한데 검색했는데 결과가 너무 많으면 곤란하다. 성별같은 경우에는 2개만 보통있는데 이런 경우르 cardinality 가 낮다고 한다. 이런 경우는 index로 인한 성능 향상을 기대하기 어렵다. 오히려 insert시에 시간이 걸려 안하는것이 낫다.

#### pagination 을 할때
---
보통 pagination 은 시간 순으로 하게되는데 이럴때는 꼭 index로 지정하는 column을 하나 만들어서 그것을 활용하는 것 이 좋다. offset을 이용하면 너무 시간이 오래 걸린다!

#### wildcard 사용할때
----
```sql
select id from table where url like "https://www.naver.com/%";
```
like의 와일드 카드가 맨 처음에 있지 않다면 (뒤일수록 좋다) index설정을 하는 것이 좋다! 