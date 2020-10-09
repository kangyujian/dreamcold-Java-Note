# 什么是Mybatis

## Mybatis简介

![](images/2020-10-09-20-03-43.png)

- Mybatis是一款优秀的持久层框架
- 它支持定制化的SQL、存储过程以及高级映射
- Mybatis避免了几乎所有的JDBC代码和手动设置参数以及获取结果集。Mybatis可以使用简单的xml或者注解来配置和映射原生类型，接口和Java的POJO(Plain Old Java Objects，普通老式Java对象)为数据库中的记录

- MyBatis 本是apache的一个开源项目iBatis, 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis 。

- 2013年11月迁移到Github。

## 如何获取Mybatis？

1. maven仓库
```java
        <!-- https://mvnrepository.com/artifact/org.mybatis/mybatis -->
        <dependency>
            <groupId>org.mybatis</groupId>
            <artifactId>mybatis</artifactId>
            <version>3.5.6</version>
        </dependency>
```
2. Github：https://github.com/mybatis/mybatis-3
3. 中文文档

## 什么是持久层?

数据持久化
- 持久化就是将程序的数据在持久化和瞬时状态转化的过程
- 内存：断电既失
- 数据库（jdbc),io文件持久化
- 生活：冷藏、罐头

**为什么需要持久化？**
- 有一些对象，不能够让他丢掉
- 内存太贵了


## 持久层

Dao层，Service、Controller层....
- 完成持久化工作的代码块
- 层界限十分明显


## 为什么需要mybaitis

- 方便
- 传统的jdbc太复杂了，将其简化、框架、自动化
- 帮助程序员将数据存到数据库中
- 不用Mybatis也可以，更容易上手
- 技术没有高低之分
- 优点
    - 简单易学
    - 灵活
    - sql和代码分离，提高了可维护性
    - 提供映射标签，支持对象和数据库中orm字段关系映射
    - 提供对象关系映射标签，支持对象关系组件维护

最重要的一点：使用的人多！

框架组合Spring+SpringMVC+Springboot

