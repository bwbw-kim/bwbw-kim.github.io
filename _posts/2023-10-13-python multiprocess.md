---
title: Python Multiprocessing으로 API 호출 속도 향상시키기: 여러 스레드로 한 번에 처리하기
date: 2023-10-13
categories: [Python]
tags: [python, multiprocess]
render_with_liquid: true
mermaid: true
---
#### 메일 보내는 API 가 response 가 너무 느리다
---
최근 이메일 API를 활용해서 사람들에게 이메일을 보내고 있는데 이게 API를 쓰면 응답이 오기까지가 시간이 꽤 걸린다.. 따라서 여러개의 thread 로 돌리면 어떨까 라는 생각을 했다.

역시 파이썬에는 이게 매우 쉽게 할수 있도록 되어있었다!

#### Multiprocess 사용하기
---
예를 들어 어떤 함수가 5초 걸린다고 해보자
```python
def print_what_i_have(n):
	print(n)
	time.sleep(5)
```

이 함수를 5번 실행한다면 총 25초가 걸리겠지만 5개의 thread 로 실행한다면 실행시간이 총 5초로 매우 짧아 질 것이다!

```python
import multiprocessing
import time

if __name__ == "__main__":
	jobs = [1,2,3,4,5]
	processes = []
	for job in jobs:
		p = multiprocessing.Process(target=print_what_i_have, args=(job,))
		processes.append(p)
		p.start()
	for p in processes:
		p.join() # 해당 프로세스가 끝나길 기다린다
```

이렇게 하면 한방에 후다닥 가능하다! 물론 남발하면 서버의 CPU 가 힘들어 할테니 조심해야한다!