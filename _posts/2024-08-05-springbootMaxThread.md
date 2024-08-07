---
title: 🚕 Springboot 최대 Thread 개수 늘리기
date: 2024-08-05
categories: [Spring & JAVA]
tags:
  - spring
  - multithread
  - tomcat
render_with_liquid: true
mermaid: true
---
#### Springboot 의 thread 개수
---
springboot 에는 내장 서버인 tomcat 이 들어가 있다
유저의 요청이 들어오면 Thread pool 에서 하나씩 배정 하여 그것을 사용한다. 이때 Thread 의 개수 제한에 따라 동시에 몇개의 요청을 처리 할 수 있을지가 결정이 된다 실제로 우리 서버에서는 최대 thread 개수가 200개로 기본값이 설정되어 있었다. 따라서 다음과 같은 코드가 있다고 하면

```java
@GetMapping("index")
  public String getMainPage(Model model) {
      System.out.println("started");
      try {
          TimeUnit.SECONDS.sleep(1);
      } catch (InterruptedException e) {
          e.printStackTrace();
      }
      System.out.println("done");
      return "index";
  }
```

400명이 동시에 요청하게 되면 200명은 1초뒤에 바로 index.html 을 볼 수 있지만 나머지 200명은 2초가 지나야 확인이 가능하다

#### Thread 개수 늘리기
---
그 개수를 수정하기 위해서는 application.yml 을 수정하면 된다!

```yml
server:
  tomcat:
    threads:
      max: 400                # 생성 가능한 thread의 총 개수
      min-spare: 20           # 항상 활성화 된 thread의 개수
    max-connections: 8192     # connection의 총 개수
    accept-count: 100         # 작업큐의 사이즈
    connection-timeout: 20000 # timeout 판단 기준 시간, 20초
```

혹시! 위의 값을 수정해도 되지 않는다면 tomcat 이 아닌 jetty 서버를 사용하고 있을 수도 있다! tomcat 을 jetty 로 바꿔주면 된다!