---
title: "어떤 툴, 기능 등이 제대로 설치 되어있는지 python 으로 확인하기: help option으로 꼼수"
date: 2024-09-01
categories:
  - misc
tags:
  - tool
  - install
render_with_liquid: true
mermaid: true
---
#### 어떤 툴이 제대로 설치되어있는지 확인하는 방법은 뭐가 있을까?
---
python, java, c 와 같은 언어들의 스타일을 교정, 취약점 분석 등을 해주는 자동 파이썬 스크립트를 간단하게 개발해 본적이 있다. 그 여러가지 언어들을 교정해주는 다양한 Tool 이 이미 설치가 되어있어야 우리의 스크립트를 자유롭게 사용이 가능했다. 그런데 그 tool 을 사용하려고 python 에서 명령을 내리면 간혹 tool이 없는 경우가 있었다. 

따라서 tool이 실제로 잘 설치가 되어있는지 확인하는 작업이 필요했다. 마치 `flutter docter` 처럼!

#### 대부분의 tool 에는 help 옵션이 있다.
---
그러다가 발견한 꼼수가 바로 이것이었다. 바로 대부분의 Tool 에는 `-h` 혹은 `--help` 와 같이 헬프 옵션이 있다는 것이다. 해당 옵션을 사용하면 큰 동작을 하는 것이 아니고 tool 을 어떻게 사용하려고 하는지 적혀져 있었기에 성능에 큰 짐이 되지도 않았다.

```yaml
pylint :
  command: "-h"
  code: 0
checkstyle :
  command: "--help"
  code :1
cppcheck : 
  command: "-h"
  code : 0
...
```
이렇게 yaml 파일로 미리 싹 정리를 해두고는 tool validation을 할때 한번 싹 실행을 시켜보는 것이다. exit code가 내가 원하는 code라면 설치가 되었다고 판단할수 있다!

help 이외에도 version 으로도 비슷하게 진행 할 수 있을 것 같다!

#### python 코드는 다음과 같다!
---

아래처럼 진행하면 특정 툴이 설치가 되어있는지 확인이 가능 하다!

```
import subprocess
def tool_installation_check(command, code):
	result = subprocess.Popen(command)
	exit_code = result.returncode
	if exit_code != code:
		# 비정상
```