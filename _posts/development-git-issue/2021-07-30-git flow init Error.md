---
layout: post
title:  "git flow init Error"
subtitle: "git flow init시 원인불명의 오류를 다룹니다."
categories: legacy
tags: git-issue
comments: true
---

git의 확장인 git-flow를 `brew install git-flow-avh` 명령어로 설치후

아무것도 안했는데 아래와 같이 

<img width="483" alt="스크린샷 2021-07-30 오후 6 06 20" src="https://user-images.githubusercontent.com/44861205/127629894-79eec275-5a63-4c15-b8c2-847206421ff8.png">


```
...생략
_flags_fatal: command not found
too many arguments
```

오류가 발생했다. 원인은 [링크](https://github.com/fork-dev/Tracker/issues/418) 에 나와있는데 

`rm -rf ~/.gitflow_export`

명령어를 통해 source tree의 path를 삭제해주면 해결이 되었다.

