---
title: 🎃 Scheduler 분산 시키기
date: 2024-05-13
categories:
  - Spring & JAVA
tags:
  - spring
  - scheduler
render_with_liquid: false
---
#### Scheduler 주기 정하기
---
처음에 Scheduler 를 추가할때 특정시간에 해야하는것 예를 들면 보지못한알림 메일을 매일 아침 9시 보낸다던가 이런 일이 아니고는 `@Scheduled(fixed = 24 * 60 * 60 * 1000)` 이렇게 해서 릴리즈 된 시점에 한번 실행하고 24시간마다 한번씩 돌도록 해두었는데 가만히 생각해 보니 이렇게 하면 문제가 있었다.

#### CPU 사용량의 증가
---
매번 릴리즈 할때마다 직후에 서비스의 CPU 사용량이 아주 높았다. 물론 엄청 높지는 않았지만 평소에 비해서는 굉장히 높은 편이었다. 그래서 그냥 처음 빌드가 되어서 그런줄 알았는데 알고보니 Scheduler 를 저렇게 해놓은 탓도 있었다.

매번 실행하기 부담스러운 일을 scheduler 에 넣어놓곤 했는데 이걸 릴리즈할때마다 하니까! 당연히 서비스가 힘들어하지! 따라서 모든 scheduler 를 특정 시간에 실행이 되도록 수정하였다.

```java
@Scheduled(fixed = ????)
@Scheduled(cron = "...")
```

Fixed 보다는 Cron 을 애용하자!