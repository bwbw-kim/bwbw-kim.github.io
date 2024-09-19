---
title: 인덱스 설정으로 개선했던 Table 한번 더 개선하기!
date: 2024-09-11
categories:
  - DB
tags:
  - database
  - mysql
  - index
render_with_liquid: true
mermaid: true
---
#### 이전에 개선했던 Table Index를 한번 더 개선해보자!
---
예전에 사용자가 이미 한번 조회했던 post의 조회수를 중복으로 늘리지 않기 위해 (1시간) 만든 Mysql table을 개선한 적이 있었다. 

우선 사용하는 쿼리는 다음과 같았다.
```mysql
select distinct uri
from user_log
where knox_id = "id" and
      uri like "%question/%" and
      create_date > "한시간전";
```

여기에서 create_date 를 기준으로 1시간 전보다 큰 date를 싹 긁어오는데 이걸 원래는 table full scan을 했었는데 index를 적용하여 range scan이 가능하도록 하였었다. 그런데 이게 점점 Q&A 플랫폼에 접속하는 사람도 많아지고 하면서 1시간 전보다 큰 date 자체도 데이터의 수가 많이 늘어났다.

#### 쿼리를 수정하자
---
우선 쿼리가 불필요하게 많은 정보를 가져오고 있었다. 내가 들어간 post에 해당하는 데이터만 가져오면 되는데!
```mysql
select count(*)
from user_log
where knox_id = "id" and
      uri like "%question/{postid}%" and
      create_date > "한시간전";
```
그리고 `한시간전` 해당 하는 데이터보다 `특정 post` 에 해당하는 데이터가 양이 훨씬 적다. (아무리 많아도 5000조회수를 잘 넘지 않았다.) 따라서 **uri 컬럼을 index**화 하기로 했다. index를 사용하기 위해서 앞쪽 wildcard 는 삭제하였다.

```mysql
select count(*)
from user_log
where knox_id = "id" and
      uri like "/question/{postid}%" and -- index
      create_date > "한시간전";
```

이 값이 이제 0 보다 크면? 증가시키지 않으면 되는것이다. 이로 인해 개선된 시간은 30ms 에서 1ms 로 줄어들었다!!

역시 공부를 하고 안하고는 차이가 굉장히 큰것같다...