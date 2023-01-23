---
layout: post
title:  "Private Branch"
subtitle: "Private Branch란?"
categories: legacy
tags: git-issue
comments: true
---

![img-32](https://user-images.githubusercontent.com/44861205/124286993-7df35180-db8a-11eb-92eb-49dcc99c1dd8.png)


이 분의 고민은 현재 자기가 `로컬에서 두개의 브랜치`를 사용하고 있는데 이걸 푸시한다면 원격(깃헙)에는 두개의 브랜치가 모두 올라가느냐 라는 질문인데,

`git push origin master` 우리가 흔히 보던 push명령어 인데 이 명령어의 형태는 원래 `git push [리모트이름] [브랜치이름]` 이다.

즉 push하고싶은 리모트도 우리 맘대로 설정할수 있고 올리고싶은 브랜치도 `우리 마음대로` 할수 있다는 것이다.

그래서 보통 원격(깃헙)에 한번 올리고 나면 오픈소스가 되어버리니까 혹시 private한 소스코드를 가지고싶다면 `private branch`를 하나 따서 그냥 그 브랜치는 `push하지 않고 계속 로컬에서만 사용`하면 된다. `private branch`라고 해서 특별한게 아니라 그냥 `git branch`명령어로 만들수 있는 똑같은 일반 브랜치인데 내가 의도적으로 깃헙에는 push 하지않는 브랜치라고 생각하면된다.