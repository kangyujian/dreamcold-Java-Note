# Spring数据源配置

## 数据源（连接池）的作用

- 数据源(连接池)是提高程序性能如出现的
- 事先实例化数据源，初始化部分连接资源
- 使用连接资源时从数据源中获取
- 使用完毕后将连接资源归还给数据源

常见的数据源(连接池): `DBCP. C3P0. BoneCP. Druid`等

## 数据源开发步骤

- 导入数据源的坐标和数据库驱动坐标
- 创建数据源对象
- 设置数据源的基本连接数据
- 使用数据源获取连接资源和归还连接资源

## 数据源的开发与创建

1. 导入相关依赖

```xml
    <dependencies>
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.32</version>
        </dependency>
        <dependency>
            <groupId>c3p0</groupId>
            <artifactId>c3p0</artifactId>
            <version>0.9.1.1</version>
        </dependency>
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.1.10</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.13</version>
        </dependency>
    </dependencies>
```

2. 创建`c3p0`连接池输入参数，获取连接

```java
        ComboPooledDataSource dataSource=new ComboPooledDataSource();
        dataSource.setDriverClass("com.mysql.jdbc.Driver");
        dataSource.setJdbcUrl("jdbc:mysql://localhost:3306/test");
        dataSource.setUser("root");
        dataSource.setPassword("root");
        Connection connection=dataSource.getConnection();
        System.out.println(connection);
        connection.close();
```

3. 创建`druid`连接池输入参数，获取参数

```java
     	DruidDataSource dataSource=new DruidDataSource();
        dataSource.setDriverClassName("com.mysql.jdbc.Driver");
        dataSource.setUrl("jdbc:mysql://localhost:3306/test");
        dataSource.setUsername("root");
        dataSource.setPassword("root");
        DruidPooledConnection connection=dataSource.getConnection();
        System.out.println(connection);
        connection.close();
```

4. 创建配置文件`jdbc.properties`在resource目录下

```properties
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/test
jdbc.username=root
jdbc.password=root
```

5. 读取配置文件创建`c3p0`连接池

```java
//创建c3p0连接池，读取配置文件  		
		ResourceBundle rb=ResourceBundle.getBundle("jdbc.driver");
        String driver=rb.getString("jjdbc.driver");
        String url=rb.getString("jdbc.url");
        String userName=rb.getString("jdbc.username");
        String passWord=rb.getString("jdbc.password");
        ComboPooledDataSource dataSource=new ComboPooledDataSource();
        dataSource.setDriverClass(driver);
        dataSource.setJdbcUrl(url);
        dataSource.setUser(userName);
        dataSource.setPassword(passWord);
        Connection connection=dataSource.getConnection();
        System.out.println(connection);
        connection.close();
```

## Spring配置数据源

1. 导入spring依赖,可以将`DataSource`的创建权交由Spring容器去完成

```xml
  <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>5.2.6.RELEASE</version>
  </dependency>
```

2. 创建spring的配置文件

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="com.mysql.jdbc.Driver"></property>
        <property name="jdbcUrl" value="jdbc:mysql://localhost:3306/test"></property>
        <property name="user" value="root"></property>
        <property name="password" value="root"></property>
    </bean>
</beans>
```

3. 测试连接

```java
        ApplicationContext app=new ClassPathXmlApplicationContext("applicationConext.xml");
        DataSource dataSource = (DataSource) app.getBean("dataSource");
        System.out.println(dataSource);
        Connection connection=dataSource.getConnection();
        System.out.println(connection);
        connection.close();
```

