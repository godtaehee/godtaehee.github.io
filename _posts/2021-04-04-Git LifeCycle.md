---
title:  "Git LifeCycle - Untracked (1) "
excerpt: "Git이 파일들을 어떤 Cycle을 통해 관리하는지 알아봅니다."
layout: single
classes: wide
categories:
  - Git
tags:
  - Git, Github
---

# Git LifeCycle - Untracked
Git을 자유자재로 다루기 위한 제일 기초가되며 제일 중요한 개념입니다. 이 개념을 숙지하지 않은채 `git add`, `git push`, `git clone`, `git commit`과 같은 명령어를 사용하면 여러분의 소중한 파일의 데이터들이 손실될수 있습니다. 저는 이 개념을 글로도 읽긴했지만 아무렇게나 사용할수있는 폴더와 아무렇게나 사용할수있는 파일들을 가지고 위의 명령어들을 쳐보면서 익혔습니다. 이 방법이 글로만 보는것보다는 확실히 개념을 더욱 쉽고 깊게 이해할수 있습니다.

![Git LifeCycle]({{ site.url }}{{ site.baseurl }}/assets/images/2021-04-03-GitLifeCycle/lifecycle.png)

위의 사진은 [링크](https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository) 에서 보실수 있으며 git-scm에 공식적으로 올라와있는 Git LifeCycle을 이해하기 위한 아주 좋은 이미지 입니다. 위의 사진을 이제부터 설명 하겠습니다.

### Untracked
- Untracked 파일은 Git이 아직 관리하지 않는 파일입니다. `git status`명령어를 통해 Untracked 파일을 확인할수 있습니다.
  
![untracked]({{ site.url }}{{ site.baseurl }}/assets/images/2021-04-03-GitLifeCycle/untrack.png)

- 따라서 Git은 이 파일이 있던지 말던지 상관쓰지 않습니다. 즉 깃의 기능인 Version Control을 하지 않습니다. 이와 관련해 아래 사진을 보면 `Untracked File` 이라는 내용의 `file1.txt`이 있는데 제가 이 파일의 내용을 `Untracked File Modiffy Untracked File`로 수정을 하고 `git status` 명령어로 상태를 확인해 보았는데 여전히 `Untracked files` 인것을 확인할수 있습니다. 이처럼 Git은 `Untracked file`의 **내용이 변경이 되던말든 신경쓰지 않습니다**.

![untracked]({{ site.url }}{{ site.baseurl }}/assets/images/2021-04-03-GitLifeCycle/modify.png)

- `git add`명령어를 통해 Untracked파일을 Git이 관리하는 파일로 만들수 있습니다. 아래와 같이 `git add `명령어를 통해 file1.txt의 상태가 `Changes to be commited` 상태로 변한것을 확인할수 있습니다. **Commit**을 할 준비가 되었다는 상태이며 Commit은 추후에 배우겠습니다.
  
![untracked]({{ site.url }}{{ site.baseurl }}/assets/images/2021-04-03-GitLifeCycle/add.png)

- 만약 file1.txt파일을 삭제하면 어떻게 될까요 ? `rm`명령어를 통해 file1.txt파일을 삭제해보겠습니다.

![untracked]({{ site.url }}{{ site.baseurl }}/assets/images/2021-04-03-GitLifeCycle/rm.png)

- 위의 사진은 삭제 직후의 사진인데 `Changes to be committed`는 아까 `Untracked`파일이였던 file1.txt를 Stage에 올려놓고 아직 커밋하지않았기때문에 남아있는 상태이며, 밑에 빨간색으로 file1.txt가 deleted 됬다고 알려주고 있습니다. 이렇게 파일이 삭제되는것도 **변경사항**이므로 Git은 방금전 파일삭제를 감지하고 우리에게 파일이 삭제되었다고 친절하게 알려줬습니다.

![untracked]({{ site.url }}{{ site.baseurl }}/assets/images/2021-04-03-GitLifeCycle/add.png)

- 저는 우선 Staged상태였던 file1.txt를 커밋하였고 **파일이 삭제되었다는 변경사항**은 Staged 상태가 아니므로 당연히 `Commit`되지 않았습니다.

- `git restore`명령어를 통해 다시 파일삭제했던 것을 아래 이미지와 같이 복구할수 있습니다.

![untracked]({{ site.url }}{{ site.baseurl }}/assets/images/2021-04-03-GitLifeCycle/restore.png)

- restore했던 파일을 다시 삭제하고 이번엔 **삭제됬다는변경내용**을 `git add`및 `git commit`을 해보겠습니다.

![untracked]({{ site.url }}{{ site.baseurl }}/assets/images/2021-04-03-GitLifeCycle/realrm.png)

- 위으 사진에서 주의깊게 봐야할 것은 처음엔 `Changes not staged for commit`상태였던 파일이 `Changes to be committed` 상태로 변했다는 점 입니다. 그냥 `rm`을 통해서 파일삭제만 하면 `Unstaged`상태가 되며 커밋을 하기위해서 `Staged`상태로 바꿔줘야 합니다.

이렇게 Git-LifeCycle중 Untracked상태를 알아보았습니다. 원래는 다른 상태들도 다루려고 했는데 생각보다 길어져서 이만 끊고 다른 게시글에서 나머지 상태들을 다루겠습니다.

다음 게시글은 Untracked외의 나머지 Git LifeCycle을 다루겠습니다.

[여기](https://wonderfulhuman.github.io/git/Git-LifeCycle/) 에서 볼수있습니다.

댓글 부탁드립니다 :D