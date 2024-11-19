---
title: spring 에서 한 ip로 부터 들어오는 traffic 제한해보기
date: 2024-11-18
categories:
  - Spring & JAVA
tags:
  - spring
  - traffic
  - limit
render_with_liquid: true
mermaid: true
---
#### 사용자의 traffic을 제한 해보자
---
최근에 우리 플랫폼에서 이벤트를 개최했다. 이벤트 공지가 나가자 마자 사람들이 많이 몰려왔고 어느 날 저녁에는 매크로를 통해서 이벤트를 참가하는 사람도 있었다... 매크로를 막을 방법이 필요하던 와중 생각난 것이 traffic을 제한 하는 것이었다.

사이트 곳곳에 숨어있는 이미지를 찾은 것인데 사람들이 메인 페이지 부터 시작해서 a tag가 걸려있는 모든 페이지를 누르고 img tag를 가져와서 눌러보는 매크로를 만들어서 실행한 흔적이 발견 되었다. 당연히.. 우리 플랫폼은 힘들어 했고 죽지는 않았지만 상당히 느려졌었다. 따라서 ip를 기반으로 해서 1분에 30개 이상의 request가 오면 `429` error를 뱉기로 하였다.

### prehandle 에서 request를 거르자
---
spring 요청은 prehandle -> controller -> posthandle ->aftercompletion 순서대로 이루어진다. 그런데 prehandle에서 false 를 return 하게 되면 나머지 순서를 진행하지 않는다. 따라서 prehandle에서 사용자의 ip 를 가져와서 1분내에 30번의 요청이 있었다면 429 code 와 함께 `return false;` 를 해버리기로 했다.

하지만 실제 사이트 접속 요청말고 이미지를 가져오거나 css, js 같은 static 파일까지 request에 포함이 되면 한번의 웹사이트 방문으로 30번 이상의 요청이 들어 올 수도 있으니 allowed url prefix를 두어 제외할 url 목록을 두었다. 

이로써 순수하게 페이지 방문만으로 트래픽을 제한 할 수 있게 되었다.

### ip 기반으로 걸러보자!
---
우선 순서는 이렇게 하려고 한다!
1. ip 에서 요청이 들어오면 requestMap 에 해당 ip 와 그 ip가 들어온 시간, 그리고 요청 횟수를 적는다
2. 1분 안에 또다시 요청이 들어오면 요청횟수를 1 늘린다
3. map 에 적은 시간보다 1분 뒤에 들어오면 다시 map의 count 와  시간을 초기화 한다.
4. 1분 안에 30번의 요청이 오면 429 에러를 뱉는다

그런데 이렇게 개발하게되면 map의 크기가 점점 커지고 memory 이슈가 있을것이라고 생각했기에 1시간에 1번씩 시간이 많이 지난 ip는 map에서 삭제해 주려고 한다.

### 코드는 다음과 같다
---
아래 처럼 trafficLimitService 라는 class를 따로 만들었다.
```
public class TrafficLimitService {

    @AllArgsConstructor
    private static class RequestInfo {
        long timestamp;
        int requestCount;
    }

    private static final Config CONF = new Config();

    private final Utils utils;

    private final String[] ALLOWED_URI_PREFIX = {
        "/images",
        ... // 추가로 제외하고 싶은것들
    };

    private final ConcurrentHashMap<String, RequestInfo> requestMap = new ConcurrentHashMap<>();

    @Scheduled(fixedRate = 1000 * 60 * 60)
    public void removeExpiredRequests() {
        long currentTime = System.currentTimeMillis();
        requestMap.entrySet().removeIf(entry -> 
            currentTime - entry.getValue().timestamp > TimeUnit.MINUTES.toMillis(CONF.getMaxRequestWindowMinuteSize() * 2); // 넉넉하게 주어서 race condition이 발생하지 않기를 기대했다.
        );
    }

    public boolean isTrafficAllowed(HttpServletRequest request, HttpServletResponse response) {
        String ip = request.getRemoteAddr();
        String requestedUri = request.getRequestURI();
        if (isAllowedUri(requestedUri)) {
            return true;
        }
        long currentTime = System.currentTimeMillis();
        if (!updateTrafficRequestInfo(ip, currentTime)) {
            response.setStatus(HttpStatus.TOO_MANY_REQUESTS.value());
            return false;
        }
        return true;
    }

    private boolean updateTrafficRequestInfo(String ip, long currentTime) {
        requestMap.putIfAbsent(ip, new RequestInfo(currentTime, 0));
        RequestInfo requestInfo = requestMap.get(ip);
        if (currentTime - requestInfo.timestamp <= TimeUnit.MINUTES.toMillis(CONF.getMaxRequestWindowMinuteSize())) {
            if (requestInfo.requestCount >= CONF.getRequestLimit()) {
                return false;
            }
            requestInfo.requestCount++;
        } else {
            requestInfo.timestamp = currentTime;
            requestInfo.requestCount = 1;
        }
        return true;
    }

    private boolean isAllowedUri(String requestUri) {
        for (String uri : ALLOWED_URI_PREFIX) {
            if (requestUri.startsWith(uri)) {
                return true;
            }
        }
        return false;
    }
}
```


그리고 위의 클래스를 이제 interceptor에 있는 prehandle에서 사용해준다!

```
public boolean prehandle(...) {
	...
	if (!trafficLimitService.isTrafficAllowed(req, res)) return false;

}
```