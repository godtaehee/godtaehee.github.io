---
layout: post
title:  "GitBash Push가 안되는 현상"
subtitle: "정말 많이 일어나는 Issue인 push 멈춤현상"
categories: legacy
tags: git-issue
comments: true
---

내가 활동하는 [깃오픈톡](https://open.kakao.com/o/gzO2bFGc)에서 GitBash가 push가 안되고 계속 멈춰있는 이슈가 있었는데 이번으로 두번째 이슈가 됬고 처음으로 이 이슈가 일어났을때 결국 해결을 못하고 폴더를 다 지웠다가 다시 해보니 됬다는 만족하지 못하는 결론을 얻게되었다.

하지만 내가 정말 열심히 소중히 갈고 닦은 프로젝트인데 폴더를 다 지웠다가 쉽게 복구도 못할 뿐더러 잘못하다가 데이터 손실이라도 난다면 이건 정말 큰일날 일이다.

그래서 위의 방법으로는 임시방편일뿐 절대 추천하지 않는 방법이다.

이상하게 클론은 잘 되는데 push가 안되고 그냥 멈춰있다 오류도 안뜨고 그냥 멈춰있다.

클론은 되고 푸시만 된다는점에 착안에 인증(Authentication)에 문제가 있는것 같았다.

그래서 `git config --list`를 통해서 `user.name`, `user.email`이 정상적으로 되어있는지 확인해 보았으며 그것도 역시 잘 되어있었다.

리모트도 잘 설정되어있었고...

push가 안되는 현상을 해결하기위해 썼던 모든 방법들을 나열해 보면

1.  한글을 포함한 폴더에 git 폴더를 만든경우
2.  리모트가 설정이 안된경우
3.  push를 한번도 해본적이 없으면서 git push origin master까지 해줘야하는데 git push만 한경우
4.  방화벽이 막고있는 경우
5.  인터넷이 연결이 안된경우

5가지의 방법이 모두 안된다면 마지막으로 소개할 해결방법은

`Cmd`창, `bash`창 `Terminal`창 즉, 모든 컴퓨터에 기본적으로 깔려있는 기본 쉘에서

`git --version`혹은 `where git` 등등의 명령어로 깃이 컴퓨터에 설치되어있는지부터 확인하고

![img-41](https://user-images.githubusercontent.com/44861205/124296290-c0219080-db94-11eb-9d46-b7820f407c0e.png)


만약 이러한 반응이 아니라 `git`을 찾을수 없다 라는 뉘앙스의 오류메시지가 출력되면 우선 깃부터 설치하자

그러고 나서 `git push`를 하고나면 Web Authentication 혹은 Access Token으로 내가 푸시를 하려는 깃헙의 레포의 `push`를 할수있는 권한이 있는지를 인증할수 있다.

`Web Authentication`을 선택하면 자동으로 깃헙 로그인페이지가 나타나며 그대로 초록색 버튼을 클릭을해서 인증을 진행하면 된다.

또한 `Access Token`을 선택한다면 깃헙에서 `Access Token`을 발급받아 인증을 직접 거쳐줘야한다. 구글링하면 방법 정말 많이 나온다.

나 또한 추후 `Access Token`을 다루겠다.

우선 푸시가 되야하니까 처음은 Web Authentication으로 진행을 하게되면 푸시가 잘되는것을 확인할수 있으며 기본 쉘창에서 `push`를 성공하면 그제서

야 `git bash`의 push도 `성공적으로 되는것`을 확인할수 있을 것이다.

`Web Authentication`은 `Access Token`을 발급받고 이러한 과정이 필요없이 클릭 몇번으로 인증을 진행시킬수 있지만 `push`하는 빈도가 많으면

많을수록 매번 인증을 진행해야하며 이것은 상당히 귀찮은 작업이다.

때문에 Access Token인증을 통해 한번 진행해놓는것을 추천하며 Access Token으로 인증한 계정에 자유롭게 푸시를 할수 있게될것이다.