---
title: Password가 맞는지 아닌지 확인해 보자 (Bcrypt0)
date: 2024-10-01
categories:
  - Spring & JAVA
tags:
  - spring
  - password
  - bcrypt
render_with_liquid: true
mermaid: true
---
#### 특정 패스워드를 입력한 사람만 들어 올수 있는 페이지를 만들어보자!
---
이번 컨퍼런스 행사때 부스를 운영하기로 했다! 부스에서 할 이벤트를 기획하는데 운영중인 Q&A플랫폼에 게임을 하나 심어서 랭킹을 저장하고 3위까지 치킨 상품을 주기로했다! 하지만 우리 플랫폼은 부스에 방문하지 않은 사람도 들어 올 수 있기 때문에 행사장에서만 비밀번호를 입력해서 들어 올 수 있도록 해야했다!

이벤트 페이지에 들어오면 비밀번호를 입력하는 창이 나온다. 거기에서 올바른 비밀번호를 입력하면 내가 원하는 게임이 보이도록 하려고 했다.

Bcrypt 를 사용해서 비밀번호를 암호화 했다.

BCrypt는 비밀번호를 저장하기 위해 사용되는 해시 알고리즘 중에 하나다! 각 비밀번호에 솔트(salt)를 추가하고 한번이 아닌 여러번의 해시를 적용하여 생성이 된다. SHA256보다 훨씬더 강한 보안을 기대해 볼 수 있다!



``` java
private String storedHash = "$2a$12${비밀}.{비밀}"; // 실제로는 DB에 있을것이다!

@PostMapping("/checkPassword")
@ResponseBody
public Map<String, Boolean> checkPassword(@RequestParam Map<String, String> body, HttpServletRequest req, HttpServletResponse res) {
    Map<String, Boolean> response = new HashMap<>();
    String enteredPassword = body.get("password");
    boolean isCorrectPassword = BCrypt.checkpw(enteredPassword, storedHash);
    response.put("passwordMatch", isCorrectPassword);
    return response;
}
```

여기에서 storedHash 는 다음과 같이 생성하면 된다

```java
BCrypt.hashpw("진짜 비밀번호", BCrypt.gensalt(12))
```