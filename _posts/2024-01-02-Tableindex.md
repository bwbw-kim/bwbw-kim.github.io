---
title: "MySQL 인덱스 설정으로 쿼리 속도 100배 빠르게 만들기"
date: 2024-01-02
categories: [DB]
tags: [mysql, index]
render_with_liquid: true
mermaid: true
---
#### 게시글에 들어갔을때 시간이 오래 걸린다!
---
Cache 를 적용했는데! 게시글에 들어갔을떄 시간이 오래 걸린다.. 개발 서버에서는 분명 100ms 아래여서 기분이 너무 좋았는데 운영에 올렸더니 물론 기존보다 빨라지긴 했지만 아직 한참 느렸다.
무엇이 문제였는지 line by line 으로 따라가다보니 문제는 조회수 증가 로직이었다.

우리 조회수 증가 정책은 다음과 같다. 같은 게시글을 계속 들어갔을때 마다 조회수를 증가시키면 악용하는 사람이 있을수 있기에 1시간 동안 그 사람이 그 게시글에 들어 왔는지 확인한 이후에 안 들어왔다면 조회수를 1 증가 시키고 있다. 

이 과정에서 DB 의 user_page_view 와 같은 table 을 조회한다.

```sql
SELECT *
FROM user_page_view
WHERE visit_timestamp > {지금 시간으로 부터 1시간 전};
```

바로 이것이 문제의 쿼리 였다. Q&A 플랫폼이 잘되기 시작하고 가입자도 방문자도 늘어나고 있었다. 그에 따라user_page_view table 은 점점 데이터가 쌓이기 시작했다. 

해결 방법은 바로 column의 index 화 였다. 저기에서 visit_timestamp 는 where 절에서 굉장히 많이 쓰이기 때문에 indexing 하게 되면 tree 형식으로 visit_timestamp 그리고 primary key (auto increment로 되어 있어서 그냥 1,2,3, 이었다) 을 기준으로 싹 DB 구조가 바뀐다. 그렇게 되면 저렇게 where 에 넣었을때 훨씬 빠르게 정보를 가져 올 수 있게 된다. 

| Key_name    | Column_name | Index_type |
| ----------- | ----------- | ---------- |
| create_date | create_date | BTREE      |

빠르게 운영에 적용하고 다시 100ms 의 영광을 가져 올 수 있었다!!!! 

아무래도 기회가 되면 DB 설계 그리고 Query tunning 공부를 해두어야겠다.

