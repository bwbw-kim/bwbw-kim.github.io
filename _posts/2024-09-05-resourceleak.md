---
title: "Resource Leak 방지법: Try-Finally와 Try-with-Resources 활용하기"
date: 2024-09-05
categories:
  - Spring & JAVA
tags:
  - try-resource
  - resourceleak
render_with_liquid: true
mermaid: true
---
#### Java Resource leak 해결해보기
---
우리 플랫폼 코드에서 Resource leak 를 유발하는 코드가 발견 되었다!
파일이나 네트워크 그리고 데이터 베이스 연결을 한 다음에 연결을 끊지 않으면 리소스 누수가 발생하게 되는데 
심각한 경우에는 시스템이 콱! 멈춰버릴수도 있다!

기존 코드는 아래와 같았다.

```java
OutputStreamWriter wr = new OutputStreamWriter(conn.getOutputStream());
wr.write(jsonRequestObj.toString());
...
```

Output stream 을 열어놓고서는 열심히 사용만 하고 닫지 않았다! 그럼 그냥 닫아주기만 하면 될까?

```java
OutputStreamWriter wr = new OutputStreamWriter(conn.getOutputStream());
wr.write(jsonRequestObj.toString());
...
wr.close();
```

이렇게만 닫아 버리면 중간에 예외 상황이 발생했을 경우 닫히지 않기때문에  이것또한 올바르지 않은 방법이다. 따라서 try finally 를 사용해서 아주 안전하게 close 해주면 된다!

```java
try {
	OutputStreamWriter wr = new OutputStreamWriter(conn.getOutputStream());
	wr.write(jsonRequestObj.toString());
	...
} finally {
	try {
		if (wr != null) wr.close();
	} catch (IOException e){
		e.printStackTrac();
	}
	
}
```
``
#### java 7 이상에서 보다 간편하게 닫기
---
python 에 with 를 사용해서 close 를 사용하지 않고 아주 간단하게 닫는 방법이 있듯이 java에도 그런 방법이 있었다! 바로 try with resource 를 사용하는 방법이다. 아래 처럼 사용하면 close하지 않고도 안전하게 해제 할 수 있다!

```java
try (OutputStreamWriter wr = new OutputStreamWriter(conn.getOutputStream())) {
	wr.write(jsonRequestObj.toString());
	...
}
```

사실 2년 넘게 java를 해왔는데 이번에 이걸 확실하게 알았다는 사실이 조금은 부끄럽기도... 하지만 이제라도 알아서 참 다행이라는 생각이 들었다!
