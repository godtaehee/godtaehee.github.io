---
layout: post
title:  "Git 개요와 세팅"
subtitle: "Git의 기본 개요와 세팅에 대해서 알아봅니다."
categories: development
tags: git
comments: true
---

`rebase`란 몇개의 커밋들을 새로운 `base commit`으로 합치거나 이동시키는 과정입니다. 아래 설명들을 모두 보시고 나서 이 뜻을 다시 헤아려 보시면 이해가 잘 되실겁니다 아래 그림부터 보겠습니다.

![img-15](https://user-images.githubusercontent.com/44861205/124277854-7e86ea80-db80-11eb-9bfa-4ab62c6d6b72.png)


1.  위 그림은 `Release Branch`에 **Rcommit1**, **Rcommit2**, **Rcommit3** 총 3개의 커밋이 있는상황이고 `Feature Branch`는 **Fcommit1**, **Fcommit2** 이렇게 2개의 커밋이 있는 상황입니다.
2.  이때 Feature 브랜치는 Rcommit1만 존재했을때 만들어진 브랜치이며 `Rcommit1으로부터 만들어진 브랜치임에 주목`합니다.
3.  그리고 나서 feature 브랜치에 Fcommit1, Fcommit2 총 두개의 커밋이 추가되었고 저희의 목표는 Feature branch에 Release branch의 커밋 3개를 얻어오는 것입니다.
4.  이때 저희는 `rebase`를 사용할 수 있으며, 다음과 같은 명령어로 `rebase`를 할수 있습니다.

```
git checkout feature
git rebase release
```

# Rebasing

`rebase` 한다는 것은, Feature branch에 Release branch의 가장 최근 커밋인 Rcommit3과 그 자식커밋의 변경내용을 모두 적용시킨다는 뜻인데요 각각의 커밋을 하나씩 충돌검사를 하며 `새로운 커밋들을 만들어` 일자의 형태로 연결 시켜줍니다.

![img](https://user-images.githubusercontent.com/44861205/124277939-99595f00-db80-11eb-94b6-8da861265133.jpg)


## Step 1

1.  명령어를 입력하는 순간 Feature 브랜치의 루트 커밋(Fcommit1)은 Release 브랜치의 가장 최근 커밋(Rcommit3)을 가리키게 됩니다.
2.  이렇게 Feature브랜치는 기존의 Fcommit1과 Fcommit2를 버리고(실제로 버리진않고 이걸 활용하긴 함 쭉 읽어보세요) Rcommit1, Rcommit2, Rcommit3를 가지게 됩니다.
3.  이렇게 리베이스가 끝나는것일까요 ?? 당연히 아닙니다. Fcommit1, Fcommit2를 그대로 버린채로 둔다면 큰일나겠죠

## Step 2

1.  이제 깃은 Fcommit1을 Rcommit3 뒤에 추가하려고 하는데요 이말은 즉 Fcommit1의 부모커밋이 `Rcommit1에서 Rcommit3로 변경`이 된다는 말입니다. 아까 위에서 제가 주목해달라고 한 이유가 이것때문입니다. `부모커밋이 변경됬으니 새로만들 Fcommit1의 해쉬값이 변경`되게됩니다. 이 말은 즉, 새로운 커밋이 생성된다는 말이며, 커밋 메시지는 같습니다. 부모의 해쉬값이 달라졌기때문에 당연히 Fcommit1의 해쉬값도 달라지게 됩니다.
2.  만약 여기서 아무런 내용 충돌이 없다면 바로 Rcommit3뒤에 Fcommit1을 추가하게 됩니다. 이때 가장 중요한것은 지금의 Fcommit1의 해쉬값과 rebase를 진행하기 전의 Fcommit1의 해쉬값은 다릅니다.
3.  또한 위와 다르게 충돌이 발생한다면, 깃이 저희에게 이러한 내용을 알려줄것이고 저희는 이것을 직접 수동으로 충돌난 내용을 해결해야합니다. 충돌을 해결하고 나면 아래의 명령어를 통해서 `rebase`를 계속 해 나갑니다. 아래의 `git add`를 하게되면 staging 영역에 수정한 파일이 올라가게되고 git은 이 내용을 가지고 리베이스를 계속 진행하게 됩니다.
4.  `git add fixedfile // 충돌 난 파일을 add 해줌 git rebase --continue`

## Step 3

1.  이렇게 Fcommit1을 추가완료하면 깃은 Fcommit2를 추가하려고 합니다.
2.  Step2와 똑같이 충돌이 없다면 Fcommit1커밋 뒤에 Fcommit2를 추가해주고 rebase를 성공적으로 마칩니다.
3.  하지만 충돌내용이 생기면, 깃은 또 저희에게 알려줄 것이고 또 수동으로 저희가 내용수정을 해주어야합니다. 내용 수정완료후에는 위의 똑같은 명령어로 rebase를 진행시킵니다.
4.  이렇게 rebase가 모두 끝나게 되면 Feature branch는 Rcommit1, Rcommit2, Rcommit3, Fcommit1, Fcommit2를 가지게되는 브랜치가 됩니다.

실제로 제가 실험 해본 결과를 같이 보며 또 상기해봅시다.

![img-16](https://user-images.githubusercontent.com/44861205/124277969-a24a3080-db80-11eb-9e51-c3dabdfc94f1.png)


1.  위의 `git lg`(원래는 git log가 원래 명령어 입니다 저는 지금 alias를 사용했기때문에 커스터마이징 된 깃 명령어를 사용하고 있습니다.) 명령어를 통해 히스토리를 조회해봤을때 제일 위의 사진과 똑같은 상황을 연출해 보았습니다.
2.  Feature브랜치에 Release브랜치를 `rebase`하였으며 결과는 아래 `git lg`와 같습니다.
3.  이때 주의해서 볼것이 feature1 커밋(63e63b5)과 feature2 커밋(e81df68)인데요 `rebase`하기전의 이 커밋들의 값은 분명히 e261561과 9dc8d0b였습니다. 하지만 이 커밋은 parent commit이 initial commit일때의 해쉬값이며 `rebase`진행후에는 Release3을 parent commit으로 `rebase`를 진행했으므로 Fcommit1의 해쉬값이 바뀌었고 Fcommit1의 해쉬값이 바뀌었으니 당연히 Fcommit2의 입장에선 부모(Fcommit1)의 해쉬값이 바뀌었으니 자신의 해쉬값도 바뀌게 됩니다.

## 집고가면 좋을 내용들

1.  `rebase`와 `merge`둘다 깃에전 유용한 도구이지만 누가 더 좋고나쁘다는 없습니다.
2.  `merge`를 하실경우에는 새로운 `merge commmit`이 생기지만 `rebase`는 `merge commit`처럼 남는 커밋이 생기지 않습니다.