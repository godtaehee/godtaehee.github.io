---
layout: post
title:  "Mac Git version Update (깃허브아님)"
subtitle: "Mac의 Git을 업데이트합니다."
categories: legacy
tags: git-issue
comments: true
---

깃허브는 이제 완벽하게 `main`브랜치를 사용하는데 내 로컬에 설치되어있는 `git`은 아직도 `master`브랜치를 사용해서 브랜치명이 헷갈린다던지 원격레포에 `main`과 `master`가 동시에 있는 레포가 많게되었다. 그래서 `git --version`명령어로 나의 깃 버전을 확인해 보았는데 다음과 같았다.

![img-19](https://user-images.githubusercontent.com/44861205/124285176-a4b08880-db88-11eb-99d9-d72ec3559fdb.png)


맥북을 산지 2년정도가 되가는거같아서 그리고 깃을 그때는 아예 모를때라서 내가 깃을 설치한건지 아니면 맥OS 자체에 기본내장되어있는건지 기억은 안나지만

아무튼 나의 `git`버전은 `2.23.0`이였고 [git 공식 홈페이지](https://git-scm.com)에서의 최신버전은 아래와 같이 `2.31.1`이었다.

![img-20](https://user-images.githubusercontent.com/44861205/124285203-ab3f0000-db88-11eb-8cf3-de5b061ab1d6.png)


`git config --global init.defaultBranch main`이라는 명령어가 `2.23.0`버전에선 전혀 먹히질 않았다. 그래서 뭔가 최신버전이 아닌거같아서 생기는 문제인것 같았고 실제로 맞았다. 그리고 구글링해서 업데이트 방법을 찾아보는데 죄다 Github에대한 얘기만 나와있어서 원하는 검색결과를 찾지 못했다. 이 실제로 맞았다는 것을 알기전에 공식 홈페이지에서 제공하는 다운방법인 `brew install git`이라는 명령어로 git을 설치(나같은 경우는 재설치 덮어씌우기함)해보았지만 내 버전은 `2.23.0`에서 올라가질 않았다.

그래서 `mac git version update`라는 키워드를 통해 `깃을 업데이트(깃헙 아님)` 하는것을 검색해 보았는데 [여기](https://stackoverflow.com/questions/8957862/how-to-upgrade-git-to-latest-version-on-macos)에서도 똑같이 말해주고 있었다.  
나는 `homebrew`가 설치 되어있기때문에 아래와 같이 첫번째 과정만 보고 말았는데

![img-21](https://user-images.githubusercontent.com/44861205/124285236-b3973b00-db88-11eb-8d20-e0f63e175d14.png)


이것도 안되는것이다. 그러다가 처음 `brew install git`명령어를 쳤을땐 정상적으로 설치가 되고 그 설치된 파일을 `link`하라는 말을 못봤었는데 다시 한번 해보니 아래와 같은 조언 메시지를 얻을수 있었다.

![img-22](https://user-images.githubusercontent.com/44861205/124285300-c1e55700-db88-11eb-9d2d-4d7708cdcf00.png)


다시한번 stack overflow를 보니 `homebrew`가 설치되지 않았을때의 방법중에 아래와 같이 link를 하는 과정이 있었다. 그래서 이 글을 보는분들 중에서 `homebrew`가 깔렸음에도 안된다면 나와 같은 방식으로 해결하면 될것 같다.

깃의 최신버전이 이미 깔렸는데 링크가 안되어있다는것이였고 링크를 하기위해서는 `brew link git`을 타이핑 하면 된다고 했고 타이핑을 했지만 또 아래와 같이 에러가 떴는데

![img-23](https://user-images.githubusercontent.com/44861205/124285974-6cf61080-db89-11eb-81ac-fc638fb331e4.png)


`이미 /usr/local/bin/git이라는 타겟이 있기때문에 심볼릭링크를 연결하지 못했다`는 에러가 떴다. 그래서 다양한 조언 메시지를 또 얻을수 있었는데

1.  `rm /usr/local/bin/git`
    1.  타겟이 되는 대상을 지우던지
2.  `brew link --overwrite git`
    1.  링킹하면서 다 그냥 덮어씌우던지
3.  `brew link --overwrite --dry-run git`
    1.  링킹하면서 덮어 씌운파일들을 나열하던지

3가지 방안중에서 나는 처음에 `/usr/local/bin/git`만 지우면 될줄알고 덮어씌우기는 혹시몰라서 첫번째 제안명령어를 타이핑 했는데 또 다음과 같은 오류가 떴다.

![img-24](https://user-images.githubusercontent.com/44861205/124286012-75e6e200-db89-11eb-871d-bfd007b54068.png)


무슨파일인진 모르겠지만 또 저 파일을 링킹하는과정중에 오류가 떴다고 하니 순간 `아 이건 그냥 덮어씌워야겠구나`싶어서 바로 `brew link --overwrite git`명령어를 통해 덮어씌우기를 진행했다. 그렇게 링킹에 성공했고 버전을 확인해보니

![img-25](https://user-images.githubusercontent.com/44861205/124286045-8008e080-db89-11eb-982b-de894fc499d9.png)


이렇게 정상적으로 버전이 바뀐것을 확인할 수 있었다. 다음에는 main브랜치로 바꾸는법과 버전 업데이트를 하고나니까 좀 특이한 현상이 발견됬는데 이 것은 다음 포스팅에서 적도록 하겠다.

# 정리

1.  git --version`을 통해서 자신의 버전을 확인할수 있고 2021.05.29 기준으로 깃의 최신버전은`2.31.1\`이다. 따라서 업데이트를 해주고싶었다.
2.  stack overflow와 공식홈페이지를 통해 `brew install git`이라는 명령어를 통해 깃을 재설치했는데도 버전업그레이드가 안됨
3.  알고보니 심볼릭 링크가 연결이 안됬던 것이고 이것을 해결하기위해 `brew link --overwrite git`을 통해 링크들을 덮어씌우기를 해서 링킹에 성공했다.