---
title: "버튼 클릭으로 실시간 번역! 웹 페이지에 글로벌 번역 서비스 추가하기"
date: 2024-04-01
categories: [Front]
tags: [translate, global]
render_with_liquid: true
mermaid: true
---
#### 페이지 번역 서비스를 만들자
---
Q&A 플랫폼의 글로벌화를 앞두고 기존에 한국인들만 사용하던 서비스를 번역하여 많은 사람들이 읽을 수 있도록 하는 작업이 꼭 필요해졌다!
나는 그냥 번역 API만 있으면 금방 해결이 될 줄 알았지 하지만 아니였어..

사내에는 utrans 라고 번역 을 해주는 서비스가 있다.
처음에는 게시글의 적당한 위치에 `translate` 버튼을 만들어서 
누르면 다시 새롭게 backend 에서 전체 번역 된 결과를 줘서 보여주자 였는데 
결정적으로 시간이 너무 오래 걸렸다. 
제일 긴 글의 경우에는 10초가 걸리는 글도 있어서 이 방법은 폐기

#### 여러가지 방법 생각
---
1. 미리 저장해 둘까?
우선 제일 먼저 든 생각은 글의 번역본을 미리 저장해 놓자 였다.
하지만 게시글, 댓글, 대댓글, 답변 등등 양이 너무 많고 한국인 사용자 (5만명) 에 비해 중국인 사용자 (5백명) 은 너무 수가 적었다. DB 에 싹다 저장하는건 비효율적일 것이라고 판단.
2. 번역 버튼을 누르면 순차적으로 위에서 부터 번역이 되도록 할까 ?
비슷하게 번역이 되는 서비스를 확인해 보았는데 `translate` 버튼을 누르자 로딩 표시를 띄우며 위에서부터 조금씩 번역을 해주었다. 사용자 입장에서 사이트가 느려진거 같다는 느낌을 받지 않고 번역 서비스를 이용 할 수 있는 좋은 방법이라고 생각하여 채택하게 되었다.

#### TextNode 뽑아서 번역하기
---
현재는 게시글에 들어가서만 번역이 되도록 하고 있는데 분명! 분명! 다른 페이지 가령 메인 페이지 같은 곳에서도 번역을 해달라는 요청이 들어올것이라고 생각해서 번역을 하고 싶은 element 에 class 로 translate-target 을 붙여주면 앞으로 아래의 tralnslsateByClass 함수를 부르면 언제든 번역이 될 수 있도록 설계하였다.

또한 element 중에 textnode 만 뽑아서 번역된 결과를 집어 놓도록 해서 전체적으로 HTML 구조가 무너지지 않도록 하였다


```js
tralnslsateByClass('translate-target', target);

async function (className, target) {;
	const textNodes getTestNodesFromClass(className)
	replaceWithTranslatedTextToNodes(textNodeList, target)
}

function getTextNodeFromClass() {
	const elements = document.querySelectorAll(`.${className}`)
	let textNodes = []
	elements.foreach(element => {
		textNodes = textNodes.concat(getTextNodes(element))
	})
}

function getTextNodes(element) {
	let textNodes = [];
	element.childNodes.forEach(node => {
		if (node.nodeType === Node.TEXT_NODE && node.nodeValue.trim() !== '') {
			textNodes.push(node);
		} else {
			extractTextNodes(node);
		}
	});
	return textNodes
}

async function replaceWithTranslatedTextToNodes (textNodes, target){
	const texts = textNodes.map(node => node.nodeValue)
	for (let index =0 ; index < textNodes.length ; index ++)
		const translateResult await translateText(texts[index], target)
		textNodes[index].textContent = translatedReult;
	}
}

async function trnslateText () {
	// translate api 호출하기
}

```

빠르고 잘된다! 여기에 코드를 넣지는 않았지만 로딩표시도 추가했는데 사용자가 느끼기에 사이트가 느리다는 느낌을 받지 않을 수 있을 것 같다.