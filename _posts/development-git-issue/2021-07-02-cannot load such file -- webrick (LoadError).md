---
layout: post
title:  "cannot load such file -- webrick (LoadError)"
subtitle: "webrick 오류에 대해서 알아봅니다."
categories: legacy
tags: git-issue
comments: true
---

![img-18](https://user-images.githubusercontent.com/44861205/124284806-44214b80-db88-11eb-8d65-15de627a8e2b.png)


깃허브 블로그 구축을 하다가 혹시 `cannot load such file -- webrick (LoadError)` 이러한 오류가 났다면 `bundle add webrick` 명령어로 해결할수 있는데 오류가 나는 이유는 다음과 같다.

[링크](https://www.ruby-lang.org/en/news/2020/12/25/ruby-3-0-0-released/) Ruby 3.0.0부터 `webrick`은 더이상 지원하지 않는 라이브러리이므로 그에 상응하는 다른 gems를 설치해라고 나와있기 때문이다.

내가 도움을 받았던 곳은 [이곳](https://github.com/jekyll/jekyll/issues/8523)이다.

질문을 조금만 읽어보면 바로 알수있다

`webrick`말고도 지원하지않는 라이브러리가 더 있으니 혹시 같은 오류인데 라이브러리 이름만 다르다면 위의 명령어를 통해 알맞는 라이브러리를 설치하면 될것이다.