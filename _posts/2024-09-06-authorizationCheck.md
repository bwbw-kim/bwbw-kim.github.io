---
title: 클라이언트쪽 에서도 서버쪽 에서도 권한 확인이 필요한 이유!
date: 2024-09-06
categories:
  - misc
tags:
  - auth
  - client
  - Server
render_with_liquid: true
mermaid: true
---
#### 권한에 따른 필터링은 back, front 둘다 있어야한다!
---
우리 플랫폼에서는 댓글 삭제 기능이 당연히 있다! 댓글을 삭제하기위해서는 Admin이거나 본인의 댓글이어야 한다. 따라서 html에 삭제 버튼을 Admin이거나 본인의 댓글이라면 표시해주었다.

```velocity
#if ($isAdmin || $comment.creatorid == $authUser.creatorid)
	<span id="comment-12345" class="comment-delete">삭제버튼</span>
#end
```

```js
  $('.comment-delete').on('click', function(e) { 
    const targetId = $(this).id();
    $.ajax({
      url: '/comment',
      type: 'DELETE',
      data: { id: targetId },
      ...
    });
  });
```

comment-delete 버튼이 눌리면 이제 서버쪽으로 delete request 를 날려주는 식이다! 하지만 이것만으로는큰 문제가 있었다.

#### 직접 사용자가 Request 를 날리면?
---
지금 브라우저에 저장된 쿠키값들을 다 가져오고 postman 같은것을 사용하면 삭제버튼이 없어보여도 사실상 그 사람은 모든 댓글을 삭제 할 수 있어진다. 따라서 Front 뿐만 아니라 서버에서도 권한 체크가 이루어져야한다.

```java
@ DeleteMapping()
public deleteComment(...) {
	Profile authUser = utils.getAuthUser(req);
	Comment comment = pc.read(commentId);
	if (comment.getCreatorId.equals(authUser.getCreatorId() || utils.isAdmin(authUser)) {
		// 삭제가능
	}
}
```

이렇게 request를 받는쪽에서도 권한 체크를 두번 진행해 주어야 악성 유저가 모든 댓글을 삭제하고 다니는 아주아주 어마무시한 큰일이 일어나지 않는다!

#### 권한 체크는 꼭 필요하다
---
저걸 어느 누가 알았다면 아주 큰일이 날뻔한 상황이다. 어떤것을 삭제하거나 또 관리자만 사용할수있는 기능이거나 이런것들은 꼭 확인하는 작업이 필요하겠다!