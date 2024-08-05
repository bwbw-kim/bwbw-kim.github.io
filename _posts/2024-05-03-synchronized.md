---
title: ❤️ synchronized 사용해서 method atomic 하게 만들기!
date: 2024-05-03
categories: [Spring & JAVA]
tags: [synchronize, multithread]
render_with_liquid: true
mermaid: true
---
#### synchronized 사용해보기
---
개발을 하다보면 여러개의 thread 들이 하나의 데이터를 접근하고 사용하는 경우가 있다. 그러면 race condition 이 생길 수 있고 여러 문제가 발생 할 것이다! 따라서 java 에서는 이것을 해결 하기 위해서 synchronized 가 있다!
예를 들면 아래와 같은 상황이 있다고 해보자!

```java
public class ThreadTest {
    public void run() {
        Thread thread1 = new Thread(() -> {
            method("1");
        });
        thread1.start();

        Thread thread2 = new Thread(() -> {
            method("2");
        });
        thread2.start();
    }
    private void method(String msg) {
        System.out.println(msg + " started");
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(msg + " done");
    }
}
```

위의 코드를 실행해보면 
```log
1 started
2 started
1 done
2 done
```

과 같이 로그가 출력이 된다! 이떄 synchronized 를 사용하게 되면

```java
public class ThreadTest {
    public void run() {
        Thread thread1 = new Thread(() -> {
            method("1");
        });
        thread1.start();

        Thread thread2 = new Thread(() -> {
            method("2");
        });
        thread2.start();
    }
    private synchronized void method(String msg) {
        System.out.println(msg + " started");
        try {
            TimeUnit.SECONDS.sleep(1);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        System.out.println(msg + " done");
    }
}
```

method 가 atomic 해지게 되고 출력 결과는 다음과 같다!

```log
1 started
1 done
2 started
2 done
```
