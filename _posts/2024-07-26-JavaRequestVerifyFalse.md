---
title: SSL 인증 오류 해결: Java에서 Verify False로 Request 보내기
date: 2024-07-26
categories: [Spring & JAVA]
tags: [request, ssl]
render_with_liquid: true
mermaid: true
---
#### Java 에서 Request 보내는데 SSL에러가 난다
---
사내에서 SSL 인증서 발급을 해주고 있다. 하지만.. 사내의 여러 Virtual Machine 이나 폐쇄망에서는 https 웹페이지가 주는 public key 가 정말 유효한 놈인지 아닌지 알지를 못한다. 왜냐면 사내 인증서버와 연결이 잘 안되니까! 그렇다고 하나하나 인증서를 설치 할 수는 없다... 따라서 정말 그러고 싶지 않지만 request 를 보낼때 verify false 를 해야하는 경우가 종종 있다.

Python 에서는 아주 쉽게 가능하다
```python
res = requests.get(... ,verify=False)
```

하지만 Java 에서는 조금 복잡했기에 이를 남겨두고자 한다!
코드는 아래와 같다!
```java
public RestTemplate getRestTemplate() {
	TrustStrategy acceptingTrustStrategy = (X509Certificate[] chain, String authType) -> true;
	SSLContext sslContext = null;
	try {
		sslContext = org.apache.http.ssl.SSLContexts.custom()
			.loadTrustMaterial(null, acceptingTrustStrategy)
			.build();
	} catch (KeyManagenmantException | NoSuchAlgorithmException | KeyStoreException e) {
		e.printStackTrace();
	}
	SSLConnectionSocketFactory csf = new SSLConnectionSocketFactory(sslContext, NoopHostnameVerifier.INSTANCE);
	CloseableHttpClient httpClient = HttpClients.custom().setSSLSocketFactory(csf).build();
	HttpComponentsClientHttpRequestFactory requestFactory = new HttpComponentsClientHttpRequestFactory();
	requestFactory.setHttpClient(httpClient);
	requestFactory.setReadTimeout(5000);
	requestFactory.setConnectTimeout(5000)
	return new RestTemplate(requestFactory);
}
```

그리고 사용은 다음과 같이 하면 된다

```java
RestTemplate restTemplate = utils.getRestTemplate();
Map <String, Object> responseMap = restTemplate.exchange(url, method, httpEntity, new ParameterizedTypeReference<Map<String, Object>>() {}).getBody();
```

이제.. 사내에서 ssl verify false 를 하면서 request 를 할 수 있다!

#### 하지만 사외에서는 함부로 하면 안돼
---
왜냐면 인증 받지 않은 곳의 public key 는 private key 가 여기 저기 돌아다니고 있을 수 있으니 내가 보낸 packet 이 복호화될 가능성이 있다! 주의 해야한다 !!!