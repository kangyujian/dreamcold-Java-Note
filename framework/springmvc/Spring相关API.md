# Spring相关API

## ApplicationContext的继承体系

applicationContext:接口类型，代表应用上下文，可以通过其实例获得Spring容器中的Bean对象

![img](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91cGxvYWQtaW1hZ2VzLmppYW5zaHUuaW8vdXBsb2FkX2ltYWdlcy81NTUxMDYxLWM2MTE2NjA5ZDk5NmM5OGYucG5nP2ltYWdlTW9ncjIvYXV0by1vcmllbnQvc3RyaXB8aW1hZ2VWaWV3Mi8yL3cvMTIwMC9mb3JtYXQvd2VicA?x-oss-process=image/format,png)

## ApplicationContext的实现类

1) ClassPathXmlApplicationContext
它是从类的根路径下加载配置文件推荐使用这种

2) FileSystemXmlApplicationContext
它是从磁盘路径上加载配置文件，配置文件可以在磁盘的任意位置。

3) AnnotationConfigApplicationContext
当使用注解配置容器对象时，需要使用此类来创建spring容器。它用来读取注解。

## getBean()方法使用

![image-20210719131210403](F:\notebook\dreamcold-Java-Note\framework\springmvc\images\image-20210719131210403.png)

1. 可以直接指定取到的bean的类型

```java
UserService userService=app.getBean(UserService.class);
```

2. 注意第一种方式可以允许容器中存在多个相同类型的bean，而第二种方式不允许容器中出现相同类型的bean，id具有唯一性

