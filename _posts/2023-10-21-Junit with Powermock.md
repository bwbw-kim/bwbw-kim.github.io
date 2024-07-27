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
#### 의존성 있는 Class 가 안에 있다면?
---

쉽게 테스트 할 수 있는 메소드만 있을 수는 없다. 왜냐면 Spring 을 개발하다보면 의존성 주입은 무조건 사용하는 것이기 때문에
Mock 이라는 개념은 무조건 알 고 있어야 한다. 아래의 class 를 대상으로 TC 를 만들어 보자!

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
		return spaceOption.get("read");
	}
}
```

#### Mock 으로 의존성 있는 Class 를 내가 원하는 대로 움직이기
---

실제로 unit test 를 만들어보자!
DBclient 라는 놈이 있기 때문에 **이 친구가 어떻게 return 을 밷어줄지** 도 Unit Test 상황이기 때문에
실제 **DB 와도 연결이 되어 있지 않기** 때문에 우리는 저 DBClient 를 조정해줄 필요가 있다. 

```java
@Mock
DC dbClient;

//혹은

dbClient = mock(DC.class);
```

이렇게 Mocking 을 해주면 우리는 이제 이 dbClient 라는 객체를 조작할수 있게 된다.

그러면 `dbClient.read(spaceName)` 을 해주었을때 나는 dbclient 가 read 라는 key 를 가진 map 을 리턴해라 라고 조작하면 된다. 그 코드는 다음과 같다.

```java
when(dbClient.read(spaceName)).thenReturn(mockMap);
```

모두 정리해보면 다음과 같이 Unit Test 를 작성 할 수 있다.

```java
import static org.mockito.Mockito.*;

@RunWith(PowerMockRunner.class)
@PowerMockIgnore("javax.management.*")
public class ByungwookTest {

	@Mock
    private DC dbClient;
    private Byungwook byungwook;

    @Before
    public void setUp() {
        byungwook = new Byungwook(dbClient);
    }

    @Test
    public void shouldGetProperSpaceReadOptionWhenGetSpaceReadOption() {
        // Given
        String spaceName = "testSpace";
        Map<String, String> mockMap = new HashMap<>();
        mockMap.put("read", "ALL");
        when(dbClient.read(spaceName)).thenReturn(mockMap);

        // When
        String actualResult = byungwook.getSpaceReadOption(spaceName);

        // Then
		String expectedResult = "ALL";
        assertEquals(expectedResult, actualResult);
    }
}
```

이렇게 하면 의존성주입된 class 를 크게 생각하지 않으면서 unit test 를 만들기가 아주 쉬워진다!