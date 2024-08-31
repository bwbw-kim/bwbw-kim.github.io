---
title: 배포 후 웹 자원 캐시 문제 해결: JS와 CSS 강제 새로고침하는 법
date: 2024-08-08
categories:
  - Front
tags:
  - browser cache
render_with_liquid: true
mermaid: true
---
#### 배포하고 나서 js, css 가 refresh 되지 않는다!
---
예전에 해결했던 문제이지만 갑작스레 생각이 나서 정리하기로 했다! 개인 로컬에서 css 또는 js 를 수정하고 나서는 `ctrl+ F5` 로 새로고침을 한다음 수정 된 것을 확인했다. 왜냐면 브라우저가 빠른 로딩을 위해서 js, css 를 cache 에 저장을 해놓기 때문이다! 이게... 개인 로컬에서만 그러면 문제가 없지만 Release 하고 나서도 이러면 정말 문제이다!!

#### 어떻게 해결 할 수 있을까?
---
js 그리고 css 를 브라우저에서 로드 할때

```
https://my.page.com/css/main.css
``` 

같은 곳에서 가져오게 되는데 같은 url 이라면 cache 에서 가져오게 된다. 그런데 뒤에 query parameter 로 항상 다른 값을 넣어주면 cache 에서 가져오지 않고 새롭게 가져오게 된다!

```
https://my.page.com/css/main.css?rev=12345
``` 

이렇게 parameter 를 넣으면 query parameter 가 하는 일은 없어서 가져오는 정보는 같지만 cache 에서 가져오지 않는다. 또 cache 에서 가져오고 싶지 않다면 rev 값을 12345 가 아닌 값을 넣어주면 된다! 이 또한 매번 릴리즈할때 우리가 넣어줄수는 없으므로 우리는 Jenkins Build 번호를 8글자 hash 로 만들어 항상 js 그리고 css 뒤에 붙이도록 했다! 

이렇게 수정을 한 뒤로는 배포하는 시점에는 cache 에서 가져오지 않고 새롭게 다운로드 받고 새롭게 배포하지 전에는 계속 cache 에서 가져 올 수 있게 되었다!