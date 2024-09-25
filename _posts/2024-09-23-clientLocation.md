---
title: Geolocation 으로 내 페이지에 접속한 사람의 위치를 대략적으로 가져와보자
date: 2024-09-23
categories:
  - Front
tags:
  - geolocation
  - client
  - address
render_with_liquid: true
mermaid: true
---
#### Geolocation 으로 client의 경도 위도 가져오기
---
최근에 혼자 버스 알림 서비스를 간단하게 만들어 보고 싶어서 토이 프로젝트 처럼 만들어 본게 있다. 어떤 사람이 들어와서 본인의 위치를 중심으로한 지도를 보게 되고 정류장을 클릭하고 버스를 누르면 2정거장 전에 알림을 받을 수 있게 한 서비스 인데 이 기능을 구현하기 위해서는 사용자의 현재 위치를 아는것이 필요했다!

이 과정에서 사용 할 수 있는 것이 js 에서 geolocation이다

#### 어떻게 사용하면 될까?
---
사용방법이 생각보다 너무 간단해서 놀랐다! 우선 브라우저가 geolocation api를 지원하는지 확인을 해야한다! (그래야 에러 로그가 뜨지 않는다)
```js
var cur_latitude = 127.0286009;
var cur_longitude = 37.2635727;
if ("geolocation" in navigator) {
	// 여기에서 cur_latitude, cur_longitude 를 구하면 된다!
}
```

geolocation을 지원하지 않는다면 내가 정한 default 위도, 경도를 가르키도록 할 예정이다!

그리고 아래처럼 코드를 작성해준다!

```js
var cur_latitude = 127.0286009;
var cur_longitude = 37.2635727;

if ("geolocation" in navigator) {
    navigator.geolocation.getCurrentPosition(
      (position) => {
        const { latitude, longitude, accuracy } = position.coords;
        cur_latitude = latitude;
        cur_longitude = longitude;
      },
      (error) => {
        alert("위치 정보를 가져올 수 없습니다.");
      },
      {
        enableHighAccuracy: true,
        timeout: 10000,
        maximumAge: 0
      }
    );
}
```
이제 `cur_latitude` 와 `cur_longitude` 에 들어있는 값을 가지고 맵을 열어주면 된다!

[여기](https://github.com/bwbw-kim/bus-station/blob/main/src%2Fmain%2Fresources%2Fstatic%2Fjs%2Findex.js) 에 가면 내가 어떻게 사용했는지 알 수 있다!