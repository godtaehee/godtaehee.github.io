---
layout: post
title:  "Nest Authentication"
subtitle: "Nest Authentication with Passport"
categories: development
tags: ecma-script
comments: true
---

Authentication(인증)의 접근 방법은 그 애플리케이션의 **요구사항**에 따라 달라진다.

**Passport**는 많은 production 단계의 애플리케이션에서 성공적으로 사용이 되고있고 커뮤니티에 의해 잘 알려진 node.js Authentication 라이브러리 이다.

Nest.js에서는 `@nestjs/passport`라는 모듈을 사용하여 쉽게 passport를 통합하여 사용할수 있습니다.

Passport는 아래의 단계를 통해 실행됩니다

- 유저의 "credentials"(username/password, JSON Web Token(JWT), Identity Provider로부터 제공되는 identity token)을 확인함으로서 인증을 진행합니다.

- 인증 상태를 관리합니다. (JWT같은 휴대할수 있는 토큰을 발행하거나, Express Session을 만드는 방식으로 관리합니다.)

- 인증된 유저의 정보를 Route handler에서 사용되는 Request object에 붙혀 사용합니다.

### username/password

username/password로 사용자가 인증을 하게되면, 서버는 연속된 클라이언트의 요청에 대해 **인증**을 위해 `authorization header`으로 `bearer token`으로서 JWT를 발행합니다.

이렇게 발급된 **유효한 JWT**를 포함한 요청만 접근 가능하도록 하게하는 route를 만들어야합니다.

username/password로 인증하는 방식에 알맞은 `passport-local`패키지를 설치해야합니다.

```shell
$ npm install --save @nestjs/passport passport passport-local
$ npm install --save-dev @types/passport-local
```

> 어떤 Passport를 사용하더라도, `@nestjs/passport`와 `passport` 패키지는 무조건 필요합니다. 이 두가지의 패키지를 설치한 후 자신의 애플리케이션에 맞는 passport를 설치해주세요 (위의 예같은경우는 passport-local 입니다.) 또한 passport에 맞는 type definition을 담고 있는 @types/~~ 패키지도 설치해주셔야 합니다.
 
### Passport 구현 전략

이렇게 해서 우리의 authentication feature를 구현할 준비를 마쳤고, 본격적으로 이 Passport를 구현하기 시작하기 전 전반적인 개요에 대해서 살펴 보겠습니다.

Passport를 `mini framework`라고 생각을 하면 편하다.

이러한 프레임워크가 authentication 과정을 우리가 커스터마이징한대로 몇가지 단계로 나누어 **추상화**를 해줍니다.

보통 이러한 프레임워크 구현은 JSON Object를 Parameter로써 받던지, Passport가 적절한 시간에 호출하는 **Callback function**으로 할수 있습니다.

그런다음 `@nestjs/passport` 모듈이 이 프레임워크를 Nest 스타일로 wrapping 해주고, Nest Application에 쉽게 통합되도록 해줍니다.

우리는 `@nestjs/passport`를 사용하기 이전 Vanila버전의 passport가 동작하는 방식에 대해 먼저 고려해보겠습니다.

> 외국에서는 Vanila라는게 "날것의"라는 뜻 아니면 "특징이 없는"으로 쓰인다고 합니다. Vanila JS도 저희가 순수 자바스크립트를 나타낼때 사용하잖아요? 그것처럼 원래 Passport는 어떻게 동작하는지 부터 살펴보기위한 절차입니다.

### Vanila Passport

Vanila Passport에서는 2가지로 Passport 전략을 설정합니다.

1. 해당 전략에 관련된 **옵션**을 정해줍니다. 예를들어, JWT 전략에서는 token을 발행하기위해서 사용하는 비밀키를 제공해야합니다. 이 비밀키를 가지고 토큰을 발행하며, 비밀키가 조금이라도 달라진다면 같은 유저정보가 들어와도 다른 토큰이 발행됩니다.

1. "검증하는 콜백함수(verify callback)"는 user 계정을 관리하는곳과 어떻게 상호작용할지에 대해서 Passport에게 알려줍니다. 또 유저가 존재하는지, 새로운 계정을 생성할수 있는지, 해당 유저의 "credentials"이 유효한지에 대해서 체크합니다. 만약 validation이 성공한다면 user의 정보를 반환하고, validation이 실패한다면 null을 반환합니다 (여기서 validation의 실패라는것은 유저 정보를 찾을수 없거나, `passport-local`에서는 비밀번호가 맞지않는것을 의미합니다.)

`@nestjs/passport`를 사용하면, `PassportStrategy`라는 클래스를 상속받아 **Passport strategy**를 설정할수 있습니다.

선택사항인 **options object**를 `subclass(PassportStrategy클래스를 상속 받는 클래스)`의 `super()`메소드의 매개변수로 사용하여 설정할수 있습니다. (위의 1번 참고)

또한, `subclass`의 `validate`함수를 구현함으로서 `verify callback`을 제공할수 있습니다. (위의 2번 참고)

We'll start by generating an AuthModule and in it, an AuthService:

AuthModule과 AuthService를 generate하겠습니다.

```shell
$ nest g module auth
$ nest g service auth
```

`AuthService`를 구현함으로서, `UserService`안의 유저에 관한 함수들을 `encapsulate`하는데 유용하다는것을 알수 있습니다. 그럼 User의 모듈과 서비스를 generate해봅시다.

```shell
$ nest g module users
$ nest g service users
```

아래의 코드로 generated된 파일들의 내용을 대체하시고, 현재 진행하고있는 튜토리얼에서는 hard coding된 in-memroy 방식의 유저 데이터를 사용합니다.

또한 username으로 유저를 찾는 find 메소드를 정의하였습니다. 실제 앱에서 find 함수는, TypeORM, Sequelize, Mongoose와 같은 라이브러리를 사용해서 모델과 `persistence` 계층을 사용하는 곳에 정의된 함수입니다.

```ts
// users/users.service.ts

import { Injectable } from '@nestjs/common';

// This should be a real class/interface representing a user entity
export type User = any;

@Injectable()
export class UsersService {
  private readonly users = [
    {
      userId: 1,
      username: 'john',
      password: 'changeme',
    },
    {
      userId: 2,
      username: 'maria',
      password: 'guess',
    },
  ];

  async findOne(username: string): Promise<User | undefined> {
    return this.users.find(user => user.username === username);
  }
}
```