---
title: 🐽 Spring Scheduler 추가하기
date: 2023-06-12
categories: [Spring & JAVA]
tags: [spring, scheduler]
render_with_liquid: false
---
#### 하루에 한번씩 돌리기
---
시스템상에서 스케줄러를 돌릴일이 생겼다.
하루에 한번씩 돌아가면서 가입자들이 재직자인지, 퇴사자인지 혹은 비실명 (봇) 계정인지
따라서 하루에 한번씩 돌아갈 수 있는 Scheduler 를 만들었다.

```yaml
controller:
	- notcounfcontroller.java
	- questionscontroller.java
utils:
	- emailutils.java
	- chatutils.java 
scheduler:
	- RetireeChecker.java

```
이렇게 controller, utils 와 같은 depth 에 폴더를 두고 여기에 계속 추가해 나갈 예정이다

#### 어떻게 추가하는가
1. 우선 EnableScheduling 이라는 annotation을 main class에 선언해 주어야한다.

```java
@EnableScheduling
```
2. 다음은 @component class 를 하나 선언하고 리턴값이 void 인 method 위에 다음과 같이 annotation 을 추가하면 된다! (cron 도 지원한다)

```java
@Scheduled(cron = "0 0 1 * * ?")
@Scheduled(fixedRate = 1000 * 60 * 60 * 24) // 서비스 시작때 실행 그리고 24시간마다 실행
```

fixedRate 도 좋지만 서비스가 deploy 될때마다 실행되면 부담이 될수 있기에 cron 으로 각각의 스케줄러를 나눠둘 생각이다.

#### 여러개로 서비스 떠있으면? (multi pod)

쿠버네티스 환경에서 서비스가 떠있기 때문에 여러개의 pod 로 서비스가 제공되고 있다. 그런데 scheduler 는 각 pod 마다 설정이 되어있으니 쓸모없이 하나의 작업을 여러번 시도하게 되는 것이다!
사실 이건 깔끔하게 해결 하지 못했다. ㅠㅠㅠ 아래와 같은 방법으로 해결을 하였다

```java
@Scheduled(cron = "${random.int[0,59]} ${random.int[0,59]} 1 * * ?")
public void run () {
	if (isDone()) return;
	// tasks
	// mark run timestamp
}
```

스케줄러를 랜덤시간을 두어 동작하도록 하고 isDone 안에서는 얘가 1시간 안에 돌았던 흔적이 있는지 확인한다. (run 한 시간을  DB에 저장했다)

#### 이런 방법은 어떨까?
더 좋은 방법을 생각하던 중 이런 생각이 떠올랐다
스케줄러를 관장하는 하나의 서버를 더 만들어서 spring scheduler 가 아니라 API 에 요청하면 돌도록 만들어두고
스케줄러 관장 서버가 우리의 서비스에 요청을 하도록 한다면 위의 문제를 보다 깔끔하게 해결 할 수 있을거 같다!
scheduler가 늘어 날수록 관리도 어려울텐데 이 방법이면 좀더 쉽게 관리도 할 수 있을거 같다!