---
layout: post
title:  "Spring Boot Web Server Off"
subtitle: "Spring Boot Web Server를 Off하는 방법을 알아봅니다."
categories: legacy
tags: spring
comments: true
---

```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.WebApplicationType;

public class Application {
    public static void main(String[] args) {
        SpringApplication springApplication = new SpringApplication();
        springApplication.setWebApplicationType(WebApplicationType.NONE);
    }
}

```

`@SpringBootApplication`을 없애고 WebServer 구동을 None 할수있다.