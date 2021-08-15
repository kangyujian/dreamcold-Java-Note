# Spring入门

## Spring程序开发步骤

1. 导入Spring开发的基本包坐标
2. 编写Dao接口和实现类
3. 创建Spring核心配置文件
4. 在Spring配置文件中配置UserDaolmpl
5. 使用Spring的API获得Bean实例

![image-20210718112814144](C:\Users\rtx2070\AppData\Roaming\Typora\typora-user-images\image-20210718112814144.png)

## Spring快速入门

1. 导入相应的spring依赖

```xml
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.0.5.RELEASE</version>
        </dependency>
```

2. 创建userDao接口，该接口中有save方法

```java
package com.dreamcold.dao;

public interface UserDao {
    public void save();
}
```

3. 创建userDao接口的对应实现类UserDaoImpl

```java
package com.dreamcold.dao.impl;

import com.dreamcold.dao.UserDao;

public class UserDaoImpl implements UserDao {
    public void save() {
        System.out.println("save running...");
    }
}
```

4. 在resource目录下创建spring的配置文件applicationContext.xml

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="userDao" class="com.dreamcold.dao.impl.UserDaoImpl"></bean>
</beans>
```

5. 运行测试是否对象成功在ioc容器中创建，获取到对象并调用其中的方法

```java
package com.dreamcold.demo;

import com.dreamcold.dao.UserDao;
import com.dreamcold.dao.impl.UserDaoImpl;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

public class Demo {
    public static void main(String[] args) {
        ApplicationContext context=new ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao = context.getBean("userDao", UserDao.class);
        userDao.save();
    }
}
```

## Spring的开发步骤

1. 导入坐标
2. 创建Bean
3. 创建applicationContext.xml
4. 在配置文件中进行配置
5. 创建ApplicationContext对象getBean
