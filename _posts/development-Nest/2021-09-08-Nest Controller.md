---
layout: post
title:  "Nest Controller"
subtitle: "Nest Controller란?"
categories: legacy
tags: nest
comments: true
---

### Controller

Controller는 client로부터 들어오는 모든 `request`에 `response`를 return 해줘야할 책임이 있습니다.

<img width="934" alt="Screen Shot 2021-09-08 at 5 13 00 PM" src="https://user-images.githubusercontent.com/44861205/132471939-a1497e4b-68e1-4a99-a138-893c2b834ed6.png">

컨트롤러의 목적은 application의 특정 request를 받기위함에 있습니다.

`routing mechanism`은 어떤 요청을 어떤 컨트롤러가 받을지 적절히 조절하는것을 말합니다.

각각의 컨트롤러는 한개 이상의 route를 가지고 있는게 빈번하며, 다른 route들은 다른 action을 수행할수 있습니다.

기본적인 controller를 만들기위해서, 우리는 클래스들과 `decorators`를 사용할수 있습니다. Decorators는 클래스에 필수적으로 필요한 metadata와 연결시키며, Nest에게 `routing map`을 만들수 있게 합니다.(적절한 controller에게 request를 대응시킵니다.)

> 기본적으로 제공되는 validation을 가진 CRUD controller를 빠르게 만들고싶다면, Nest CLI의 generator인 `nest g resource` [name]`을 사용하면 된다. ex) nest g service somename` 이렇게 하면 somename이라는 이름을 가진 service를 Nest CLI를 통해 자동생성 됩니다.


