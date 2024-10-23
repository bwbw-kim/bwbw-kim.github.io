---
title: Github Repository 를 commit 기록까지 한꺼번에 옮기기
date: 2024-10-21
categories:
  - misc
tags:
  - git
  - repo
render_with_liquid: true
mermaid: true
---
#### Github Repository를 그대로 옮겨보자
---
이번에 우리 repo를 다른 organization으로 이사해야하는 일이 생겼다! 그런데 코드만 쏙 옮기고 싶지 않고 commit, release, tag 기록들도 다 같이 옮겨지면 일하는 것에 큰 지장이 없을 것이라고 생각해서 통째로 옮겨 보려고 한다!


#### 우선 코드만 한번 옮겨보자
---
그냥 코드만 옮기려면 아래와 같이 아주 간단하게 하면 된다

```bash
git clone https://github.com/testing/origin-repo.git
cd origin-repo
rm -rf .git # git 에 대한 모든 기록을 삭제해버린다!
```

이렇게 까지 하면 이제 아예 git project 가 아닌것이니 다시 처음부터 해주면 된다.
```bash
git init
git remote add origin https://github.com/testing/new-repo.git
git push -u origin main
```

#### 전체 히스토리를 옮기려면?
---
전체 히스토리를 옮기려면 `--mirror` 옵션을 활용해서 clone을 받아줘야한다
```bash
git clone --mirror https://github.com/testing/origin-repo.git
cd origin-repo.git # .git 이 붙은 directory가 생긴다.
```

그 다음에 내가 원하는 곳의 git url 에 넣어준다!

```bash
git push --mirror https://github.com/testing/new-repo.git
```

#### 혹시 remote rejected 에러가 난다면?
---
mirror 로 받았을경우에는 git refs까지 전부 복사하기때문에 에러가 발생할수가 있다! 그럴때는
```bash
git clone --bare https://github.com/testing/origin-repo.git
```

bare 옵션으로 clone 받고 push해주면 된다!