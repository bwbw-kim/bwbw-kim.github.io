---
title: Cookie와 LocalStorage 비교: 저장 용량과 활용 방법
date: 2023-07-19
categories: [Front]
tags: [cookie, localstorage]
render_with_liquid: true
mermaid: true
---
#### LocalStorage vs Cookie
---
html5 부터 localstorage 를 지원하기 시작했다 이번에 이걸 활용 할 일이 생겼는데 이번기회를 통해 정리해보고자 한다.

#### Cookie
---
Cookie 는 많은 사람들이 익숙할것이다. 쿠키 삭제, 쿠키 허용하기 이런 옵션들을 웹페이지를 왔다 갔다하며 많이 접했을 것이니까!
Cookie 는 map 처럼 key value 쌍으로 이루어져 있고 string 의 데이터만 저장 가능하다. 1개의 Domain에 최대 20개 그리고 1개달 4Kb 까지 저장이 가능하다. 그리고 제일 중요한건 cookie 값들은 request 를 보낼때 함께 날아가게된다. (그래서 cookie 를 이용해서 Auth Check 가 보통 이루어 진다.)

#### LocalStorage
---
반면에 Localstorage 는 오직 정말 오직 Client 만의 정보이다. Server 로는 넘어 오지 않는다. 5Mb 까지 저장이 가능하면 여러가지 객체를 저장할수있기에 확장성이 크다. key, value 형식으로 사용한다.

사용방법도 매우 간단하다! 

```js
window.localStorage.setItem(key, value);

window.localStorage.getItem(key);

window.localStorage.removeItem(key);
```