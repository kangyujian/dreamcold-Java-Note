# AOP

## 什么是AOP
AOP(Aspect Oriented Programming)翻译为面向切面编程，通过预编译方式和运行期间动态代理实现程序功能的统一维护的一种技术。AOP是OOP的延续，是软件开发中的一个热点，也是Spring框架中的一个重要内容，是函数式编程的一种衍生泛型，利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑之间的耦合度降低，提高程序的可重用行，同时提高开发效率。


![](https://gitee.com/kangyujian/notebook-images/raw/master/images/2020-10-03-14-41-24.png)

1. 我们的核心业务逻辑就是耕山改查
2. 但是公司现在要加入日志的功能
3. 我们不可以直接来修改代码，需要通过AOP来讲这些非核心业务的周边功能定义为切面，使得核心业务功能和切面功能分别独立进行开发，然后再把切面功能和核心业务功能编织在一起，就叫做AOP。

## AOP在Spring中的作用

提供声明式事务，允许用户自定义切面
一下的名词需要了解下：

- 横切关注点：跨越应用程序多个模块的方法和功能，即是，与我们业务逻辑无关的，但是我们需要关注的部分，就是横切关注点，比如日志，安全、缓存、事务
- 切面(ASPECT):横切关注点呗模块化的特殊对象，即它是一个类(日志类)
- 通知(ADVICE):切面所必须完成的工作，即他是类中一个方法(日志类中的一个方法)
- 目标(Target)：被通知的对象(接口)
- 代理(Proxy):向目标对象应用通知后创建的对象(生成的代理类)(代理类)
- 切入点(PointCut):切面通知执行的地点的定义(在哪里执行)
- 连接点(JoinPoint):与切入点匹配的执行点

![](https://gitee.com/kangyujian/notebook-images/raw/master/images/2020-10-03-14-54-26.png)

在SpringAOP中，通过Advice定义横切逻辑，Spring中支持5种类型的Advice

![](https://gitee.com/kangyujian/notebook-images/raw/master/images/2020-10-03-14-55-46.png)

1. 前置通知：在前面加一个方法
2. 后置通知：在后面加一个方法
3. 环绕通知：在前后都加上一个方法
4. 异常抛出通知：方法抛出异常
5. 引介通知：类中增加新的方法属性

## 使用Spring实现Aop

在使用Aop织入，需要导入依赖包!
```xml
<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
   <groupId>org.aspectj</groupId>
   <artifactId>aspectjweaver</artifactId>
   <version>1.9.4</version>
</dependency>
```

1. 首先创建一个用户的接口

```java
package com.dreamcold.demo.service;

public interface UserService {
    public void insert();
    public void update();
    public void delete();
    public void find();
}

```

2. 编写对应的实现类

```java
package com.dreamcold.demo.service;

public class UserServiceImpl implements UserService {
    @Override
    public void insert() {
        System.out.println("增加了一个用户!");
    }

    @Override
    public void update() {
        System.out.println("更新了一个用户!");
    }

    @Override
    public void delete() {
        System.out.println("删除了一个用户!");
    }

    @Override
    public void find() {
        System.out.println("查找一个用户!");
    }
}

```

### 方法一：使用Spring的Api接口

编写两个增强类，一个前置增强类，另外一个后置增强类
1. 前置增强类

```java
package com.dreamcold.demo.log;

import org.springframework.aop.MethodBeforeAdvice;

import java.lang.reflect.Method;

public class Log implements MethodBeforeAdvice {

    //method:要执行的目标对象的方法
    //Object:是一个目标对象
    //args:参数
    @Override
    public void before(Method method, Object[] objects, Object o) throws Throwable {
        System.out.println(o.getClass().getName()+"类的"+method.getName()+"方法!");
    }


} 
```

2. 后置增强类

```java
package com.dreamcold.demo.log;


import org.springframework.aop.AfterAdvice;
import org.springframework.aop.AfterReturningAdvice;

import java.lang.reflect.Method;

public class AfterLog implements AfterReturningAdvice {
    @Override
    //returnValue返回值
    public void afterReturning(Object o, Method method, Object[] objects, Object o1) throws Throwable {
        System.out.println("执行了"+method.getName()+"方法"+",返回了"+o);
    }
}

```
之后在Spring文件中注册并实现aop切入实现，注意导入约束

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 注册配置-->
    <bean id="userService" class="com.dreamcold.demo.service.UserServiceImpl">
    </bean>

    <bean id="log" class="com.dreamcold.demo.log.Log">
    </bean>

    <bean id="afterLog" class="com.dreamcold.demo.log.AfterLog">
    </bean>


    <!-- 配置Aop  -->
    <aop:config>
<!--        切入点，我们在哪个地方要去执行，方法-->
<!--        execution()是要执行的位置-->
<!--        修饰词 返回值 类名 方法名 参数-->
<!--        *（任意前面） UserServiceImpl类 的所有方法 的有任意的参数-->
        <aop:pointcut id="pointcut" expression="execution(* com.dreamcold.demo.service.UserServiceImpl.*(..))"/>
<!--        执行环绕增强-->
<!--        增强那个类， 在哪个切入点-->
        <aop:advisor advice-ref="log" pointcut-ref="pointcut"></aop:advisor>
        <aop:advisor advice-ref="afterLog" pointcut-ref="pointcut"></aop:advisor>

    </aop:config>


</beans>
```

测试

```java
package com.dreamcold.demo.test;

import com.dreamcold.demo.service.UserService;
import com.dreamcold.demo.service.UserServiceImpl;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = (UserService) context.getBean("userService");
        userService.find();
    }
}

```

![](https://gitee.com/kangyujian/notebook-images/raw/master/images/2020-10-09-13-41-53.png)

Aop的重要性：很重要，一定要理解其中的思路，主要是思想的理解这一块.

Spring的Aop就是将公共的事物(日志、安全等)和领域的业务结合起来，当执行领域业务时候，会把公共业务加进来，实现公共业务的重复利用，业务领域会更加的纯粹，程序员会专注于业务，其本质还是动态代理。


### 方法二：自定义类来实现AOP

目标业务类不变依旧是userServiceImpl

第一步：写我们自己的一个切入类

```java
package com.dreamcold.demo.diy;

public class DiyPointCut {
    public void before(){
        System.out.println("======方法执行之前=========");
    }

    public void after(){
        System.out.println("=======方法执行之后==========");
    }
}

```
在spring中的配置
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 注册配置-->
    <bean id="userService" class="com.dreamcold.demo.service.UserServiceImpl">
    </bean>

    <bean id="log" class="com.dreamcold.demo.log.Log">
    </bean>

    <bean id="afterLog" class="com.dreamcold.demo.log.AfterLog">
    </bean>


<!--    第二种方式-->
    <bean id="diy" class="com.dreamcold.demo.diy.DiyPointCut"></bean>

    <aop:config>
<!--        自定义切面，ref是要引用的类-->
        <aop:aspect ref="diy">
<!--            切入点-->
            <aop:pointcut id="point" expression="execution(* com.dreamcold.demo.service.UserServiceImpl.*(..))"/>
<!--            在此之前执行什么方法-->
            <aop:before method="before" pointcut-ref="point"></aop:before>
            <aop:after method="after" pointcut-ref="point"></aop:after>
        </aop:aspect>
    </aop:config>

</beans>
```
测试结果
```java
package com.dreamcold.demo.test;

import com.dreamcold.demo.service.UserService;
import com.dreamcold.demo.service.UserServiceImpl;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = (UserService) context.getBean("userService");
        userService.find();
    }
}

```

![](https://gitee.com/kangyujian/notebook-images/raw/master/images/2020-10-09-14-02-52.png)


### 第三种方式:使用注解方式实现AOP

1. 使用注解实现一个注解实现的增强类

```java
package com.dreamcold.demo.diy;

import org.aspectj.lang.annotation.After;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;

//标注这个类是一个切面
@Aspect
public class AnnotionPointCut {

    @Before("execution(* com.dreamcold.demo.service.UserServiceImpl.*(..))")
    public void before(){
        System.out.println("======开始打印日志===========");
    }

    @After("execution(* com.dreamcold.demo.service.UserServiceImpl.*(..))")
    public void after(){
        System.out.println("======结束打印日志===========");
    }
}

```

2. 在spring配置文件中，注册bean，并支持注解的配置

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/aop
       http://www.springframework.org/schema/aop/spring-aop.xsd">

    <!-- 注册配置-->
    <bean id="userService" class="com.dreamcold.demo.service.UserServiceImpl">
    </bean>

    <bean id="log" class="com.dreamcold.demo.log.Log">
    </bean>

    <bean id="afterLog" class="com.dreamcold.demo.log.AfterLog">
    </bean>

<!--    注册切面-->
    <bean class="com.dreamcold.demo.diy.AnnotionPointCut" id="annotionPointCut"></bean>
<!--    开启注解支持-->
    <aop:aspectj-autoproxy></aop:aspectj-autoproxy>
</beans>
```

3. 测试

```java
package com.dreamcold.demo.test;

import com.dreamcold.demo.service.UserService;
import com.dreamcold.demo.service.UserServiceImpl;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class MyTest {
    public static void main(String[] args) {
        ApplicationContext context = new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = (UserService) context.getBean("userService");
        userService.find();
    }
}

```

![](https://gitee.com/kangyujian/notebook-images/raw/master/images/2020-10-09-14-16-06.png)


通过aop命名空间的<aop:aspectj-autoproxy />声明自动为spring容器中那些配置@aspectJ切面的bean创建代理，织入切面。当然，spring 在内部依旧采用AnnotationAwareAspectJAutoProxyCreator进行自动代理的创建工作，但具体实现的细节已经被<aop:aspectj-autoproxy />隐藏起来了

<aop:aspectj-autoproxy />有一个proxy-target-class属性，默认为false，表示使用jdk动态代理织入增强，当配为<aop:aspectj-autoproxy  poxy-target-class="true"/>时，表示使用CGLib动态代理技术织入增强。不过即使proxy-target-class设置为false，如果目标类没有声明接口，则spring将自动使用CGLib动态代理。

