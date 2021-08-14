# Spring依赖注入

## 引例

1. 创建service中的UserService接口，并创建其对应的实现类

UserService.java

```java
package com.dreamcold.service;

public interface UserService {
    public void save();
}
```

UserServiceImpl.java

```java
package com.dreamcold.service.impl;

import com.dreamcold.dao.UserDao;
import com.dreamcold.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class UserServiceImpl implements UserService {
    public void save() {
        ClassPathXmlApplicationContext context=new ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao1 = context.getBean("userDao", UserDao.class);
        userDao1.save();
        System.out.println(userDao1);
        context.close();
    }
}
```

2. 在applicationContext.xml中注册相应的UserServiceImpl实现类

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="fatory" class="com.dreamcold.factory.DynamicFactory"></bean>
    <bean id="userDao" factory-bean="fatory" factory-method="getUserDao"></bean>

    <bean id="userService" class="com.dreamcold.service.impl.UserServiceImpl"></bean>
</beans>
```

3. 模拟controller中调用service层中的方法

```java
package com.dreamcold.controller;

import com.dreamcold.service.UserService;
import com.dreamcold.service.impl.UserServiceImpl;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class UserController {
    public static void main(String[] args) {
        ApplicationContext context=new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = context.getBean("userService", UserService.class);
        userService.save();
    }
}
```

4. 运行结果

```
无参构造器执行！
无参构造器执行！
save running...
com.dreamcold.dao.impl.UserDaoImpl@7085bdee
```

## Bean的依赖注入分析

目前UserService实例和UserDao实例都存在与Spring容器中，当前的做法是在容器外部获得UserService实例和UserDao实例，然后在程序中进行结合。

![image-20210718200700718](C:\Users\rtx2070\AppData\Roaming\Typora\typora-user-images\image-20210718200700718.png)

因为UserService和UserDao都在Spring容器中，而最终程序直接使用的是UserService,所以可以在Spring容器中，将UserDao设置到UserService内部。

![image-20210718200809938](C:\Users\rtx2070\AppData\Roaming\Typora\typora-user-images\image-20210718200809938.png)

## 依赖注入的概念

- 依赖注入(Dependencylnjection) :它是Spring框架核心I0C的具体实现。在编写程序时，通过控制反转,把对象的创建交给了Spring,但是代码中不可能出现没有依赖的情况。

- I0C解耦只是降低他们的依赖关系,但不会消除。例如:业务层仍会调用持久层的方法。
- 那这种业务层和持久层的依赖关系，在使用Spring之后，就让Spring来维护了。
- 简单的说，就是坐等框架把持久层对象传入业务层,而不用我们自己去获取。

## Bean依赖注入的方式

怎么将UserDao怎样注入到UserService内部呢?

- 构造方法
- set方法

### set依赖注入

1. 首先为UserServiceImpl创建setter方法

```java
package com.dreamcold.service.impl;

import com.dreamcold.dao.UserDao;
import com.dreamcold.service.UserService;

public class UserServiceImpl implements UserService {
    
    private UserDao userDao;

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void save() {
       userDao.save();
    }
}
```

2. 在applicationContext.xml配置文件中注册相应的bean,并通过setter方法来完成相应属性的创建

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean class="com.dreamcold.dao.impl.UserDaoImpl" id="userDao"></bean>

    <bean class="com.dreamcold.service.impl.UserServiceImpl" id="userService">
            <property name="userDao" ref="userDao"></property>
    </bean>
</beans>
```

3. 测试controller层完成相应的方法调用

```java
package com.dreamcold.controller;

import com.dreamcold.service.UserService;
import com.dreamcold.service.impl.UserServiceImpl;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class UserController {
    public static void main(String[] args) {
        ApplicationContext context=new ClassPathXmlApplicationContext("applicationContext.xml");
        UserService userService = context.getBean("userService", UserService.class);
        userService.save();
    }
}
```

4. 测试效果，执行成功

```
无参构造器执行！
save running...
```

### p命名空间注入

1. p命名空间是set注入的一种简便方法，其特殊在于其其需要引入p命名空间

```xml
       xmlns:p="http://www.springframework.org/schema/p"
```

2. 采用p空间注入不需要写property属性，但是注意p:属性名是普通属性的注入，p:属性名-ref是引用类型的注入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean class="com.dreamcold.dao.impl.UserDaoImpl" id="userDao"></bean>
    <bean class="com.dreamcold.service.impl.UserServiceImpl" id="userService" p:userDao-ref="userDao">
    </bean>
</beans>
```

### 有参构造

1. 为UserserviceImpl创建构造器方法

```java
package com.dreamcold.service.impl;

import com.dreamcold.dao.UserDao;
import com.dreamcold.service.UserService;

public class UserServiceImpl implements UserService {

    private UserDao userDao;

    public UserServiceImpl(UserDao userDao) {
        this.userDao = userDao;
    }

    public void setUserDao(UserDao userDao) {
        this.userDao = userDao;
    }

    public void save() {
       userDao.save();
    }
}
```

2. 完成相应的属性注入，通过构造器xml配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean class="com.dreamcold.dao.impl.UserDaoImpl" id="userDao"></bean>
    <bean class="com.dreamcold.service.impl.UserServiceImpl" id="userService" >
        <constructor-arg name="userDao" ref="userDao"></constructor-arg>
    </bean>
</beans>
```

## Bean依赖注入的类型

上面的操作，都是注入的引用Bean,除了对象的引用可以注入，普通数据类型,集合等都可以在容器中进行注入。注入数据的三种数据类型

- 普通数据类型
- 引用数据类型
- 集合数据类型

### 普通属性注入

1. 首先为UserServiceImpl类加入相应的setter注入方法,定义userName和age属性

```java
package com.dreamcold.dao.impl;

import com.dreamcold.dao.UserDao;

public class UserDaoImpl implements UserDao {

    private String userName;
    private Integer age;

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public UserDaoImpl() {
        System.out.println("无参构造器执行！");
    }

    public void init(){
        System.out.println("执行初始化方法!");
    }

    public void destroy(){
        System.out.println("执行销毁方法！");
    }

    public void save() {
        System.out.println(userName+"==="+age);
        System.out.println("save running...");
    }
}
```

2. 在配置文件中注入普通属性

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean class="com.dreamcold.dao.impl.UserDaoImpl" id="userDao">
        <property name="age" value="12">
        </property>
        <property name="userName" value="zhangsan">
        </property>
    </bean>
    <bean class="com.dreamcold.service.impl.UserServiceImpl" id="userService" >
        <constructor-arg name="userDao" ref="userDao"></constructor-arg>
    </bean>
</beans>
```

### 集合注入

1. 创建domain.User类

```java
package com.dreamcold.domain;

public class User {
    private String name;
    private String addr;

    public User() {
    }

    public User(String name, String addr) {
        this.name = name;
        this.addr = addr;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public String getAddr() {
        return addr;
    }

    public void setAddr(String addr) {
        this.addr = addr;
    }
}
```

2. 为UserDaoImpl类加入List、Map、Properties类型的属性

```java
package com.dreamcold.dao.impl;

import com.dreamcold.dao.UserDao;
import com.dreamcold.domain.User;

import java.util.List;
import java.util.Map;
import java.util.Properties;

public class UserDaoImpl implements UserDao {

    private List<String> strList;
    private Map<String, User> userMap;
    private Properties properties;

    public void setStrList(List<String> strList) {
        this.strList = strList;
    }

    public void setUserMap(Map<String, User> userMap) {
        this.userMap = userMap;
    }

    public void setProperties(Properties properties) {
        this.properties = properties;
    }

    private String userName;
    private Integer age;

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public UserDaoImpl() {
        System.out.println("无参构造器执行！");
    }

    public void init(){
        System.out.println("执行初始化方法!");
    }

    public void destroy(){
        System.out.println("执行销毁方法！");
    }

    public void save() {
        System.out.println(userName+"==="+age);
        System.out.println(strList);
        System.out.println(properties);
        System.out.println(userMap);
        System.out.println("save running...");
    }
}
```

3. 配置文件相关注入

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p" xmlns:util="http://www.springframework.org/schema/util"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">

    <bean class="com.dreamcold.dao.impl.UserDaoImpl" id="userDao">
        <property name="age" value="12">
        </property>
        <property name="userName" value="zhangsan">
        </property>
        <property name="strList">
            <list>
                <value>aaa</value>
                <value>bbb</value>
                <value>ccc</value>
            </list>
        </property>
        <property name="userMap">
            <map>
                <entry key="xiaoming" value-ref="user1"></entry>
                <entry key="kitty" value-ref="user2"></entry>
            </map>
        </property>
        <property name="properties">
            <props>
                <prop key="aaa">aaa1</prop>
                <prop key="bbb">bbb1</prop>
            </props>
        </property>
    </bean>

    <bean class="com.dreamcold.domain.User" id="user1">
        <property name="name" value="dreamcold">
        </property>
        <property name="addr" value="beijing">
        </property>
    </bean>

    <bean class="com.dreamcold.domain.User" id="user2">
        <property name="name" value="ketty">
        </property>
        <property name="addr" value="nanjing">
        </property>
    </bean>


    <bean class="com.dreamcold.service.impl.UserServiceImpl" id="userService" >
        <constructor-arg name="userDao" ref="userDao"></constructor-arg>
    </bean>
</beans>
```

4. 效果

```
无参构造器执行！
zhangsan===12
[aaa, bbb, ccc]
{bbb=bbb1, aaa=aaa1}
{xiaoming=com.dreamcold.domain.User@7d0587f1, kitty=com.dreamcold.domain.User@5d76b067}
save running...
```

## 引入配置文件（分模块开发）

实际开发中，Spring的配置内容非常多,这就导致Spring配置很繁杂且体积很大，所以，可以将部分配置拆解到其他配置文件中，而在Spring主配置文件通过import标签进行加载

```xml
<import resource="applicationContext-xxx.xml"/>
```

1. 创建用户模块的配置文件在resource目录下

![image-20210719130044617](C:\Users\rtx2070\AppData\Roaming\Typora\typora-user-images\image-20210719130044617.png)

2. 在主配置文件中引入其他的配置文件

```xml
<import resource="applicationContext-user.xml"></import>
```



## 总结

```xml
<bean>标签
    id属性:在容器中Bean实例的唯一标识，不允许重复class属性:要实例化的Bean的全限定名
    scope属性:Bean的作用范围，常用是singleton (默认)和prototype<property>标签:属性注入
    name属性:属性名称
    value属性:注入的普通属性值ref属性:注入的对象引用值<list>标签
<map>标签
<properties>标签<constructor-arg>标签
<import>标签:导入其他的spring的分文件
```

