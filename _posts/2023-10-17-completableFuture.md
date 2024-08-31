---
title: CompletableFuture로 비동기 작업 처리: 사내 게시판 연동에서 성능 향상하기
date: 2023-10-17
categories: [Python]
tags: [completableFuture, async]
render_with_liquid: true
mermaid: true
---
#### 사내 게시판과의 연동
---
우리 Q&A 플랫폼의 부흥을 위하여! 사내 게시판과의 연동을 한적이 있다. 요구사항은 다음과 같다
1. 새로운 글이 올라오면 사내 게시판에서 정한 형식에 맞게 request를 보내자
2. 우리 플랫폼에서 조회수가 올라가면 그 조회수를 사내 게시판에 보내서 조회수를 사내 게시판에서도 볼 수 있게 하자 (조회수 동기화)
그래서 우선 당연하게 글이 올라오고 마지막으로 `return "redirect:" + "지금 막 작성한 글 주소"` 하기 직전에 request 를 보내는 부분을 추가하였다.
다음 요구사항도 마찬가지로 어떤 사람이 page 에 들어오면 해당 페이지를 return 하기 전! request 를 보내는 부분을 추가하였다.

당연히 문제가 발생했다.

새로운 글을 작성할때 우선 리다이렉트까지의 시간이 느려졌다. 사실 글을 작성하는 것은 잦은 이벤트가 아니라서 무시할수있다고 100번 양보를 해도
글을 조회할때 느려지는것은 용납 할 수 없었다.
request 부터 response 받는 것이 시간이 오래 걸리는걸 어떻게 줄일까 생각을 하다 이 친구를 비동기로 해버리면 request 도 가고 사용자가 느끼기에도 이전과 거의 유사하게 느낄 수 있겠다고 생각했다.

#### 코드는 이렇다!
---
원래 코드는 다음과 같았다.
```java
private void sendPostInfoToBulletinBoard (String postId, String content, String title) {
	// 게시판에 요청 보내기
}
```
이걸 바로 이렇게 수정하였다!
```java
private CompletableFuture<Boolean> sendPostInfoToBulletinBoard(String postId, String content, String title) {
	return CompletableFuture.supplyAsync(() -> {
		// 게시판에 요청 보내기
	});
}
```
이렇게 사소한 수정으로도 시간은 100ms 이상 단축이 된다!

#### 이렇게 수정하고 나니
---
이렇게 수정하고 나니까
1. 글을 올리고 메일 알림을 설정한 사람들에게 메일 보내는 동작
2. 전체 사용자에게 특정 space 권한을 부여하는 작업
3. db에 user log 를 쌓는 동작
등등 여러가지 작업들이 sync 하게 동작 할 필요가 없었다. 많은 부분을 수정 할 수 있었다!