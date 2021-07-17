# Spring

## 简介

- Spring：春天即给软件行业带来了春天!
- 2002年，首次推出了Spring框架的雏形：interface 21框架!
- 2004年3月24日，Spring框架是以interface21框架为基础，经过了重新的设计，并不断的丰富其内涵，于2004年3月21日发布了1.0正式版本
- Rod Johnson是Spring的创始人，著名作者，他是悉尼大学的博士，然而他的专业不是计算机，而是音乐学。
- spring的设计理论：使得现有的技术更加容易使用，本身是一个大杂烩
- SSH：Struct2+Spring+Hibernate
- SSM：SpringMVc+Spring+Mybatis

官网：https://spring.io

官方下载地址： https://start.spring.io/

github地址：https://github.com/spring-projects/spring-framework

```xml
     <dependencies>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-webmvc</artifactId>
        </dependency>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>
                mysql-connector-java
            </artifactId>
            <version>5.1.25</version>
        </dependency> 
    </dependencies>
```

## Spring优点

1. spring是一个开源的免费的框架（容器)!
2. spring是一个轻量级、非入侵式的框架!
3. 控制反转，面向切面编程(AOP)
4. 支持事务的处理，对框架进行整合!


总结一句话：Spring就是一个轻量级的控制反转(IOC)和面向切面编程(AOP)的框架!


## 组成

![](https://gitee.com/kangyujian/notebook-images/raw/master/images/2020-09-01-11-59-43.png)


## 扩展

在Spring的官网有这个介绍：现代化的Java开发，说白了就是基于Spring的开发。

![](https://gitee.com/kangyujian/notebook-images/raw/master/images/2020-09-01-12-02-50.png)

- SpringBoot 
    - 是一个快速开发的脚手架，通过Springboot可以开发出一个单个的微服务
    - 约定大于配置
    - 现在大多数的公司都在使用SpringBoot进行快速开发
    - 学习SpringBoot的前提是完全掌握Spring和SpringMVC
    - 承上启下的作用

- SpringCloud是基于SpringBoot的实现的。

弊端
- 发展了很多年导致配置十分繁琐，人称配置地狱













