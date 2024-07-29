---
title: 🏸 2개의 Ajax Request 가 같은 데이터를 수정한다면?
date: 2024-06-10
categories: [Front]
tags: [ajax, async, race condition]
render_with_liquid: false
---
#### profile 을 동시에 2곳에서 수정하고있다?
---
예전에 추가된 newbie modal 이라는 기능이 있다. 처음 가입한 사람이 들어오면 modal 창을 띄워서 여러가지 설정을 한다. 사진, 닉네임, 본인 website, 메일 알림설정 등등 많은 것들을 설정하고 마지막으로 저장을 한다.

#### 저장이 될때도있고 안될때도 있다.
---
정말 힘들었다... 왜 이게 언제는 사진이 저장이 되고 언제는 안되고 그렇다고 에러 로그도 나오지 않고... 너무 힘들었지만 해결하고 나니 안될 수 밖에 없는 이유가 있었다.
기존에 사진, 닉네임, Website 는 본인의 profile 화면에서 수정이 가능했다.
메일 알림 설정은 setting 페이지에서 수정이 가능했다. 따라서 그것을 수정할수 있는 post request를 받으려는 api 가 있었다. 
그 모든것은 profile 이라는 object 에 있었고 DB에도 저장이 되어있었다.

newbie modal 의 마지막페이지에 도달을 하면 그 2가지의 api 를 사용했다.
```js
$("#newbie-modal-save").on("click", function() {
	$.post({profile 화면 에서 수정 하던 로직});
	$.post({setting 화면 에서 수정하던 로직});
})
```

#### Race Condition 이 발생했던 것이었다.
---
정말 힘들었다. 사실 해결 방안은 간단히 sync 하게 진행을 하자! 였지만 여기까지 원인을 찾아내는것이 정말 정말 어려웠다. 

해결방안은 여러가지 방법이 나왔다.
1. JS에서 Sync 하게 Request 를 보내자
2. Spring 에서 새롭게 Endpoint 를 하나 또 만들어서 거기에서 원래 사용하던 함수를 사용하자
2번째 방법으로 하기로 결정했고 잘 해결이 되었다!
후다음부터 JS 를 만들어갈땐 생각 할 수 있는게 하나 더 생겨 좀 뿌듯하다