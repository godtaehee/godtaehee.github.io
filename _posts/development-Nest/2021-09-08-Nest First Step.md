---
layout: post
title:  "Nest First Step"
subtitle: "Nest First Step에 대하여.."
categories: development
tags: nest
comments: true
---

Nest는 `Typescript`를 사랑하지만 무엇보다도 `Node.js`를 사랑합니다. 이것이 Nest가 Typescript와 Vanila JS 둘다 호환이 되는 이유입니다.

Nest는 최신 언어의 장점을 모두 가지고 있어서, Vanila JS를 사용하고싶다면 `babel`컴파일러가 있어야 합니다.

이 튜토리얼에서는 `Typescript`로만 진행을 합니다. 하지만 공식 문서에 보시면 우상단에 버튼클릭을 통해 `Javascript`코드도 볼 수 있다고합니다.

### main.ts

`main.ts`는 처음 Nest 프로젝트를 만들게 되면 Nest Application instance를 만드는 core function인  `Nestfactory`를 사용하는 application의 최초 진입점이 되는 파일입니다.

`main.ts`는 **bootstrap**이라는 `async` 함수를 통해 Nest Application을 부팅합니다.

```ts
import { NestFactory } from '@nestjs/core';
import { AppModule } from './app.module';

async function bootstrap() {
  const app = await NestFactory.create(AppModule);
  await app.listen(3000);
}
bootstrap();
```

Nest Application Instance를 만들기 위해서, `NestFactory`라는 core class를 사용합니다. NestFactory는 application instance를 만들어주는 몇개의 `static method`를 가지고 있습니다.

`create()` 메소드는 `INestApplication` interface를 만족하는 **application object`를 반환합니다.

이 object는 앞으로 다룰 chapter에서 사용할 함수들을 제공합니다. 위의 `main.ts` 예에서는, HTTP listener를 통해 application이 inbound HTTP 요청을 대기하게 합니다. 

> Nest CLI로 만든 초기 프로젝트의 프로젝트 구조는 각각의 모듈들이 Nest에 의해 장려되는 directory 컨벤션을 따르도록 되어 있습니다. 즉 Nest CLI로 초기 프로젝트의 구조를 만들고, 그 이후 다른 모듈들(service, controller, etc...)을 생성할때도 Nest가 지정한 directory convention을 따르도록 되어있습니다.

### Platform

Nest는 Platform에 구애받지 않는 프레임워크를 만드는것을 목표로 합니다.

독립된 플랫폼은 애플리케이션의 타입에 구애받지 않고 개발자들이 자신이 작성한 코드를 재사용할수 있다는 장점을 가집니다.

기술적으로, Nest는 adapter가 한번 만들어지면 어떠한 Node HTTP framework와도 사용가능합니다. 즉시 지원되는 플랫폼은 `express`, `fastify`가 있습니다. 여러분이 편하신것을 사용하시면 됩니다.

#### platform-express

Express는 잘 알려진 node를 위한 minimalist web framework입니다. 
커뮤니티에 의해 구현된 많은 리소스들로 test된 프로덕션 level도 지원하는 라이브러리입니다.

`@nestjs/platform-express` 패키지가 기본값이며, 많은 유저들이 express를 사용하고있으며, 따로 설정해줄게 전혀없이 Nest가 알아서 Express를 platform의 기본값으로 사용합니다. 

실제 Nest 프로젝트 생성후 node_modules/@nestjs 에 들어가 보면 `platform-express`폴더가 기본적으로 설치된것을 확인하실수 있습니다.

#### platform-fastify

Fastify는 최고의 성능과 속도에 집중한 고성능, 낮은 오버헤드를 가진 framework입니다. Nest에서 fastify를 사용해주기 위해서는 따로 작업이 필요합니다. [여기](https://docs.nestjs.com/techniques/performance) 에서 확인하실수 있습니다.

platform-express와는 달리 node_modules/@nest폴더에 platform-fastify는 찾아볼수 없는것을 확인하실수 있습니다.

#### 정리

어떤 플랫폼이 사용되든, 그 플랫폼에 알맞는 interface를 사용합니다.

그 인터페이스들은 각각 `NestExpressApplication`와 `NestFastifyApplication`입니다.

아래의 예처럼, `NestFactory.create()`메소드의 파라미터로서 각각 사용하고싶은 플랫폼의 인터페이스를 주게된다면, 각각의 플랫폼에 맞는 method들을 가진 app 객체가 반환될것입니다. 아래의 예에서는 타입을 지정해주긴 했지만 만약 default platform인 express를 사용할 경우는 타입을 저렇게 지정해줄 필요는 없습니다.

```ts
const app = await NestFactory.create<NestExpressApplication>(AppModule)
```


### 애플리케이션 실제 실행

한번 설치과정을 마치면, 아래의 명령어와 함께 inbound HTTP 요청을 기다리는 application을 실행할수 있습니다.

```shell
$ npm run start
```

이 명령어는 `src/main.ts`파일에 정의된 포트로 HTTP 요청을 대기하고 있으며, `http://localhost:3000`으로 접속하게 되면 `Hello World!`메시지를 볼수 있습니다.