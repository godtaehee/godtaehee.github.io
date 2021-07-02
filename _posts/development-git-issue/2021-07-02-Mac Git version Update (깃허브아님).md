---
layout: post
title:  "Unmodified, Modified, Staged"
subtitle: "Git LifeCycle인 Unmodified, Modified, Staged에 대해서 알아봅니다."
categories: development
tags: git-issue
comments: true
---

## ✅ 상황

깃헙 블로그 테마 jekyll-theme-chirpy 를 정상 구축했는걸로 생각했는데 \_posts폴더에 포스팅을 하자마자 깃헙 레포의 빌드에 실패했다. 7일이 넘게 그리고 수십번 저 테마를 가지고 블로그를 구축해보았지만 실패하여 그냥 테마를 다른것을 쓰기위해 기존의 나의 레포에 있던 jekyll-theme-chripy 파일들을 모두 지웠고 심지어 레포까지 지웠는데도 여전히 크롬으로 godtahee.github.io 블로그를 들어갔을때 살아있었다. 근데 이상한 점은 아이패드나 다른 휴대폰 기기 부모님꺼 핸드폰 기기, 친구들 몇명한테 부탁해서 저 사이트를 들어갔을땐 404(페이지를 찾을수 없음) 에러가 뜬것이다.

![img-12](https://user-images.githubusercontent.com/44861205/124283907-69618a00-db87-11eb-9fa5-5256117bff30.png)


### ✅ 해결하고  싶은것


크롬으로 들어가지는 나의 기존 블로그가 없어져야 다음작업(새로운 테마로 블로그를 구축하는작업)을 할텐데 저기서 막혀버려서 아무것도 진행할수없었다.

### ✅ 해결과정


위에서 밑줄친부분의 이상한점을 잘 생각해보면 내 PC에서만 접속가능한거지 다른 기기로 들어가면 페이지를 찾을수없음(404)오류가 뜬다는건데 질문하기 전까지만해도 이것이 [웹 캐시](https://hahahoho5915.tistory.com/33)때문인지 전혀 몰랐다.

물어볼곳이 없어서 오픈톡에 들어가서 물어보게됬는데 부방장님께서 캐시에대한 내용을 언급해주셨다.

내가 주소창에 블로그의 주소(godtaehee.github.io)를 치고 엔터를 누르는행위가 곧 깃허브 서버에 요청(request)를 보내는것이고 요청을 받은 서버는 자신에게 저장된 블로그 파일들을 찾아서 다시 나에게 응답(response)해준다.

![img-13](https://user-images.githubusercontent.com/44861205/124283964-77afa600-db87-11eb-80e0-0fe5b66fea72.png)


보통 웹서버와 클라이언트의 상호작용은 이렇게 일어나는데 이러한 요청과 응답이 빈번해지면 서버가 부하되어 다른 클라이언트에게도 응답(response)해주는 시간이 길어질수 있으며 트래픽이 초과 될수있다.

따라서 웹 캐시라는것을 이용해 **자주 요청받는 파일은 특정 위치에 복사본을 저장해놓고 클라이언트는 서버에서 원하는 블로그파일을 받는것이 아닌 캐시에 저장해두었던 파일을 가지고 서비스를 받게된다.**

부하를 줄여주고 트래픽을 감소시켜 전바적인 성능효과가 있지만 웹개발을 하는 사람들에게는 오히려 이게 안좋을수있다.

예를 들어보면 html, css를 가지고 웹사이트를 개발하고있을때 css의 적용이 마음에 안들어서 페이지를 빈번하게 새로고침 해오는 경우가 많다.

그런데 css를  수정하고 새로고침을 해도 서버에서 새로운 css(수정된  새로운 버전의 css)를 받아오는것이 아닌 **캐시에 저장된 이미 있는(수정하기 전의 기존 css)css파일만을 계속 받아오므로** 나는 분명히 css를 수정했는데 웹페이지에 css 적용이 안되는현상을 겪을수 있다.

그렇다고 빨간색 오류가 나는것도 아니기 때문에 이는 정말 찾기 어려운 오류가 된다. (나는 꼬박 일주일을 해결방법을 못찾음)

![img-14](https://user-images.githubusercontent.com/44861205/124283995-81d1a480-db87-11eb-85ea-68501604c1c7.png)


### ✅ 해결방안

두가지의 방안이 있는데 개발자 도구를 통해 해결하는 방법과 강력한 새로고침을 통해  해결하는 방법이 있다.

#### 1\. 개발자 도구

<img width="288" alt="img-15" src="https://user-images.githubusercontent.com/44861205/124284031-88f8b280-db87-11eb-934d-e0bb78934efe.png">


![img-16](https://user-images.githubusercontent.com/44861205/124284058-90b85700-db87-11eb-8bcd-0445fe73cd3d.png)

![img-17](https://user-images.githubusercontent.com/44861205/124284100-9b72ec00-db87-11eb-8617-ec8f77b70cd3.png)



저렇게 직접 캐시를 Disable을 해주게 되면 앞으로는 캐시에서 요청한 파일을 가져오는 것이 아닌 서버에서 매번 새로운(따끈따끈한) 파일을 받아올수 있다.

#### 2\. 강력한 새로고침

이름이 조금 웃긴데, 우리가 자주 사용하는 새로고침과는 다르게 **강력한 새로고침**은 **캐시를 삭제하고 새로고침**을 해준다. 즉 우리가 해결해야하는것이 결국 캐시 이기때문에 2가지 방법이 모두 통한다

개발자 도구를 연상태로 새로고침 주소창 바로왼쪽의 새로고침 표시를 우클릭  하면 강력한 새로고침을 할수있다.

나는 맥을 쓰므로 **Command + Shift + R**이라는 단축키를 통해 강력한 새로고침을 해준다.

윈도우는 **ctrl + shift + r** 이며이러한 오류를 겪고나서 나는 무조건 강력한 새로고침으로 하는 버릇이 생겼다.

### 참고

[HTTP Request  & HTTP Response](https://joshua1988.github.io/web-development/http-part1/)

[웹  캐시](https://hahahoho5915.tistory.com/33)

[강력한 새로고침](https://fimtrus.tistory.com/entry/Chrome-%ED%81%AC%EB%A1%AC-%EA%B0%95%EB%A0%A5%ED%95%9C-%EC%83%88%EB%A1%9C-%EA%B3%A0%EC%B9%A8-%EB%8B%A8%EC%B6%95%ED%82%A4-%ED%81%B4%EB%A6%AC%EC%96%B4-%EC%BA%90%EC%8B%9C)