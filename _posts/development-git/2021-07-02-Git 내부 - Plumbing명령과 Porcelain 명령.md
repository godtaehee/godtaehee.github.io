---
layout: post
title:  "Git 내부 - Plumbing명령과 Porcelain 명령"
subtitle: "Git 내부의 저수준 명령어들을 알아봅니다."
categories: development
tags: git
comments: true
---

`Plumbing`명령어는 저수준의 명령어이며 좀 더 사용자에게 친숙한 사용자용 명령어는 `Porcelain`명령어라고 부른다.

우리가 주로 사용했던 `git checkout`, `git branch`, `git remote`는 Porcelain 명령어라고 부른다.

새로만든 디렉토리나 이미 파일이 있는 디렉토리에서 `git init`명령을 실행하면 Git은 데이터를 저장하고 관리하는 `.git` 디렉토리를 만든다. 이 디렉토리를 복사하기만 해도 저장소가 백업 된다. 이게 무슨말이냐면 .git폴더만 가지고 있다면 언제든지 나의 파일을 가지고있는 저장소를 만들수 있다는것이다. 빈 폴더에 복원하고싶은 `.git`폴더를 복사해놓으면 그걸로 파일 복구는 끝이다. 실제로 해보길 바란다.

`.git`폴더는 다음과 같은 구조를 가진다.

![img-48](https://user-images.githubusercontent.com/44861205/124280446-a75caf00-db83-11eb-8b4b-28f931c62dbd.png)


HEAD와 동일한 계층에 있는 폴더혹은 파일 이 git init후 최초의 파일과 폴더이며 그 아래 또 많은 파일 혹은 폴더가 있는것을 확인할수 있다. 깃은 `.git`폴더의 이러한 계층구조를 바탕으로 버전관리를 하고있었던 것이다.

`description`파일은 기본적으로 GitWeb 프로그램에서만 사용하기 때문에 신경쓰지 않아도 된다.

`config`파일에는 해당 프로젝트에만 적용되는 설정 옵션이 들어 있다.

`info`디렉토리는 `.gitignore`파일처럼 무시할 파일의 패턴을 적어 두는 곳이다.

하지만 `.gitignore` 파일과는 달리 Git으로 관리되지 않는다.

`hook` 디렉토리에는 클라이언트 훅이나 서버 훅이 위치한다. 자세한 내용은 [Git Hooks](http://git-scm.com/book/ko/v2/Git%EB%A7%9E%EC%B6%A4-Git-Hooks#_git_hooks)에서 설명한다.

이제 남은 네 가지 항목(HEAD, index, objects, refs)은 모두 중요한 항목이다.

`objects` 디렉토리는 모든 컨텐트를 저장하는 데이터베이스이고

`refs`디렉토리에는 커밋 개체의 포인터(브랜치, 태그, 리모트 등)을 저장한다.

`HEAD`파일은 현재 Checkout 한 브랜치를 가리키고 `index`파일은 Staging Area의 정보를 저장한다.( git-scm에서 Staging Area를 index라고 부르는 이유이다.)