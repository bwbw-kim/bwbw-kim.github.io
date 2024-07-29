---
title: 🔮 clickEventTracker 만들어서 어디 클릭했는지 알아보기
date: 2023-10-24
categories: [Front]
tags: [click, tracker, django]
render_with_liquid: false
---
#### Click Event 를 트래킹해보자
---
우리 사이트에서 사용자들이 어떤 곳을 많이 클릭하는지 확인해보고 싶었다. 사실 어떤 페이지에 들어갔는지는 쉽게 파악이 되지만 어디를 클릭했는지는 확인하고 있지 않았다. 따라서 tracker 를 하나 만들어서 넣기로 했다.

#### tracker.js django 로 띄우기
---
우리 서비스와는 아예 별개로 만들어서 우리 서비스 이외에도 사용 할 수 있게 만들고 싶었다.
django 로 띄우고 js 도 cdn 처럼 다른 곳에서 import 할 수 있도록 하려고 한다.

가장 기본적인 js코드는 다음과 같다. 사용자의화면 사이즈도 알고 싶어서 width, height 를 넣었고 width 에 따라 click 은 달라질수있기에 어느 element 를 눌렀는지도 추가하였다.
```js
window.document.addEventListener("click", (position) => {
	// django 로 request 보내도록
	sendReport("click", {
		payload: {
			width: window.innerwidth,
			height: window.innerHeight,
			x: position.pageX,
			y: position.pageY,
			el: position.target
		}
	})
})
```

그럼 django에서 이렇게 request 를 받는다

```python
@router.post("/click")
async def add(request: Request):
	json_data = await request.json()
	new_row = Click(date=datetime.now(), **json_data)
	# width,height,x,y,el이 들어간 Model이다
	with db.session() as session:
		session.add(new_row)

```

이 django 띄우고 우리 사이트에서 
```html
<script nonce="$cspNonce" data-group="qna" src="https://{django}/static/tracker.js"></script>
```

하여 사용하면 된다! 아주 잘 동작하고 사용자들이 사용하지 않는 기능을 쉽게 알아 챌 수 있다!!