---
layout: post
title:  "Bare-repository and Work-repository"
subtitle: "Bare-repository와 Work-repository에 대해서 알아봅니다."
categories: development
tags: git
comments: true
---

[Git 서버 - 프로토콜](https://git-scm.com/book/ko/v2/Git-%EC%84%9C%EB%B2%84-%ED%94%84%EB%A1%9C%ED%86%A0%EC%BD%9C)을 공부하다가 `Bare 저장소`라는 말이 계속나와서 처음에는 그냥넘기려다가 자꾸나와서 이해가 점점 안되길래 구글링을해서 공부를 해보았습니다. 보통 깃관련 구글링을 해보면 git-scm의 내용을 그대로 적은 결과만 많이 보게됬는데 [여기](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true&blogId=srup&logNo=60167624633)에는 직접 실습을 통해서 Bare-repository와 Work-repository의 차이를 알려주어서 이해가 쉽게되었습니다. 그럼 저랑도 같이 실습을 해보시죠.

# 시작하기

먼저 아무것도 없는 빈 폴더를 만들어서 `cd`명령어를 통해 그 폴더로 들어갑니다.

![img-17](https://user-images.githubusercontent.com/44861205/124278258-010faa00-db81-11eb-97c3-4de6972ab616.png)


그런다음 project라는 폴더를 하나만들어 거기에 README를 하나 추가해 줍니다.

그런다음 project 폴더에 들어가서 git init으로 깃 저장소를 만들어주면 현재 Untracked files로 README파일 하나가 있습니다.

이것을 `git add` 해주고 `git commit`까지 해줍니다.

# bare-repository와 non-bare-repository 만들어보기

![img-18](https://user-images.githubusercontent.com/44861205/124278296-0f5dc600-db81-11eb-95f1-c756292577c9.png)


위의 과정을 다 마쳤다면 `cd ..`으로 상위폴더로 이동후 클론을 총 2번 해줄건데 한번은 `--bare옵션을 추가`하여 클론해주고 다른 한번은 그냥 `아무런 옵션이 없이` clone하려고 한다.

```
git clone --bare project/ bare.git
git clone project/ non-bare.git
```

위의 명령어도 중요하지만 더 중요한 것은 각각 `ls`명령어로 목록을 확인해봤을때의 차이점이 더 중요하다.

`non-bare.git`은 `ls -a`명령어로 숨김폴더까지 모두 출력하게 했는데 이유는 혹시 .git파일이 있을까 해서인데 역시 있었다.

차이점이 무엇인지 아시겠나요 ? 차례로

```
git clone --bare project/ bare.git
ls bare.git/
```

위의 명령어는 `bare-repository를 만들고 그 내용을 확인`하는 것이며

```
ls non-bare.git/
```

위의 명령어는 `non-bare-repository를 만들고 그 내용을 확인`하는 것이다.

bare.git 디렉토리는 respository에 대한 정보를 갖고있는 파일들로 구성되어 있지만 non-bare.git 디렉토리는 순수하게 원본을 복사한 것과 같다.

즉, bare.git 디렉토리는 `git init`으로 생성한 `.git`폴더와 폴더 구조가 상당히 유사하다. 하지만 non-bare.git 디렉토리는 .git폴더를 포함한 우리가 실제로 작업을 하는 `work-repository`이다. 차이점을 알겠나요 ?

위의 예제는 로컬에서 실험해봤으니 이제 깃헙을 대상으로도 실험을 해보면

![img-19](https://user-images.githubusercontent.com/44861205/124278336-197fc480-db81-11eb-9c32-88afab2bb04d.png)


아무 빈 폴더를 또 만들어서

clone을 총 두번해보았는데 깃헙에 있는 내 test레포를 대상으로 진행해 보았다.

두 clone의 차이점은 `--bare`옵션을 붙혔는지 안 붙혔는지의 차이이다.

그런데 둘의 폴더 내부의 구조를 보면 확연히 다르다.

`test`폴더는 실제 파일들이 존재하지만 `test.git`폴더는 실제 파일들은 존재하지 않고 변경사항, 이력등의 `history`의 정보들을 저장합니다.

그럼 이력들만 존재하는 `test.git`파일은 무슨 쓸모가 있을까 ? `test.git`파일은 이력들만 저장된 폴더이므로 우선 실제 파일들이 존재하는 `test`폴더보다 가볍습니다. 그래서 `test.git`파일만 공유하면 이력뿐만아니라 이것을 클론해서 실제 파일들을 다시 `clone`해올수 있습니다. 한번 해볼까요?

![img-20](https://user-images.githubusercontent.com/44861205/124278367-20a6d280-db81-11eb-9a2b-4e29cb01f0ed.png)


`test`폴더와 `test.git`폴더가 존재하지 않는 아예 새로운 폴더를 만들고 이동하여 `git clone ~/arsd/test.git`을 통해 방금 만든 `bare repository`를 `--bare`옵션 없이 `clone`해보았습니다. 그랬더니 `test`폴더가 생기고 내용을 확인해보니 `test`폴더와 똑같은 파일들을 `clone`해올수 있었습니다. 실제로 `github`에 `push`할수 있는 파일의 용량은 정해져 있으며 그다지 크지 않습니다. 그렇다보니 깃으로 관리하는 프로젝트의 용량이 커지면 커질수록 기껏 열심히 개발해놨는데 github에 push하지 못하는 상황이 오게됩니다.

이와 비슷한게 `package.json`이 있겠습니다. node.js 프로젝트를 진행할때 node\_modules폴더에 담긴 패키지 리소스들이 너무 많고 용량도 많다보니 보통 어떠한 패키지를 이 프로젝트에서 사용했는지를 더불어 프로젝트의 전반적인 정보를 담고있는 package.json을 배포하게 됩니다.

이것은 json형태의 자바스크립트이기때문에 용량이 상당히 작으며 다른사용자들이 내 node 프로젝트를 사용하기위해서는 내가 사용한 패키지들을 다운받아야 하는데 패키지 리소스를 직접받는게 아닌 package.json에 적어둔 내용을 바탕으로 `npm i`라는 명령어를 통해 `npm`이 알아서 `package.json`에 있는 패키지들을 다운받아 줍니다.

이처럼 실제 프로젝트에 존재하는 프로젝트 리소스들은 용량이 클수있으므로 상당히 가벼운 `bare-repository`같은 폴더 혹은 `package.json`같은 파일만 있으면 그 후에 프로젝트 개발자가 아닌 사용자들은 이 파일이나 폴더를 통해 실제 리소스를 사용할수 있습니다. 물론 개발자도 나중에 패키지의 내용이 손실됬다하더라도 이 `bare-repository`혹은 `package.json`을 통해 쉽게 패키지의 내용을 다시 복구할수 있습니다.