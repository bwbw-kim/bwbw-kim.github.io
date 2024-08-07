---
title: 🍺 submit 을 사용 할 때 훨씬 확장성있게, 속도감 있게 만드는 방법
date: 2024-07-19
categories: [Front]
tags: [submit, form]
render_with_liquid: true
mermaid: true
---
#### submitform binding
---
우리 플랫폼을 글을 수정할때 form 을 사용하고 있다. form 은 html tag 중에 하나로 submit 될때 server 측으로 get 혹은 post 요청을 보낼 수 가 있다.

아래와 같은 느낌을 구성이 되어있다.

```html
<form action="/question/edit" method="POST" class="edit-post">
	<input ...></input>
	<input ...></input>
	<input ...></input>
	<button type="submit">$!lang.get("submit")</button>
</form>
```

사실은 여기까지만 있어도 spring 의 `question/edit` 으로 post request 를 보낼수가 있게 된다. 하지만! 이것은 단점이 있다.

#### submit 을 가로채기?
---
우리 플랫폼은 submit event 를 샥 가로채서 ajax 로 처리를 하고 있다. 코드는 아래와 같다!

```js
function submitForm(formElement, callbackfn, errorfn) {
	let form = $(formElement);
	$.ajax({
		type: "POST",
		url: formElement.action,
		data: form.serialize(), // 원래라면 data 로 들어가야할 내용을 여기서 수동으로 넣어준다!
		success: function(data, status, xhr) {
			callbackfn(data, status, xhr, formElement);
		},
		error: function (xhr, statusText, error) {
			errorfn(xhr, statusText, error, formElement);
		}
	});
}

function submitFormSnatcher(formname, callbackfn, errorfn) {
	return $(document).on("submit", formname,  function() {
		submitForm(this, callbackfn, errorfn);
		return false;
	});
}
```

실제로 form 을 submit을 할때 submitFormSnatcher 을 거치도록 조치를 해둔다. 

사용은 다음과 같이 사용하면 된다!

```js
submitFormSnatcher("form.edit-post", function (data, status, xhr, form) {
	// callback
}, function (xhr, status, error, form) {
	// error call back
});
```

#### 왜? submit 을 가로챌까?
---
submit 을 그러면 ajax 로 비동기로 보내기 때문에 사용자에게 로딩 표시를 보여주어 사용자가 사이트가 느리지 않다라고 느끼게 해줄 수 있다!
또한 callback 함수 error 시 handling 해줄 수 있는 함수를 넣어 줄 수 있게되어 아주 사용이 유용하다!
