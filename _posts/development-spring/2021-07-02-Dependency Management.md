---
layout: post
title:  "Dependency Management"
subtitle: "Spring Boot의 의존성 관리를 알아봅니다."
categories: legacy
tags: spring
comments: true
---

[원문](https://docs.spring.io/spring-boot/docs/current/reference/htmlsingle/#using.build-systems.dependency-management)

스프링 부트의 각각 버전은 지원되는 의존성이 담긴 `curated list`를 제공 합니다

> A curated list is a list on a certain topic that has been compiled carefully, usually by research and by a creator of the included content.

`curated list`는 연구에 의해서나 컨텐츠를 만든 사람에 의해 신중하게 편집된 특정 주제에 대한 목록이다.

실제 빌드를 할때 스프링이 어떠한 의존성이라도 다 관리를 해주기때문에 우리가 직접 버전을 제공해줄 필요가없다.

이게 무슨말이냐면 pom.xml같은거 보면 버전을 명시를 해줘도 되는게 있고 없는게 있는데

```
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>untitled</artifactId>
    <version>1.0-SNAPSHOT</version>∂∂

    <properties>
        <maven.compiler.source>11</maven.compiler.source>
        <maven.compiler.target>11</maven.compiler.target>
    </properties>

    <!-- Inherit defaults from Spring Boot -->
    <parent>
      <groupId>org.springframework.boot</groupId>
      <artifactId>spring-boot-starter-parent</artifactId>
      <version>2.2.0.RELEASE</version>
    </parent>

    <!-- Add typical dependencies for a web application -->
    <dependencies>
      <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
      </dependency>
    </dependencies>

    <!-- Package as an executable jar -->
    <build>
      <plugins>
        <plugin>
          <groupId>org.springframework.boot</groupId>
          <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
      </plugins>
    </build>



</project>


```

pom.xml의 parent태그를 보게되면 `org.springframework.boot`에 있는 `spring-boot-starter-parent` 폼파일로 실제로 가보면

나의 `spring-boot-starter-web`의 의존성을 `spring-boot-starter-parent`이 이미 해주고 있기때문에 `spring-boot-starter-web`의 버전을 직접 명시해주지 않아도 된다는 말이다 `parent`에서 관리해주는것은 우리가 하는게 아니며 `spring-boot`프로젝트를 생성시 알아서 설정해주는 것이다. 저걸 하지않게된다면 저런 설정을 우리가 일일이 다해야한다. 버전을 근데 명시를 해주는게 좋다. 왜냐하면 내 프로젝트는 만약 스프링 부트 5버전을 쓰고있는데 스프링 부트가 업그레이드 되어 5.1버전이 됬다고 하자. 그럼 버전명시를 해주지않으면 내 프로젝트는 자동으로 5.1버전을 쓰게되는데 이때 내 프로젝트가 만약 5.1을 쓰면 안된다고 하면 오류가 발생할수있으므로 명시를 안해주는것이 꼭 좋은것만은 아니다.

> `spring-boot-starter-web`처럼 버전명시를 안해주는 의존성들도 필요하다면 우리가 버전 명시를 직접 해줄수있다.

스프링 부트가 업그레이드되면 우리의 의존성들도 일관된 방식으로 업그레이드 됩니다.

`curated list`에는 Spring Boot와 함께 사용할수 있는 모든 Spring 모듈과 `third-party`라이브러리가 포함되어있습니다.

`curated list`는 `Maven`과 `Gradle`의 `Bills of Materials`(spring-boot-dependencies)의 기준으로 사용할수 있습니다.

\*\* Spring Boot의 버전은 Spring Framework의 기반버전과 관련이 되어있으므로 Spring Boot의 버전을 직접 명시하는것을 정말 추천하지 않습니다.