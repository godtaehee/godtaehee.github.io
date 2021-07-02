---
layout: post
title:  "renamed된 파일 add 되지않는 현상"
subtitle: "프로젝트 진행중 renamed관련한 이슈입니다."
categories: development
tags: git-issue
comments: true
---

프론트와 백엔드를 나누어 웹프로젝트를 진행중인데, 프론트 쪽에서 작업을 마치고 총 세분이 작업내용들을 합치려고 하는 상황중에 react의 component폴더를 한분은 Component라고 했고 나머지 두분은 component라고 하게되어 component로 통일을 하는 과정중에서 그냥 폴더의 이름을 component로 바꾸었는데 나의 예상은 `git status`로 상태를 확인했을때 Components하위에 있는 모든 파일과 폴더들이 `renamed`됬다고 뜰줄 알았는데 막상 디스코드 라이브로 상황을 봐보니 아래와 같이 staged되지 않았다는 상태를 보았다.

![img-33](https://user-images.githubusercontent.com/44861205/124295531-e98dec80-db93-11eb-8f1c-2c13a4aa27fa.png)


이미 Component에서 component로 바꾸었으니 Component 하위의 모든것들은 없는거나 마찬가지였으므로 `git add`가 되지 않았다. 그런데 실제로 존재하는 component폴더 하위의 모든것들은 정상적으로 add가 되었다. 이게 정상적인 결과인데 Component들이 왜 안됬는지는 원인은 찾지못했지만 해결은 하였다. 원인은 내가 더 깃에대해 잘 알게되면 회고하다가 풀릴꺼같다. 아래는 `git add .`을 적용한 결과이다.

![img-34](https://user-images.githubusercontent.com/44861205/124295588-f9a5cc00-db93-11eb-9ff4-f8b38746c3ea.png)

별 방법으로도 안되길래 결국 해결을 못하다가 오픈톡을 통해서 해결하게 되었는데 그 해결과정에서 많은걸 배웠다.

바로 `git rm`과 `git stash`인데 `git rm`은 따로 포스팅으로 준비하고 stash도 따로 공부를 할껀데

`stash`를 사용하다가 저렇게 말을 안듣는 변경사항들을 강제로 버려버리는데 사용할수 있지않을까 생각하게 되었는데 다른 대안들이 두개가 있었다.

바로 `git reset --hard`와 `git clean -fd`인데 근데 두개다 오류 해결의 직접적인 해결책이 되진 않았다.

# git rm --cached

이 명령어는 실제로 로컬에서 삭제하지는 않지만 Git저장소에서 삭제를한다. 이 말은 원격 저장소에는 파일을 삭제하지만 로컬에서는 삭제하지 않으며 git의 track대상에서 제외시킨다.

![img-35](https://user-images.githubusercontent.com/44861205/124295614-01fe0700-db94-11eb-9ee1-43799e814e3b.png)

실제로 위 사진을 보면 나의 깃헙 `test`레포를 클론 받아와서 `wqefwqe`라는 파일(파일 이름 아무 의미없음 그냥 변경사항 빨리 만들려다 보니..)을 `git rm --cached`를 통해 지웠는데 이 파일은 deleted상태이면서 Untrack상태이며 이것을 커밋후 푸시를 진행하게 되면 아래와 같이 원격에서 실제로 저 파일이 없는것을 확인 할수 있다.

![img-36](https://user-images.githubusercontent.com/44861205/124295666-13471380-db9

그렇다면 Component폴더에 있는 파일은 실제로 로컬에도 존재하지않는데 이상한 변경사항을 만들어내고 있으므로 `git rm --cached`로 깃의 추적대상에서 제외 시킬뿐만아니라 만약 폴더와 파일이 존재했다면 다시한번 삭제해주고 Untrack상태로 만들어 주었을 것이다.

![img-37](https://user-images.githubusercontent.com/44861205/124295674-14784080-db94-11eb-8a8a-99bfd29ab2ba.png)

위의 사진은 절대 안움직이던 Component폴더 하위의 모든것들이 정상적으로 삭제되었다는 이력이고 이것을 커밋한후 실제로 아래와 같이 실제 로컬에 존재하는 파일만 깃이 추적하게 만들수 있었다.

![img-38](https://user-images.githubusercontent.com/44861205/124295683-16da9a80-db94-11eb-88b1-cd2c05c5cbdb.png)
