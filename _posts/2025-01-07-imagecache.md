---
title: Spring 으로 이미지 브라우저에 캐시 저장하도록 알려주기
date: 2025-01-07
categories:
  - Spring & JAVA
tags:
  - image
  - cache
render_with_liquid: true
mermaid: true
---
#### 이미지를 브라우저 캐시에 저장 했을때 장점
---
spring 에서는 resource 아래에 있는 자원(image, css, js) 의 경우에는 기본적으로 브라우저에서 캐싱을 하도록 지원을 한다. 하지만 다른 곳에 있는 이미지는! 따로 우리가 처리를 해줘야 브라우저에서 이를 알고 캐시에 넣는다!
물론 이미지 한장 가져오는것이 엄청 큰 부하는 아니지만 이미지가 무지하게 많은 페이지에 들어갔을때 (예를 들어서 회원 페이지) 이미지가 많이 캐싱되어있다면 여러가지 장점이 있을 것이다!

1. 우선 Spring으로의 요청수가 확 준다. 이미지 가져오는 것도 요청이다! 이게 브라우저 캐시에 들어가 있으면 요청수 1번을 save 할 수 있다!
2. 사진이 깜빡 거리지 않는다! 요청이 없어지기때문에 사진이 바로 딱! 뜬다 그러면 사용자들은 이미지 blinking을 느끼지 못한다!

물론 어떤 url에 있는 이미지가 같은 url을 사용하는데 이미지가 자주 변경된다? 그러면 당연히 캐싱을 사용했을때 별로겠지만 우리 플랫폼은 한 url에 대해 이미지가 변경될 일이 전혀 없었기 때문에 당돌하게 이미지를 브라우저 캐시에 저장하기로 결정했다!

#### Spring 에서 이미지 브라우저 캐시에 저장하도록 하게 만들기
---
file을 리턴하는 controller가 있다면 해당 responseEntity에 cachecontrol 을 넣어주면 된다! 엄청 간단하다! 아래 처럼 한다! 우리의 경우는 이미지가 정말 바뀔일이 없기때문에 revalidate로 하고 캐시의 age를 7일로 길게 었다.

```java
@GetMapping
@ResponseBody
public ResponseEntity<Resource> serveFile(@PathVariable String filename, HttpServletRequest req) {
	Resource file = storageService.load(filename);
	return ResponseEntity.ok()
			.cacheControl(CacheControl.maxAge(2592000, TimeUnit.SECONDS).mustRevalidate())
			.header(HttpHeaders.CONTENT_DISPOSITION, "attachment; filename=\"" + name + "\"")
			.contentType(MediaType.parseMediaType(contentType)).body(file);
	return 
}
```

#### 여러가지 cache관련 옵션들
---
이거 말고도 엄청 옵션이 많다! 확인해보면서 본인의 상황에 맞는 놈으로 잘 골라 사용하면 될거같다!

1. must-revalidate: 만료된 캐시는 반드시 서버와 재검증.
2. no-cache: 캐시에 저장하지만 매번 서버에서 유효성 검증.
3. no-store: 캐시에 저장하지 않고 매번 새로 요청.
4. immutable: 변경되지 않는 데이터, 만료 후에도 검증 없이 사용.
5. max-age: 캐시의 유효 기간을 초 단위로 설정.
6. s-maxage: 공유 캐시(CDN, 프록시)의 유효 기간을 초 단위로 설정.
7. public: 모든 캐시(브라우저, 프록시 등)에 저장 가능.
8. private: 브라우저와 같은 개인 캐시에만 저장 가능.
9. no-transform: 캐시 또는 프록시가 콘텐츠를 변환하지 못하도록 제한.
10. proxy-revalidate: 공유 캐시는 만료된 데이터 재검증 필수.

