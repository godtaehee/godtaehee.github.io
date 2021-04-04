---
title:  "Git LifeCycle (2)"
excerpt: "Git이 파일들을 어떤 Cycle을 통해 관리하는지 알아봅니다."
layout: single
classes: wide
categories:
  - Git
tags:
  - Git, Github
---

# Git LifeCycle

이 글에서는 Untracked를 뺀 나머지 개념을 다루고 있습니다. Untracked가 생각보다 양이 많아서 따로 다루었기 때문입니다. Untracked는 [여기](https://wonderfulhuman.github.io/git/Git-LifeCycle/) 에서 볼수있습니다


![Git LifeCycle]({{ site.url }}{{ site.baseurl }}/assets/images/2021-04-03-GitLifeCycle/lifecycle.png)

Untracked와 마찬가지로 위의 사진을 가지고 개념설명을 하겠습니다.

### Unmodified
- Commit을 모두 마친 상태이며 Commit은 깃에 내가 만든 코드를 저장을 완료한 상태입니다. 이렇게 저장이 완료되면(Commit 되면) 언제든 내가 저장했던 시점 (Commit했던 시점)의 코드를 볼수있으며 본래 작업했던 소스코드로도 언제든 돌아올수 있게 됩니다. 저장을 완료했고 그 다음에 아무런 소스코드를 수정하지 않았다면 Unmodified 상태, 즉 아무것도 아직 수정하지않은 상태가 됩니다.
- Unmodified상태는 `git status`명령어를 통해 직접 눈으로 `Unmodified`라는 단어를 가지고 파악할수 없습니다. 그저 아래와 같이 직접적으로 `Unmodified`라고는 하지않지만 아래의 이미지처럼 `nothing to commit, working tree clean`라는 메세지를 출력합니다.

![unmodified]({{ site.url }}{{ site.baseurl }}/assets/images/2021-04-04-GitLifeCycle/unmodified.png)

- `Staged`상태에서 `Commit`을 하게된다면 `Unmodified` 상태가 됩니다.

### Modified
- Git으로 **관리되고 있던** 코드를 수정하여 변경이 일어난 상태를 말합니다. 여기서 **관리되고 있던**을 제가 강조표시 해놓았는데 Untracked파일은 관리하고 있던 파일이 아니므로 아무리 내용을 변경해도 Untracked -> Modified 와 같은 상태변화는 일어나지 않습니다. (나는 파일 내용변경을 했는데 왜 Modified상태가 안되는거죠? 라는 질문에 대한 답)

- Unmodified상태의 파일을 수정하면 아래 이미지와 같이 Modified상태가 됩니다. `git status` 명령어의 결과를 주의깊게 봅니다. 하지만 `Unstaged`상태 이기때문에 아직 commit을 할수 없습니다.

![unmodified]({{ site.url }}{{ site.baseurl }}/assets/images/2021-04-04-GitLifeCycle/modified.png)

- 위에서 보이듯이 Modified상태는 `Unstaged` 상태에서는 커밋을 할수 없으며 `git add` 명령어를 통해서 파일을 `Staged` 상태로 만들어주면 커밋이 가능합니다. 이처럼 `Modified`명령어의 커밋 가능여부는 파일이 `Staged`상태인지 `Unstaged`상태인지에 따라 달려있습니다. 아래는 `Unstaged`상태였던 파일을 `git add` 명령어를 통해 `Staged`상태로 변경시켜주었으며 이제 커밋할 준비가 됬습니다.

![unmodified]({{ site.url }}{{ site.baseurl }}/assets/images/2021-04-04-GitLifeCycle/stagedmodified.png)

### Staged

-  이제 변경된 코드내용을 저장해도 좋다는 상태입니다. 예를들어 내가 다른작업을 하고싶은데 일단 이 코드는 저장해놓고 다른작업을 진행하고싶을때 파일을 `Staged`상태로 만들어주고 커밋을 하고난 후 다른 작업을 시작합니다.

- Untracked/Modified 상태인 파일을 add 하면 `Staged`상태가 됩니다.

 Untracked게시물과 이번 게시물을 통해 이렇게 Git LifeCycle을 모두 알아보았습니다.

댓글 달아주시면 감사하겠습니다 :D





