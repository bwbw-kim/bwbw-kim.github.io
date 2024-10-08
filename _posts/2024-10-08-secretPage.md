---
title: spring 에서 random endpoint 만들어서 보안 유지해보기
date: 2024-10-08
categories:
  - Spring & JAVA
tags:
  - spring
  - random
  - security
render_with_liquid: true
mermaid: true
---
#### 마치 endpoint가 랜덤하게 매번 바뀌는것 같은 페이지를 만들어보자!
---
계속해서 이벤트 준비를 하고 있는데 처음에는 비밀번호만 맞추면 내가 원하는 비밀 페이지 (/1d7ec9fq 이런식)로 redirect 하도록 하려고 했는데 이렇게 하면 사실 그 페이지의 url만 알게되면 누구든지 들어올수가 있다! 따라서 조금 더 생각해 보았다.

비밀번호를 맞추면 내가 원하는 랜덤 endpoint 로 이동하고 그 endpoint를 삭제해 버리면 어떨까 라는 생각을 했다. 따라서 해당 랜덤 endpoint를 DB에 잠시 저장하고 그 endpoint에 방문했다면 DB 에서 삭제하는 것이다! 

이렇게 되면 비밀번호를 맞추면 딱 한번씩만 그 페이지에 접속이 가능하고! 누가 어깨너머로 그 url을 봤다고 하더라도 다시 그 url로는 들어 갈 수 없다!


#### 코드를 한번 보자!
---

```java

@PostMapping("/checkPassword")
@ResponseBody
public Map<String, Boolean> checkPassword(@RequestParam Map<String, String> body, HttpServletRequest req, HttpServletResponse res) {
    Map<String, Boolean> response = new HashMap<>();
    String enteredPassword = body.get("password");
    boolean isCorrectPassword = BCrypt.checkpw(enteredPassword, storedHash);
	if (isCorrectPassword) {
        String randomHash = UUID.randomUUID().toString().replaceAll("-", "").substring(0, 16);
		RandomEndpoint randomEndpoint = new RandomEndpoint(randomHash);
		response.put("randomEndpoint", randomHash);
		pc.create(randomEndpoint);
	}
    return response;
}

@GetMapping("/event/{random}")
public String eventPage(@PathVariable("random") String random, Model model, HttpServletRequest req, HttpServletResponse res) {
	RandomEndpoint randomEndpoint = pc.read(random);
	if (randomEndpoint == null) {
		// 없는 페이지를 방문하려는 시도!!
		return "mainpage";
	}
	pc.delete(random);
    // eventPage 를 위해 model에 데이터를 넣는다
    return "eventPage"; 
}
```

이렇게 하면 한번 비밀번호 통과당 한번의 접속 권한을 부여 할 수 있다!