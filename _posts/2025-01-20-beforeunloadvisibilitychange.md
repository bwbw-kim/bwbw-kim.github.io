---
title: Beforeunload, visibilitychange 사용해서 사용자가 웹페이지를 떠날때 이벤트 잡기
date: 2025-01-20
categories:
  - Front
tags:
  - beforeunload
  - visibilitychange
render_with_liquid: true
mermaid: true
---
#### 사용자가 마지막까지 이 페이지에 얼마나 머물렀는지 알고 싶다면?
---
사용자가 어떤 페이지에서 떠날 때를 알고 싶을때 사용 할 수 있는것이 `beforeunload` 이벤트이다!
요게 나는 단순하게 뒤로가기나 다른 페이지를 눌렀을때만 발생하는줄 알았는데 이거 뿐만 아니라 브라우저를 종료했을때도 어김없이 요 이벤트가 호출이 된다!

#### 코드를 살펴보자
---
이렇게 코드를 짜면 사용자가 어떤 페이지에서 얼마만큼 머물렀는지 알 수 있다! 
```js
const enterTime = Date.now();
window.addEventListener('beforeunload', () => {
    const leaveTime = Date.now();
    const duration = (leaveTime - enterTime)
    const xhr = new XMLHttpRequest();
    xhr.open("POST", "/save-stay-duration", false);
    xhr.setRequestHeader("Content-Type", "application/json");
    xhr.send(JSON.stringify({ duration }));
});
```

하지만 이렇게 하면 간혹 엄청 큰 값이 들어올수가 있다!

#### 탭을 바꾸는것 처럼 사이트를 나가지 않았는데 다른곳으로 간거는?
---
`beforeunload` 로는 사용자가 탭을 바꿔서 실제로는 다른 작업을 하고 있는것을 잡을 수 없다. 따라서 이럴때 사용할수 있는것이 `visibilitychange` 이벤트이다! 요 이벤트로는 사용자가 실제로 내 페이지를 보고 있있는지 아닌지를 알 수 있다! 따라서 두개를 합친다면 조금더 정확한 값을 구할 수 있다!

#### 코드
----
visibility를 이용해서 진짜 있었던 시간을 싹 더해서 그 값만 beforeunload에 보내준다

```js
let totalTime = 0; 
let lastEnterTime = Date.now();
document.addEventListener('visibilitychange', () => {
    if (document.visibilityState === 'hidden') { // 안볼때        
        const leaveTime = Date.now();
        totalTime += leaveTime - lastEnterTime;
    } else if (document.visibilityState === 'visible') { // 볼때
        lastEnterTime = Date.now();
    }
});
window.addEventListener('beforeunload', () => {
    const leaveTime = Date.now();
    totalTime += leaveTime - lastEnterTime;
    const xhr = new XMLHttpRequest();
    xhr.open("POST", "/save-stay-duration", false);
    xhr.setRequestHeader("Content-Type", "application/json");
    xhr.send(JSON.stringify({ duration: totalTime }));
});
```


