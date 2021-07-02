---
layout: post
title:  "Lexical Environment (렉시컬 환경) - 변수"
subtitle: "Lexical Environment에 대해서 알아봅니다."
categories: development
tags: ecma-script
comments: true
---

**이 글에 나오는 용어를 한글로 번역하게되면 어색해져서 최초언급할때 빼고는 모두 영어로 용어를 칭하겠습니다.**

자바스크립트에선 실행 중인 함수, 코드 블록 `{...}`, 스크립트 전체는 \_Lexical Environment\_이라 불리는

_internal hidden associated object_(내부 숨김 연관 객체)를 갖습니다.

Lexical Environment 객체는 두 부분으로 구성되는데요

<img width="407" alt="img-42" src="https://user-images.githubusercontent.com/44861205/124297251-d7ad4900-db95-11eb-98f1-3498405bc610.png">

1.  **Environment Record** (환경 레코드) : 모든 지역 변수를 프로퍼티로 저장하고 있는 객체입니다. `this`값과 같은 기타 정보도 여기에 저장됩니다.
2.  **Outer Lexical Environment**(외부 렉시컬 환경): 자신의 스코프를 제외한 외부 코드와 연관되어있습니다.

**이렇듯 우리가 평소에 알던 '변수'는 특수 내부 객체인 Environment Record의 프로퍼티 일 뿐입니다.**  
**'변수를 가져오거나 변경' 하는것은 '환경 레코드의 프로퍼티를 가져오거나 변경'함을 의미합니다.**

아래 두 줄까리 코드엔 렉시컬 환경이 하나만 존재합니다.

<img width="711" alt="img-43" src="https://user-images.githubusercontent.com/44861205/124297254-d9770c80-db95-11eb-9101-6af254903f20.png">

이렇게 test.js라는 자바스크립트 전체와 관련된 Lexical Environment는 **global Lexical Environment**(전역 렉시컬 환경)이라고 합니다.

global Lexical Environment는 어떠한 중괄호로도 감싸져있지 않기때문에 제일 넓은 범위의 스코프를 가지며 **Outer Lexical Environment**가 `null`입니다.

[원문](https://ko.javascript.info/closure)에는 test.js라는게 존재하지않습니다 하지만 제가 이 글을 읽으며 공부할때 처음에 코드만 덩그러니

있길래 이게 global Lexical Environment인지 한눈에 파악이 안됬어서 그림 좀 편집해서 올렸습니다.

앞으로의 이미지들은 test.js안에 있는 코드라고 봐주시면 감사하겠습니다.

![img-44](https://user-images.githubusercontent.com/44861205/124297257-da0fa300-db95-11eb-9007-683c7a342501.png)

왼쪽은 코드, 오른쪽은 코드가 한 줄, 한 줄 실행될 때마다 global Lexical Environment가 어떻게 변화하는지 보여줍니다.

1.  스크립트가 시작되면 스크립트 내에서 선언한 변수 전체가 Lexical Environment에 올라갑니다 (pre-populated)  
    1-1. 이때 변수의 상태는 special internal state(특수 내부 상태)인 **'uninitialized'**가 됩니다. 자바스크립트 엔진은 'uninitialized' 상태의 변수를 인지하긴 하지만, `let`을 만나기 전까진 이 변수를 참조할 수 없습니다. (후에 다루겠지만 함수 선언식으로 만들어진 함수는 **'uninitialized'** 상태가 아니라 선언과 동시에 할당됩니다.)
2.  `let phrase`가 나타났지만 아직 값을 할당하기 전이기 때문에 프로퍼티 값은 `undefined`입니다. `phrase`는 이 시점 이후부터 사용할 수있습니다. 즉, undefined라고 사용을 못하는게아니라 undefined로써 사용가능합니다 후에 phrase에 우리가 원하는 값을 넣어서 자유롭게 사용할수 있다는 말입니다.
3.  phrase에 문자열 "Hello"라는 값이 할당되었습니다.
4.  phrase이 문자열 "Bye"라는 값으로 변경되었습니다.

지금까지 배운 내용을 요약해 보면

1.  변수는 특수 내부 객체인 환경 레코드의 프로퍼티일뿐입니다. Environment record는 현재 실행 중인 함수와 코드 블록, 스크립트와 연관되어 있습니다. 이렇게 연관되어있으니 함수를 실행시킬수있으며, 코드 블록, 다른 스크립트를 사용할수 있습니다.
2.  변수를 변경한다는것은 Environment record의 프로퍼티를 변경하는것과 같습니다.

**! Lexical Environment는 명세서에만 존재합니다.**

Lexical Environment는 명세서(Spec)에서 자바스크립트가 어떻게 동작하는지 설명하는 데에 쓰이는 '이론상의' 객체입니다.

따라서 코드를 사용해 이 Lexical Environment를 직접 얻거나 조작하는 것은 불가능합니다. '이론상의' 객체이기 때문입니다.

자바스크립트 엔진들은 명세서에 언급된 사항을 준수하면서 엔진 고유의 방법을 사용해 Lexical Environment를 최적화 합니다.

사용하지 않는 변수를 버려 메모리를 절약하거나 기타 내부 트릭을 써서 말이죠

다음 내용은 **Lexical Environment (렉시컬 환경) - 함수 선언문** 입니다.