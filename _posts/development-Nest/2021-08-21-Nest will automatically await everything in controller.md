---
layout: post
title:  "Nest will automatically await everything in controller"
subtitle: "Nest.js가 async/await를 처리해주는데 있어서 재밌는것을 발견하여 글을 올립니다."
categories: development
tags: ecma-script
comments: true
---

![Screen Shot 2021-08-27 at 12 35 56 AM](https://user-images.githubusercontent.com/44861205/130992728-5260bed8-9963-4eff-b8da-d626b1bbc2ff.png)

Nest.js 디스코드 방에서 재밌는 토론이 열려서 소개를 드리려고 합니다.

Nest.js 공식 문서에서 여러가지 예제 코드를 보았는데 

```javascript
userRepository.save();
userRepository.find();
```



Express에서 ORM이나 ODM을 이용하게되면 다음과 같이 `save`, `find`와 같은 함수로 INSERT와 SELECT를 쿼리문을 작성하지 않고 결과를 얻어올수 있다.

대신 DB에서 결과를 얻어올때까지 기다려줘야하므로 보통 비동기로 처리하는 경우가 대부분인데

이때 나는 `Syntax Sugar` 이점이 있는 async/await를 주로 사용했다.

async/await 사용을 안하면 `Promise<pending>`이 반환되므로 이것을 `then`이나 `catch`로 추가 작업을 해줘야한다.

그런데 이상하게도 Nest.js에서 똑같은 ORM이나 ODM을 사용하는데 어떤건 await를 사용하고 어떤것은 await를 사용하지 않는다는것에서 질문을 한것이다.

![Screen Shot 2021-08-27 at 12 42 28 AM](https://user-images.githubusercontent.com/44861205/130993658-c1af072d-2df7-4bdc-b59b-14cf38addc1a.png)

> Boryan Bomba: 
I know that async-await is used for asynchronous code. I don't understand why some methods in Service use async-await (like UsersRepository.save(user)) and others (like UsersService.find() which returns all users) don't

`async-await`는 비동기적인 코드에 사용을 하는것으로 알고있는데 왜 Service 계층의 몇몇 함수들은 async-await를 사용하고 안하는지 잘 모르겠다.

> Boryan Bomba:
we need to wait for the db to return the result of our query in both cases, but only create case uses async-await

두가지의 케이스 모두 DB로부터 결과를 받아오기위해 기다려야하는데 어떤건 기다리고 어떤건 안기다린다.

> papooch:
That some services don't explicitly 'await' the function does not mean they are not asynchronous. They just return the promise to the caller (e.g a controller) to await it. What is not clear at the first sight, though, is that if you return a promise from a controller, Nest automatically awaits it and only then sends the response.

몇몇의 서비스들에게 await를 붙히지 않는다고해서 그게 비동기적인 함수가 아니라는것을 의미하지않는다. 그 비동기 함수들은 그냥 단지 Promise를 자신을 호출한 계층(Controller)에게 return 할뿐이다. 한번에 알아보기 힘든점은 Nest가 controller로부터 return된 promise를 자동으로 await 해준후에서야 response 객체를 통해 보낸다는것이다.

> 지금 이 게시글의 핵심이다. Nest.js는 Controller에서 만약 await를 붙혀지지 않은채 그냥 반환을 해도 await를 자동으로 해주고 난후에서야 response에게 그 결과값을 전해준다는것이다. 근데 관련문서에대해서 구글링을 해보아도 마땅한 답을 얻지 못해서 저 말이 사실인지는 아직 증명이 안된상태였다. 물론 저사람 말이 맞을수는 있지만 혹시나 틀릴수도있으니.. 더 토론한 내용을 우선 살펴보자

> Boryan Bomba:
So there's no need to use async-await at all? 

그래서 async-await를 필요가 전혀 없다는것인가?

> papooch: 
if you need to retrieve something from the database and then perform some bussiness logic, you have to await the call, otherwise you won't have access to the result. Just google some articles on promises and async/await.

database로부터 어떤걸 받아와서 어떠한 `Business logic`을 수행해야한다면 `await`를 무조건 사용해야한다 그렇지 않으면 그 쿼리결과에 접근을 할수 없기때문이다. async/await에 대해서 구글링을 몇개 해봐라

> 쿼리 결과를 받아온 직후 그 결과를 가지고 어떤 비지니스 로직을 처리해야한다면 await를 사용해서 결과를 무조건 받아와서 하려는 비지니스로직을 처리해야한다. 하지만 그게아니라면 굳이 await를 붙혀줄 필요 없다는것이다. 이게 좀 나는 충격이였다. async함수는 await를 무조건 붙혀줘야하는줄 알았는데 그게 아니였다. 근데 이것은 우선 Nest에서만 가능한것 같다. 왜냐하면 Nest가 자동으로 await를 해주니까..

![Screen Shot 2021-08-27 at 1 03 51 AM](https://user-images.githubusercontent.com/44861205/130996824-3fb4fa3b-6b78-4113-968c-b520a315caae.png)

> Boryan Bomba:
So basically if I return awaited result from service, I don't need to use async-await in controller and vice versa?

Service에서 await를 끝마친 결과를 반환하면 controller에서는 async-await가 필요 없는거냐 ?

> papooch:
well, you have to. Because in order to use await inside a method, the method must be async (i.e. returning a promise), so the caller code (the controller) would receive a promise either way.
you can either return a new promise (with return await dbCall) or the dbCall promise itself. For the controller, it does not really matter.

그게아니라 async-await는 무조건 붙혀야한다. 함수 안에서 await를 하기위해서는 그 함수는 무조건 async 함수여야한다, 그래야 호출자(controller)가 promise를 받을수있다. controller에서 새로운 새로운 promise(database Call을 포함해서)를 반환하거나 그냥 database Call Promise 자체를 반환할수있다.

> taehee: If I use the result for service logic, I have to await result. (Use await)
If I don't use the result now, I don't have to await result (Don't need to use await)
is It right ?

서비스 로직을 위해 결과를 사용할거면 결과를 무조건 await해야하고(await를 사용해야한다),
지금 당장 result를 사용하지 않을것이면, result를 굳이 await할 필요는 없다

라고 내가 두가지 케이스로 정리를 해보았다. 답장으로 `yup`이라는 답변을 받았으며 이제 확실히 이해가 되었다.

![Screen Shot 2021-08-27 at 1 09 40 AM](https://user-images.githubusercontent.com/44861205/130997714-c8f60533-83fd-458d-a328-a31a235268a0.png)


결론은 Nest.js에서 Controller는 Promise를 만약 await를 하지않고 결과를 반환한다면, 자동적으로 Nest에서 await한 결과값을 클라이언트에게 response해준다.

내가 처음에 오해했던점은 모든 계층 (Controller, Service, Repository)에서 나는 await를 하지 않아도 알아서 
Promise에 감싸져있는 result가 반환이 되는줄알았다. 하지만 그게아니라 위에서도 언급했듯이

Controller에서 최종적으로 Promise를 await하지 않은 결과를 return하게 된다면 그것만 await를 Nest가 자동으로 해주어 반환을 한다는것이다.

실제 내가 Express하고 비교를 해보기위해서 몇가지 실험을 해보았는데 결과는 이 날 디스코드에서 진행된 회의의 결과와 같다.

## 가정 상황

Express와 Nest.js 모두 GET요청 /test API를 만들어 async-await함수의 결과값인 Promise를 `await 없이` 반환해보았다. 만약 위에 적었던 내용이 모두 사실이라면, Nest.js에서는 Promise에 감싸져있는 'test'라는 스트링을 화면에 출력해줄것이다. 왜냐하면 await를 통해 Promise에서 resolve된 값을 꺼내서 화면에 보여줘야만 하기때문이다.

### Express

![Screen Shot 2021-08-27 at 10 09 05 PM](https://user-images.githubusercontent.com/44861205/131132221-9371901d-a468-4b16-bc39-d13abe40bbb5.png)
console 창에 보면 `Promise { 'string' }`가 찍혔으며 await를 하지않고 response 객체에 send함수를 호출했기때문에 Promise가 그대로 남아있는 상태가 된다. 

![Screen Shot 2021-08-27 at 10 11 46 PM](https://user-images.githubusercontent.com/44861205/131132556-6b7ab344-8975-4964-bf88-fe587e105ea1.png)


따라서 localhost:5000/test API에서는 아무것도 담겨있지 않은 JSON 객체를 출력하고있다.

### Nest.js

![Screen Shot 2021-08-27 at 10 14 35 PM](https://user-images.githubusercontent.com/44861205/131133027-cbf3f82f-8db9-491b-b4ef-1802508fcbd9.png)

이번엔 Nest.js에서 실험한 내용인데, 똑같이 Promise로 감싸져있는 'test'라는 string을 확인할수 있다.

이를 await없이 그냥 반환했더니 결과는 아래와 같다.

![Screen Shot 2021-08-27 at 10 15 46 PM](https://user-images.githubusercontent.com/44861205/131133182-885a907f-8d63-4912-9596-1e229067170d.png)

Express와 달리 Nest.js는 Promise 안에 들어있던 'test'라는 스트링을 정확히 브라우저에 출력해주는것을 확인할수 있다.

## 결론

1. 프로미스로 감싸져있는 결과를 사용하고싶을땐 await를 바로써라
2. 안쓸꺼면 await안써도된다.
3. Nest.js에서 Controller에서  최종적으로 반환을 할때 await를 안해줘도 해준것처럼 Nest.js가 자동적으로 await 하여 반환을해준다