---
layout: post
title:  "git hash-object -w --stdin의 --stdin옵션에 대하여.."
subtitle: "git hash-object 의 특정 옵션에 대해서 알아봅니다."
categories: legacy
tags: git
comments: true
---
[이전글](https://godtaehee.tistory.com/37)에서 글을 포스팅하다가 hash-object의 `--stdin`옵션이 정확히 어떤건지를 잘 모르겠어서 구글링과 오픈톡에 물어봤지만 원하는 답을 찾지 못했다.

이전글의 포스팅을 끝마칠때쯔음 [git-scm: Git의내부-Git개체](https://git-scm.com/book/ko/v2/Git%EC%9D%98-%EB%82%B4%EB%B6%80-Git-%EA%B0%9C%EC%B2%B4) 두번째 예시에서 echo의 결과값이 아닌 이제 파일을 가지고 하는예제를 진행하다가 `파일은 --stdin옵션을 붙히지 않는데 착안`해서 뭔가 알거같은 느낌이 들어 여러가지 재밌는 실험들을 해보았다.

![img-10](https://user-images.githubusercontent.com/44861205/124282924-51d5d180-db86-11eb-82cd-688812fbc9e8.png)


이렇게 아예 test.txt라는 파일을 생성하고나서 hash-object 명령어를 사용할때 --stdin옵션을 사용하지 않는것을 알수있었다.

# 첫번째, 폴더가 아닌 파일

이 글의 마지막 `이 옵션이 없으면 파일 경로를 알려줘야 한다`를 계속해서 읽어보니 처음에 나는 `objects`폴더에 저장을 못하는줄 알고 이 폴더와 관련이 있다고 생각을 했는데 폴더가 아니라 파일 경로라는것에 집중을 해보니 뭔가 실마리가 나왔다.

```
echo 'test content' | git hash-object -w --stdin
```

이 명령어 자체가 우선 echo의 결과물인 test content라는 내용을 git 의 hash-object로 git 데이터베이스에 저장을 하겠다는 건데 생각을 해보니 echo의 결과물은 파일이 아니라 그냥 출력물일뿐이었다.

git-scm에서 예시로 든 저 명령어에만 집중을 하니 `파일을 hash-object`로써 저장할수는 없을까? 라는 생각을 하지 못했고 그래서 해결책을 찾지 못하고 있다가 파이프 라인(|)과 stdin과의 관계를 검색해보았다.

# 두번째, 파이프라인에 대하여

파이프 라인이란 어떠한 프로그램의 출력 결과를 다른 프로그램의 입력 값으로 사용할때 사용하는 특수문자이다.

그러면 다시 위의 명령어를 아래에서 보자

```
echo 'test content' | git hash-object -w --stdin
```

`파이프라인을 기준으로 왼쪽 명령의 stdout에서 나온 결과를 파이프 라인 기준 오른쪽의 명령어의 stdin으로 사용하겠다는 뜻이다.`

이제 모든 의문점이 해결되었다.

stdout에서 나온 결과는 `test content`이며 이것을 git hash-object의 --stdin옵션으로 받아 hash-object로 만들어 주는것이다.

# 그러면 --stdin옵션 말고 파일로 해보면??

우선 첫번째로 stdin option이라는 내용을 가지는 test.txt파일을 만들어서 hash-object로 등록과 출력을 해보았다. 최대한 처음의 예제를 따라하기위해서 echo도 넣어봤는데 --stdin옵션이 빠진것을 확인할수 있다.

그런데 결과가 `stdin option`이라는 내용으로 나오는것을 확인할수 있으며 이 결과를 통해서 우리는 `--stdin`옵션이 필수가 아니라는것을 알수 있으며 다시 생각해보니 옵션인데 내가 왜 필수라고 생각을 했었는지 의문이다. 즉 여기서는 echo의 결과값이 git hash-object의 stdin으로 들어왔지만 --stdin옵션이 없기때문에 stdin에 있는 test라는 내용을 hash-object를 만드는데 사용하지 않았으며 test.txt라는 파일을 가지고 hash-object를 생성했다.

<img width="423" alt="img-11" src="https://user-images.githubusercontent.com/44861205/124282963-58644900-db86-11eb-8ae4-b769e81f96ff.png">


그러면 이제는 echo를 빼고 사용해보자

echo를 빼면 당연히 같은 결과가 나왔다. 이렇게 오늘 공부한 hash-object의 모든 의문이 풀렸다!