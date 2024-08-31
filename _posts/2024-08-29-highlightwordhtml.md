---
title: "HTML에서 하이라이트 적용하기: 중복 문제를 방지하는 효율적인 방법"
date: 2024-08-29
categories:
  - misc
tags:
  - algorithm
  - highlight
render_with_liquid: true
mermaid: true
---
#### 검색 후 Highlight 처리 로직 개선
---
우리 서비스는 궁금한것을 묻고 답하는 플랫폼이니 만큼 검색 서비스가 있다! 
구글에서 검색을 하면 검색어가 Hightlight가 되는데 우리도 그 기능이 있으면 좋을것 같아서 기능을 구현해 놓았다. 해당 글자에 `<span class="highlight">{해당 글자}</span>` 이렇게 highlight 가 class 로 되어있는 span tag를 감싸면 노랗게 되도록 해 두었다.

예를 들어 python class 라고 검색을 하면 python 으로 한번 highlight 를 하고 class로 다시 highlight 를 하는 로직이다.

아래와 같은 코드로 동작을 한다! innerHtml을 가져와서 그에 해당하는 글자가 있으면 span을 넣고 innerHtml에 넣어버린다. 또 같은 방법으고 다음 글자가 있으면 span을 넣는 동작을 시킨다.
```js
for (let query of splitedQuery) {
	let index = el.innerHTML.toLowerCase().indexOf(query);
	if (index >= 0) {
		el.innerHTML = `${el.innerHTML.substring(0, index)}
		<span class="highlight"> ${el.innerHTML.substring(index, index + query.length)}</span>
		${el.innerHTML.substring(index + query.length)}`;
	}
}
```

이렇게 하면 완벽할줄 알았지만... `class="highlight"` 부분까지 인식해 버린다는 문제가 있었다..
그럼 결과가 아래처럼 기이하게 나온다! 

```html
<span <span class="highlight">class</span>="highlight">python</span>
```


#### 어떻게 해결하지?
---
우선 highlight 씌우는 작업을 중간 중간 query마다 하는것이 아닌 전체 highlight 할 구간을 모두 정해두고 나서 마지막에 씌우기로 결정했다. 그럼 우선 어느 구간이 highlight 해야하는지 알아야하는데 그 구간들을 map 에 정리를 했다. 예를 들면 0 부터 3까지 해야 한다면 highlightedMap 안에는 Set 처럼 정리를 한다.

```json
{
	0 : true,
	1 : true,
	2 : true,
	3 : true
}
```

이렇게 쭉 정리를 하면 중복되는 구간도 한번에 정리를 할 수 있다. 마지막으로 span tag를 끼워야하기때문에 뒷 index 부터 바꿔나가면 된다. 코드는 아래와 같다!

```js
// Map 채우기
let highlightedMap = {};
for(let query of splitedQuery) {
	let index = el.innerHTML.toLowerCase().indexOf(query);
	for(let i = 0 ; i < query.length ; i ++) highlightedMap[index + i +1] = 1;
}

// 뒤쪽 index 부터 span 태그를 씌운다. 전체적인 복잡도는 O(n) 이다.
let endInd = -1;
for (let i = el.innerHTML.toLowerCase().length - 1 ; i >= 0 ; i ++) {
	if (highlightedMap.hasOwnProperty(i) && endInd < 0) endInd = i;
	else if (!highlightedMap.hasOwnProperty(i) && endInd >= 0) {
		el.innerHTML = `${el.innerHTML.substring(0, i)}
		<span class="highlight"> ${el.innerHTML.substring(i, endInd)}</span>
		${el.innerHTML.substring(endInd)}`;
		endInd = -1;
	}
}
```

