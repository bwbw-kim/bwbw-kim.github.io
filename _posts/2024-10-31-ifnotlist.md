---
title: "if not list: 의 정확한 뜻 파악해보기"
date: 2024-10-31
categories:
  - Python
tags:
  - python
  - list
render_with_liquid: true
mermaid: true
---
#### List가 비어있을때를 확인 할 수 있는 방법은?
---
가장 처음에 드는 생각은 아래 방법일수도 있다. 사실 내가 그랬다!

```python
target_list = []
if len(target_list) == 0:
    return "Empty"
```

이렇게 하면 target_list의 길이를 가져오고 그것이 0 인지를 확인하는 것이다. 하지만 파이썬 개발자들은 대부분 list의 비어있음을 확인 할때는 아래 처럼 확인한다.

```python
target_list = []
if not target_list:
    return "Empty"
```

이렇게 작성하게되면 위의 코드와 거의 유사한 동작을 한다고 보면 된다! (훨씬 pythonic하고 속도도 약간 빠르다고 한다)

#### 장점이 있을까?
----
위에처럼 작성하게 되면 속도가 아주 약간 빠르다고 하지만 사실 그것보다는 target_list의 None여부도 같이 판단할수있기때문에 아주 유용하다.
`not target_list` 는 `not bool(target_list)` 로 바뀌고 
`bool(target_list)` 는 `target_list == None OR len(target_list) == 0` 이라고 생각 할 수 있다!

따라서 아래처럼 작성하게 되면 NoneType Error를 피할 수 있게 된다! 에러를 미리 방지 할 수 있다!

```python
def get_xxx_from_list(my_list):
    if not my_list:
        return "ERROR"
    ...
```

추가로 또 이 방법은 list뿐만 아니라 tuple, set, dict, string 모두 가능하다! 많이 애용해야지!