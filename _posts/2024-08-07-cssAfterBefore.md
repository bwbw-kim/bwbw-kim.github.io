---
title: 🏐 css after 그리고 before
date: 2024-08-07
categories:
  - Front
tags:
  - css
  - after
  - before
render_with_liquid: true
mermaid: true
---
#### css 는 어려워
---
보통 spring 만 다루다 보니까 UI를 수정할때 CSS를 거의 뭐 대충 이해하고 있는 수준으로 개발을 진행해왔다. 이러면 안될거 같았고 CSS 도 조금씩 공부해야 겠다 라고 생각했다!
오늘은 css 의 After 그리고 Before 이다!
오픈소스를 가져와서 개발을 해나가다보니 css 에 간혹 after, before 가 나오는데 이 친구들은 뭘까 ?

#### Before, After
---
아래와 같은 친구가 있다고 생각해보자! 
```css
#speech-bubble {
 ...
}
```

```html
<div id="speech-bubble">
	<span>중간</span>
</div>
```

이제 여기에서 after 와 before 을 추가하면 해당 element 의 **자식요소**로 하나가 생긴다! before 은 자식요소중 처음 after 는 자식요소중 마지막이 된다!css 를 다음과 같이 수정을 해보자 

```css
#speech-bubble {
 ...
}

#speech-bubble::before {
 content:"처음";
}

#speech-bubble::after {
 content:"마지막";
}
```

그럼 실제로 화면이 로드되었을때 아래와 같이 뜨는걸 볼 수 있다
```
처음중간마지막
```

개발자 도구로 확인해보면
```html
<div id="speech-bubble">
	::before
	<span>중간</span>
	::after
</div>
```
이렇게 나오는걸 확인 할 수 있다!
이번에 여러곳에서 추가할수있는 speech-bubble 을 넣었는데 화살표를 그리기 위해 after 를 사용했다!