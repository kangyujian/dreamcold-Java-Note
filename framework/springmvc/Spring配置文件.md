# Spring配置文件

## Bean标签的基本配置

用于配置对象交由**Spring来创建**。
默认情况下它调用的是类中的**无参构造函数**,如果没有无参构造函数则不能创建成功。
基本属性:

- id: Bean实例在Spring容器中的唯一标识
- class: Bean的全限定名称

## Bean标签的范围配置



![image-20210718115320650](C:\Users\rtx2070\AppData\Roaming\Typora\typora-user-images\image-20210718115320650.png)

1. 引入junit测试包

```xml
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.11</version>
            <scope>test</scope>
        </dependency>
```

2. 编写测试类来测试scope范围

```java
 @Test
    //测试scope中的属性
    public void testSingleton(){
        ApplicationContext context=new ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao1 = context.getBean("userDao", UserDao.class);
        System.out.println(userDao1);
        UserDao userDao2 = context.getBean("userDao", UserDao.class);
        System.out.println(userDao2);
    }
```



3. 在applicationContext.xml中修改类的scope的范围为singleton

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="userDao" class="com.dreamcold.dao.impl.UserDaoImpl" scope="singleton"></bean>
</beans>
```

效果

```
com.dreamcold.dao.impl.UserDaoImpl@5a1c0542
com.dreamcold.dao.impl.UserDaoImpl@5a1c0542
```

为了展示对象创建的实际，我们重写userDao类的无参构造方法

```java
package com.dreamcold.dao.impl;

import com.dreamcold.dao.UserDao;

public class UserDaoImpl implements UserDao {

    public UserDaoImpl() {
        System.out.println("无参构造器执行！");
    }

    public void save() {
        System.out.println("save running...");
    }
}
```

效果

```
无参构造器执行！
com.dreamcold.dao.impl.UserDaoImpl@d21a74c
com.dreamcold.dao.impl.UserDaoImpl@d21a74c
```
scope为singleton的bean，在加载整个配置文件的时候创建对象

4. 在applicationContext.xml中修改类的scope的范围为prototype

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="userDao" class="com.dreamcold.dao.impl.UserDaoImpl" scope="prototype"></bean>
</beans>
```

效果

```
com.dreamcold.dao.impl.UserDaoImpl@23faf8f2
com.dreamcold.dao.impl.UserDaoImpl@2d6eabae
```

scope为singleton的bean，在获取对象的时候创建对象

```
无参构造器执行！
com.dreamcold.dao.impl.UserDaoImpl@424e1977
无参构造器执行！
com.dreamcold.dao.impl.UserDaoImpl@10d68fcd
```

**总结**

1)当scope的取值为singleton时
Bean的实例化个数: 1个
Bean的实例化时机:当Spring核心文件被加载时,实例化配置的Bean实例
Bean的生命周期:

- 对象创建: 当应用加载，创建容器时，对象就被创建了
- 对象运行: 只要容器在，对象一直活着
- 对象销毁: 当应用卸载,销毁容器时，对象就被销毁了

2)当scope的取值为prototype时
Bean的实例化个数:多个
Bean的实例化时机:当调用getBean0方法时实例化Bean

- 对象创建: 当使用对象时，创建新的对象实例
- 对象运行: 只要对象在使用中，就一直活着
- 对象销毁: 当对象长时间不用时，被Java的垃圾回收器回收了

## Bean的生命周期配置

- init-method:指定类中的初始化方法名称
- destroy-method:指定类中销毁方法名称

1. 创建UserDaoImpl中的初始化方法和销毁方法

```java
package com.dreamcold.dao.impl;

import com.dreamcold.dao.UserDao;

public class UserDaoImpl implements UserDao {

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
        System.out.println("save running...");
    }
}
```

2. 在对应的配置文件中，指定相应胡初始化方法以及销毁方法

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="userDao" class="com.dreamcold.dao.impl.UserDaoImpl" init-method="init" destroy-method="destroy"></bean>
</beans>
```

3. 测试相应胡初始化方法以及对应的销毁方法

```java
    @Test
    //测试初始化方法以及销毁方法
    public void testInitAndDestory(){
        ClassPathXmlApplicationContext context=new ClassPathXmlApplicationContext("applicationContext.xml");
        UserDao userDao1 = context.getBean("userDao", UserDao.class);
        System.out.println(userDao1);
        context.close();
    }
```

4. 初始化方法在无参构造器后面执行，手动关闭容器，会调用相应的销毁方法

```java
无参构造器执行！
执行初始化方法!
com.dreamcold.dao.impl.UserDaoImpl@396f6598
执行销毁方法！
```

## Bean实例化的三种方式

### 无参构造方法实例化

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="userDao" class="com.dreamcold.dao.impl.UserDaoImpl" scope="prototype"></bean>
</beans>
```

### 工厂静态方法实例化

1. 创建工厂，工厂返回对应类型的对象

```java
package com.dreamcold.factory;

import com.dreamcold.dao.UserDao;
import com.dreamcold.dao.impl.UserDaoImpl;

public class StaticFactory {
    public static UserDao getUserDao(){
        return new UserDaoImpl();
    }
}

```

2. 在配置文件中指定用工厂方法来创建对象

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--    <bean id="userDao" class="com.dreamcold.dao.impl.UserDaoImpl" init-method="init" destroy-method="destroy"></bean>-->
        <bean id="userDao" class="com.dreamcold.factory.StaticFactory" factory-method="getUserDao"></bean>
</beans>
```

### 工厂实例方法实例化

1. 创建DynamicFactory类，该类中的静态方法返回对应的对象

```java
package com.dreamcold.factory;

import com.dreamcold.dao.UserDao;
import com.dreamcold.dao.impl.UserDaoImpl;

public class DynamicFactory {
    public UserDao getUserDao(){
        return new UserDaoImpl();
    }
}
```

2. 创建对应的配置文件，先创建工厂类对象，再调用工厂类中的对象的方法

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="fatory" class="com.dreamcold.factory.DynamicFactory"></bean>
    <bean id="userDao" factory-bean="fatory" factory-method="getUserDao"></bean>
</beans>
```





