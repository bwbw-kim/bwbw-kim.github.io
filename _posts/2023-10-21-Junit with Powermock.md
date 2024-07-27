---
title: 간단한 Junit Testcase 만들어보기!
date: 2023-10-21
categories:
  - Spring & JAVA
tags:
  - Junit
  - mock
  - powermock
  - spring
render_with_liquid: false
---
#### Mock (미작성)
```
// src/java/com/test/utils/Byungwook.java
@Component
class public Byungwook {
	DC dbClient;
	@Autowired
	public ByungwookControllers (DC dbClient) {
		this.dbClient = dbClient;
	}

	public String getSpaceReadOption(String spaceName) {
		Map<String, String> spaceOption = dbClient.read(spaceName);
		return spaceOption.get(spaceOption);
	}
}
```

