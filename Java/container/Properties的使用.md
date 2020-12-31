# Properties使用

## 简介

- Properties 继承于 Hashtable。表示一个持久的属性集，属性列表以key-value的形式存在，key和value都是字符串。
- Properties 类被许多Java类使用。例如，在获取环境变量时它就作为System.getProperties()方法的返回值。
- 我们在很多**需要避免硬编码的应用场景**下需要使用properties文件来加载程序需要的配置信息，比如JDBC、MyBatis框架等。Properties类则是properties文件和程序的中间桥梁，不论是从properties文件读取信息还是写入信息到properties文件都要经由Properties类。

## 读取配置文件

用`Properties`读取配置文件非常简单。Java默认配置文件以`.properties`为扩展名，每行以`key=value`表示，以`#`课开头的是注释。以下是一个典型的配置文件：

test.properties

```properties
# setting.properties
last_open_file=/data/hello.txt
auto_save_interval=60
```

可以从文件系统读取这个`.properties`文件：

```java
package com.dreamcold.container;

import java.io.FileInputStream;
import java.io.IOException;
import java.util.Properties;

public class Demo15 {
    public static void main(String[] args) throws IOException {
        String f= "test.properties";
        Properties properties=new Properties();
        properties.load((new FileInputStream(f)));
        String filepath = properties.getProperty("last_open_file");
        String interval = properties.getProperty("auto_save_interval", "120");
        System.out.println(filepath);
        System.out.println(interval);
    }
}
```

注意文件路径：

<img src="images/image-20201231155110949.png" alt="image-20201231155110949" style="zoom:67%;" />

关于java文件路径参考：https://blog.csdn.net/u011983531/article/details/48443195/

可见，用`Properties`读取配置文件，一共有三步：

1. 创建`Properties`实例；
2. 调用`load()`读取文件；
3. 调用`getProperty()`获取配置。

调用`getProperty()`获取配置时，如果key不存在，将返回`null`。我们还可以提供一个默认值，这样，当key不存在的时候，就返回默认值。

也可以从classpath读取`.properties`文件，因为`load(InputStream)`方法接收一个`InputStream`实例，表示一个字节流，它不一定是文件流，也可以是从jar包中读取的资源流：

也可以从classpath读取`.properties`文件，因为`load(InputStream)`方法接收一个`InputStream`实例，表示一个字节流，它不一定是文件流，也可以是从jar包中读取的资源流：

```java
Properties props = new Properties();
props.load(getClass().getResourceAsStream("/common/setting.properties"));
```

如果有多个`.properties`文件，可以反复调用`load()`读取，后读取的key-value会覆盖已读取的key-value：

```java
Properties props = new Properties();
props.load(getClass().getResourceAsStream("/common/setting.properties"));
props.load(new FileInputStream("C:\\conf\\setting.properties"));
```

上面的代码演示了`Properties`的一个常用用法：可以把默认配置文件放到classpath中，然后，根据机器的环境编写另一个配置文件，覆盖某些默认的配置。

`Properties`设计的目的是存储`String`类型的key－value，但`Properties`实际上是从`Hashtable`派生的，它的设计实际上是有问题的，但是为了保持兼容性，现在已经没法修改了。除了`getProperty()`和`setProperty()`方法外，还有从`Hashtable`继承下来的`get()`和`put()`方法，这些方法的参数签名是`Object`，我们在使用`Properties`的时候，不要去调用这些从`Hashtable`继承下来的方法。

## 写入配置文件

如果通过`setProperty()`修改了`Properties`实例，可以把配置写入文件，以便下次启动时获得最新配置。写入配置文件使用`store()`方法：

```java
package com.dreamcold.container;

import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;
import java.util.Properties;

public class Demo16 {
    public static void main(String[] args) throws IOException {
        Properties properties=new Properties();
        properties.setProperty("name","dreancold");
        properties.setProperty("age","21");
        properties.store(new FileOutputStream("ouput.properties"),"comments");

    }
}
```

结果

<img src="images/image-20201231160013688.png" alt="image-20201231160013688" style="zoom: 67%;" />![image-20201231160028950](images/image-20201231160028950.png)

<img src="images/image-20201231160013688.png" alt="image-20201231160013688" style="zoom: 67%;" />![image-20201231160028950](images/image-20201231160028950.png)

## 编码

早期版本的Java规定`.properties`文件编码是ASCII编码（ISO8859-1），如果涉及到中文就必须用`name=\u4e2d\u6587`来表示，非常别扭。从JDK9开始，Java的`.properties`文件可以使用UTF-8编码了。

不过，需要注意的是，由于`load(InputStream)`默认总是以ASCII编码读取字节流，所以会导致读到乱码。我们需要用另一个重载方法`load(Reader)`读取：

```
Properties props = new Properties();
props.load(new FileReader("settings.properties", StandardCharsets.UTF_8));
```

就可以正常读取中文。`InputStream`和`Reader`的区别是一个是字节流，一个是字符流。字符流在内存中已经以`char`类型表示了，不涉及编码问题。

## 小结

Java集合库提供的`Properties`用于读写配置文件`.properties`。`.properties`文件可以使用UTF-8编码。

可以从文件系统、classpath或其他任何地方读取`.properties`文件。

读写`Properties`时，注意仅使用`getProperty()`和`setProperty()`方法，不要调用继承而来的`get()`和`put()`等方法。



## 引用自

廖雪峰官方网站：https://www.liaoxuefeng.com/wiki/1252599548343744/1265119084411136

