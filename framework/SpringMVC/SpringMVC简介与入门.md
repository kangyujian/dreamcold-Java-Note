# SpringMVC简介与入门

## 什么是SpringMvc？

- SpringMVC:是基于spring的一个框架，实际上就是spring的一个模块。专门是 做web开发的。
  理解是servlet的一个升级
- web开发底层是servlet，框架 是在servlet基础上面加入一些功能，让你做veb开发方便。
- springMVC就是一一个spring,spring是容器， Ioc能够管理对象，使用`<bean>`，`@Component`，`@Repository`, `@Service`, `@Controller`
- SpringMVC能够创建对象，放入到容器中（SpringMVC容器），SpringMVC容器中放的是控制器对象
- 我们要做的是使用`@Contorller`创建控制器对象，把对象放入到springmvc容器中，把创建 的对象作为控制器使用
  这个控制器对象能按收用户的请求，显示处理结果， 就当做是一个servlet使用。
- 使用Controller注解创建的是一个普通类的对象，不是Servlet. springmvc赋于 了控制器对象一些额外的功能.
- DispatherServlet:负责接收用户的所有请求，用户 把请求给了DispatherServlet(中央servlet),之 后Dispatherservlet把请求转发给我们的Controller对象，最后 是Controller对象处理请求。
- Index .jsp----->Dispatherservlet (Servlet) ---->转发，分配给---->Controller对象( `@Controller`注解创建的对象)

## SpringMVC的优点

**1.基于MVC架构**
	基于MVC架构，功能分工明确。解耦合
**2.容易理解，上手快: 使用简单。**
就可以开发一个注解的SpringMVC项目，SpringMVC也是轻量级的jar很小。不依赖的特定的接口和类。
**3.作为Spring框架一部分，能够使用Spring的loC和Aop.方便整合**
Strtus,MyBatis,Hiberate,JPA等其他框架。
**4.SpringMVC强化注解的使用，在控制器，Service, Dao 都可以使用注解。方便灵活。**
使用@Controller创建处理器对象,@Service创建业务对象，@Autowired 或者@Resource在控制器类中注入Service, Service 类中注入Dao.

##  创建基于SpringMVC的Hello World项目

1. 打开IDEA，创建空工程,next,并为空工程起名

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210627235402864.png" alt="image-20210627235402864" style="zoom:50%;" />

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210627235459377.png" alt="image-20210627235459377" style="zoom:50%;" />

2. File点击Project Structure，点击"+"号创建新的模块

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210628000237703.png" alt="image-20210628000237703" style="zoom:50%;" />

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210628000121210.png" alt="image-20210628000121210" style="zoom:50%;" />

3. 一路next,直到输入项目名称

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210628000511039.png" alt="image-20210628000511039" style="zoom:50%;" />

4. 项目目录详解

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210628092228584.png" alt="image-20210628092228584" style="zoom:50%;" />

## 项目的功能描述

1. 需求:用户在页面发起一个请求，请 求交给springmvc的控制器对象，并显示请求的处理结果（在结果页面显示欢迎语句）。
2. 加入依赖spring-webmvc，会把spring的依赖都加入项目中，还要加jsp、servlet依赖因为底层实现的缘故

3. 重点:
   在web. xml中注册springmvc框架的核心对象Dispatcherservlet
   - Dispatcherservlet叫做中央调度器，是一个servlet，它的父 类是继承Ht tpServlet
   - Dispatcherservlet页叫做前端控制器( front controller )
   - DispatcherServlet负责接收用户提交的请求，调用其它的控制器对象，并把请求的处理结果显示给用户

4. 创建发起请求页面index.jsp
5. 在类的上面加入@Controller注解，创建对象，并放入到springmvc容器中，在类中的方法上加入@RequestMapping注解
6. 创建一个作为结果的jsp，显示请求的处理结果
7. 创建springmvc的配置文件( spring的配置文件一样)，声明组件扫描器，声明视图解析器

## 实现项目需求

1. 导入maven依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>

<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.dreamcold</groupId>
  <artifactId>dreamcold</artifactId>
  <version>1.0-SNAPSHOT</version>
  <packaging>war</packaging>
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>
  <dependencies>
    <!--        Servlet依赖-->
    <dependency>
      <groupId>javax.servlet</groupId>
      <artifactId>javax.servlet-api</artifactId>
      <version>3.1.0</version>
      <scope>provided</scope>
    </dependency>
    <!--SpringMVC依赖    -->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>5.2.5.RELEASE</version>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>4.11</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-compiler-plugin</artifactId>
        <version>3.7.0</version>
        <configuration>
          <source>1.8</source>
          <target>1.8</target>
          <encoding>UTF-8</encoding>
        </configuration>
      </plugin>
    </plugins>
  </build>
</project>
```

导入springmvc会自动导入相关spring的依赖

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210628100124908.png" alt="image-20210628100124908" style="zoom:67%;" />

2. 提高web.xml的版本到4.0,File->Project Structure->web,接下来点击减号

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210628175448698.png" alt="image-20210628175448698" style="zoom:50%;" />

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210628175938874.png" alt="image-20210628175938874" style="zoom:50%;" />

3. 修改web.xml的名称

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210628180425433.png" alt="image-20210628180425433" style="zoom:50%;" />

4. 声明注册springmvc核心对象DispatchServlet，由于servlet只有被访问才被创建，我们希望再tomcat启动的时候就创建这个servlet对象，需要在tomcat启动后创建DispatchServlet实例，为什么要创建其实例呢？是因为在创建DispatchServlet过程中会同时创建springmvc容器对象，读取springmvc配置文件，把配置文件中的对象都创建好，当用户发起请求时就可以直接使用对象了，servlet初始化会指定init方法，DispatchServlet在init方法中

```java
//1.创建容器读取配置文件
WebApplicationContext ctx=new ClassPathxmlApplicationContext("springmvc.xml");
//2. 把容器对象放入ServletContext中
getServletContext().setAttribute(key,ctx);
```

在tomcat启动后创建servlet对象，load-on-startup表示tomcat创建对象的顺序，其数值越小，tomcat创建其的时间就越早，其保证了创建servlet对象的顺序

```xml
<?xml version="1.0" encoding="UTF-8"?>
<web-app xmlns="http://xmlns.jcp.org/xml/ns/javaee"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://xmlns.jcp.org/xml/ns/javaee http://xmlns.jcp.org/xml/ns/javaee/web-app_4_0.xsd"
         version="4.0">
    <servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!-- 在tomcat启动后创建servlet对象-->
        <load-on-startup>1</load-on-startup>
    </servlet>
</web-app>
```

5. springmvc创建容器对象读取的文件默认是`/WEB-INF/<servlet-name>-servlet.xml`,但是我们一般希望不采用这样的规则而是采用自定义的xml文件位置

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210629145912191.png" alt="image-20210629145912191" style="zoom:50%;" />

```xml
<servlet>
        <servlet-name>springmvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <!--  自定义springmvc读取的配置文件的位置      -->
        <init-param>
            <!-- springmvc配置文件位置的属性           -->
            <param-name>contextConfigLocation</param-name>
            <!--   指定自定义文件位置         -->
            <param-value>classpath:springmvc.xml</param-value>
        </init-param>
        <!-- 在tomcat启动后创建servlet对象-->
        <load-on-startup>1</load-on-startup>
</servlet>
```

6. 创建servlet-mapping,主要有两种格式一种格式采用扩展名或者斜杠的方式

```xml
    <servlet-mapping>
        <servlet-name>springmvc</servlet-name>
        <!--
         使用框架的时候，url-pattern可以使用两种值
            1.使用扩展名方式，语法*. xxxx,
            xxxx是自定义的扩展名。常用的方式 *.do, ".action, mvc等等
            http://localhost:8080/myweb/some.do
            http://localhost:8080/myweb/other.do
            2.使用斜杠"/"
         -->
        <url-pattern>*.do</url-pattern>
    </servlet-mapping>
```

7. 创建控制类，新建index.jsp文件

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210629224747374.png" alt="image-20210629224747374" style="zoom:50%;" />

控制器源代码MyController.java

```java
package com.dreamcold.controller;

import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

/**
 * @Controller创建处理器对象
 *能处理请求的都是控制器(处理器) : MyController能处理请求，
 *做后端控制器( back controller )
 */
@Controller(value="myController")
public class MyController {
    /**
     * 处理用户提交的请求，springmvc中 是使用方法来处理的。
     * 方法是自定义的可以有多种返回值
     * 方法名称自定义
     */
    /**
     * 准备使用doSome方法处理some.do请求。
     * @RequestMapping:请求映射，作用是把一个请求地址和一个方法綁定在起个请求指定一个方法处理。
     * 属性: 1. value 是-个string ,表示请求的uri 地址的( some.do )。
     * value的值必须是唯一的，不能重复。 在使用时， 推荐地址以/
     * 位置:1.在方法的上面，常用的。
     *      2.在类的上面
     * 说明:使用Reques tMapping修饰的方法叫做处理器方法或者控制器方法。
     * 使用RequestMapping修饰的方法可以处理请求的，类servlet中的doGet, doPost
     * @return
     * 返回值: ModeLAndView表示本次请求的处理結果
     * Model:数据，请求处理完成后，要显示给用户的数据
     * View: 视图，比如jsp等等。
     */
    @RequestMapping(value = "/some.do")
    public ModelAndView doSome(){
        //处理some.do请求了。相 当于service调用处理完成
        ModelAndView mv=new ModelAndView();
        //添加数据，框架在请求的最后把数据放入到request作用域。
        //request . setAttribute("msg" , "欢迎使用springmvc做web开发");
        mv.addObject("msg","欢迎使用springmvc做web开发！");
        mv.addObject("fun","指定的是doSome方法");
        //指定视图，指定视图的完整路径
        //框架对视图执行的forward操作，request. getRequestDi spather("/show . jsp). forward(...)
        mv.setViewName("/show.jsp");
        return mv;
    }
}

```

show.jsp

```jsp
<%@ page contentType="text/html;charset=UTF-8" language="java" %>
<html>
<head>
    <title>Title</title>
</head>
<body>
    <h3>show.jsp</h3><br>
    <h3>msg数据：${msg}</h3>
    <h3>fun数据：${fun}</h3>
</body>
</html>
```

8. 在springmvc.xml声明组件扫描器

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="
         http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context.xsd">
        <!--声明组件扫描器 -->
        <context:component-scan base-package="com.dreamcold.controller"/>
</beans>
```

9. 配置tomcat服务器，运行测试该项目

1. 添加配置

![image-20210629230914102](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210629230914102.png)

- 点击"+"号

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210629234040321.png" alt="image-20210629234040321" style="zoom:50%;" />

- 选择tomcat服务器

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210629234214701.png" alt="image-20210629234214701" style="zoom:50%;" />

- 选择服务器路径和监听端口

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210629234319356.png" alt="image-20210629234319356" style="zoom:50%;" />

- 添加要部署的项目

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210629234414528.png" alt="image-20210629234414528" style="zoom:50%;" />

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210629234510868.png" alt="image-20210629234510868" style="zoom:50%;" />

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210629234558948.png" alt="image-20210629234558948" style="zoom:50%;" />

- 运行项目

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210629234645330.png" alt="image-20210629234645330" style="zoom:67%;" />

11. 浏览器访问http://localhost:8080/ch0_hello_springmvc/

![image-20210629233736992](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210629233736992.png)

12. 向controller发起请求

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210629233801850.png" alt="image-20210629233801850" style="zoom:50%;" />







