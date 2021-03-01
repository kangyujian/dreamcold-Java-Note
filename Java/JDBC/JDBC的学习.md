# JDBC

## JDBC概述

### 数据的持久化

- 持久化(persistence)：把数据保存到可掉电式存储设备中以供之后使用。大多数情况下，特别是企业级应用，
  数据持久化意味着将内存中的数据保存到硬盘上加以”固化”，而持久化的实现过程大多通过各种关系数据库来完
  成。
- 持久化的主要应用是将内存中的数据存储在关系型数据库中，当然也可以存储在磁盘文件、XML数据文件中。

<img src="images/image-20210226155800764.png" alt="image-20210226155800764" style="zoom:50%;" />

### Java中数据存储技术

- JDBC(Java Database Connectivity)是一个独立于特定数据库管理系统、通用的SQL数据库存取和操作的公共接
  口（一组API），定义了用来访问数据库的标准Java类库，（java.sql,javax.sql）使用这些类库可以以一种标准的方法、方便地访问数据库资源。
- JDBC为访问不同的数据库提供了一种统一的途径，为开发者屏蔽了一些细节问题。
- JDBC的目标是使Java程序员使用JDBC可以连接任何提供了JDBC驱动程序的数据库系统，这样就使得程序员无需
- 对特定的数据库系统的特点有过多的了解，从而大大简化和加快了开发过程。
- 如果没有JDBC，那么Java程序访问数据库时是这样的：

<img src="images/image-20210226155917653.png" alt="image-20210226155917653" style="zoom:50%;" />

有了JDBC，Java程序访问数据库时是这样的：

<img src="images/image-20210226155943480.png" alt="image-20210226155943480" style="zoom:50%;" />

- 总结如下：

<img src="images/image-20210226160027227.png" alt="image-20210226160027227" style="zoom:50%;" />

### JDBC体系结构

JDBC接口（API）包括两个层次：

- **面向应用的API：**Java API，抽象接口，供应用程序开发人员使用（连接数据库，执行SQL语句，获得结
  果）。
- **面向数据库的API**：Java Driver API，供开发商开发数据库驱动程序用。
  JDBC是sun公司提供一套用于数据库操作的接口，java程序员只需要面向这套接口编程即可。
  不同的数据库厂商，需要针对这套接口，提供不同实现。不同的实现的集合，即为不同数据库的驱动。
  ————面向接口编程

### JDBC程序编写的过程

<img src="images/image-20210226160204438.png" alt="image-20210226160204438" style="zoom:50%;" />

> 补充：ODBC(Open Database Connectivity，开放式数据库连接)，是微软在Windows平台下推出的。使用
> 者在程序中只需要调用ODBC API，由 ODBC 驱动程序将调用转换成为对特定的数据库的调用请求。



## 获取数据库连接

**连接方式一**

```java
package com.dreamcold.connect;

import java.sql.Connection;
import java.sql.Driver;
import java.sql.SQLException;
import java.util.Properties;

public class ConnectionDemo01 {
    public static void main(String[] args)  {
      try {
          Driver driver=null;
          driver=new  com.mysql.jdbc.Driver();

          String url="jdbc:mysql://localhost:3305/test";

          Properties info=new Properties();
          info.setProperty("user","root");
          info.setProperty("password","abc123");
          Connection connection=driver.connect(url,info);
          System.out.println(connection);
      }catch (SQLException e){
          e.printStackTrace();
      }


    }
}
```

> 说明：上述代码中显式出现了第三方数据库的API

**连接方式二：**

```sql
package com.dreamcold.connect;

import java.sql.Connection;
import java.sql.Driver;
import java.util.Properties;

public class ConnectionDem02 {
    public static void main(String[] args) {
        try {
            String className="com.mysql.jdbc.Driver";
            Class clazz=Class.forName(className);
            Driver driver=(Driver) clazz.newInstance();
            String url="jdbc:mysql://localhost:3305/test";

            Properties info=new Properties();
            info.setProperty("user","root");
            info.setProperty("password","abc123");
            Connection connection=driver.connect(url,info);
            System.out.println(connection);
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}

```

> 说明：相较于方式一，这里使用反射实例化Driver，不在代码中体现第三方数据库的API。体现了面向接口编程
> 思想

**连接方式三**

```java
package com.dreamcold.connect;

import java.sql.Connection;
import java.sql.Driver;
import java.sql.DriverManager;

public class ConnectionDemo03 {
    public static void main(String[] args) {
        try {
            String url="jdbc:mysql://localhost:3305/test";
            String user="root";
            String password="abc123";
            String driverName="com.mysql.jdbc.Driver";
            Class clazz=Class.forName(driverName);
            Driver driver=(Driver)clazz.newInstance();
            //注册驱动
            DriverManager.registerDriver(driver);
            Connection connection = DriverManager.getConnection(url, user, password);
            System.out.println(connection);

        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

> 说明：使用DriverManager实现数据库的连接。体会获取连接必要的4个基本要素。

**连接方式四：**

```java
package com.dreamcold.connect;

import java.sql.Connection;
import java.sql.DriverManager;

public class ConnectionDemo04 {
    public static void main(String[] args) {
        try {
            String url="jdbc:mysql://localhost:3305/test";
            String user="root";
            String password="abc123";
            String driverName="com.mysql.jdbc.Driver";
            Class.forName(driverName);
            Connection connection = DriverManager.getConnection(url, user, password);
            System.out.println(connection);
        }catch (Exception e){
            e.printStackTrace();
        }
    }
}
```

> 说明：不必显式的注册驱动了。因为在DriverManager的源码中已经存在静态代码块，实现了驱动的注册。

**连接方式五(最终版)**

```java
package com.dreamcold.connect;

import java.io.InputStream;
import java.sql.DriverManager;
import java.util.Properties;

public class ConnectionDemo05 {
    public static void main(String[] args) {
        try {

            InputStream in = ConnectionDemo05.class.getResourceAsStream("jdbc.properties");
            Properties properties = new Properties();
            properties.load(in);
            String url="jdbc:mysql://localhost:3305/test";
            String user="root";
            String password="abc123";
            String driverName="com.mysql.jdbc.Driver";
            Class.forName(driverName);
            DriverManager.getConnection(url,user,password);
        }catch (Exception e){
            e.printStackTrace();
        }

    }
}
```

> 说明：使用配置文件的方式保存配置信息，在代码中加载配置文件
> 使用配置文件的好处：
> ①实现了代码和数据的分离，如果需要修改配置信息，直接在配置文件中修改，不需要深入代码 ②如果修改了
> 配置信息，省去重新编译的过程

## 使用PreparedStatement实现CRUD操作

### 操作和访问数据库

- 数据库连接被用于向数据库服务器发送命令和 SQL 语句，并接受数据库服务器返回的结果。其实一个数据库连
  接就是一个Socket连接。
- 在 java.sql 包中有 3 个接口分别定义了对数据库的调用的不同方式：
  - Statement：用于执行静态 SQL 语句并返回它所生成结果的对象。
  - PrepatedStatement：SQL 语句被预编译并存储在此对象中，可以使用此对象多次高效地执行该语句。
  - CallableStatement：用于执行 SQL 存储过程

<img src="images/image-20210301140618934.png" alt="image-20210301140618934" style="zoom:50%;" />

### 使用Statement操作数据表的弊端

- 通过调用 Connection 对象的 createStatement() 方法创建该对象。该对象用于执行静态的 SQL 语句，并且返
  回执行结果。
- Statement 接口中定义了下列方法用于执行 SQL 语句：

- 但是使用Statement操作数据表存在弊端：
  - 问题一：存在拼串操作，繁琐
  - 问题二：存在SQL注入问题

- SQL 注入是利用某些系统没有对用户输入的数据进行充分的检查，而在用户输入数据中注入非法的 SQL 语句段
  或命令(如：SELECT user, password FROM user_table WHERE user='a' OR 1 = ' AND password = ' OR '1' =
  '1') ，从而利用系统的 SQL 引擎完成恶意行为的做法。
- 对于 Java 而言，要防范 SQL 注入，只要用 PreparedStatement(从Statement扩展而来) 取代 Statement 就可
  以了。
- 代码演示：

```java
package com.dreamcold.excute;

import java.io.InputStream;
import java.lang.reflect.Field;
import java.sql.*;
import java.util.Properties;
import java.util.Scanner;

public class Demo01 {


    public void testLogin(){
        Scanner scan=new Scanner(System.in);
        System.out.println("请输入用户名：");
        String userName=scan.nextLine();
        System.out.println("密码：");
        String pasWord=scan.nextLine();
        String sql="SELECT USER,PASSWORD FROM USER_TABLE WHERE USER='"+userName+"' AND PASSWORD='"+pasWord+"'";
        User user=get(sql,User.class);
        if(user!=null){
            System.out.println("登录成功");
        }else{
            System.out.println("用户名或者密码错误");
        }
    }

    public <T> T get(String sql,Class<T> clazz)  {
        T t=null;
        Connection conn=null;
        Statement st=null;
        ResultSet rs=null;
        try{
            //加载配置文件
            InputStream is=Demo01.class.getClassLoader().getResourceAsStream("jdbc.properties");
            Properties pros=new Properties();
            pros.load(is);
            //读取配置文件
            String user=pros.getProperty("user");
            String password=pros.getProperty("password");
            String url=pros.getProperty("url");
            String dirverClass=pros.getProperty("driverClass");
            //加载驱动
            Class.forName(dirverClass);
            //获取连接
            conn= DriverManager.getConnection(url,user,password);
            st=conn.createStatement();
            rs=st.executeQuery(sql);
            //获取结果集合中的原数据
            ResultSetMetaData rsdm=rs.getMetaData();
            int columnCouunt=rsdm.getColumnCount();
            if (rs.next()){
                t=clazz.newInstance();
                for (int i=0;i<columnCouunt;i++){
                    //获取列名
                    String columnName=rsdm.getColumnLabel(i+1);
                    //根据列名获取数据表中的数据
                    Object columnVal=rs.getObject(columnCouunt);
                    //将数据表中得到的数据封装到对象
                    Field field=clazz.getDeclaredField(columnName);
                    field.setAccessible(true);
                    field.set(t,columnVal);
                }
                return t;
            }
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            //关闭资源
            if (rs!=null){
                try {
                    rs.close();
                }catch (SQLException e){
                    e.printStackTrace();
                }
            }
            if (st!=null){
                try {
                    st.close();
                } catch (SQLException e) {
                    e.printStackTrace();
                }
            }

            if (conn!=null){
                try {
                    conn.close();
                }catch (SQLException e){
                    e.printStackTrace();
                }
            }

        }
        return null;
    }
}

```

综上：

<img src="images/image-20210301143250857.png" alt="image-20210301143250857" style="zoom:80%;" />

### PreparedStatement的使用

PreparedStatement介绍

- 可以通过调用 Connection 对象的 preparedStatement(String sql) 方法获取 PreparedStatement 对象
- PreparedStatement 接口是 Statement 的子接口，它表示一条预编译过的 SQL 语句
- PreparedStatement 对象所代表的 SQL 语句中的参数用问号(?)来表示，调用 PreparedStatement 对象的
- setXxx() 方法来设置这些参数. setXxx() 方法有两个参数，第一个参数是要设置的 SQL 语句中的参数的索引(从 1开始)，第二个是设置的 SQL 语句中的参数的值

### PreparedStatement vs Statement

- 代码的可读性和可维护性。
- PreparedStatement 能最大可能提高性能：
- DBServer会对预编译语句提供性能优化。因为预编译语句有可能被重复调用，所以语句在被DBServer的编译器编译后的执行代码被缓存下来，那么下次调用时只要是相同的预编译语句就不需要编译，只要将参数直接传入编译过的语句执行代码中就会得到执行。
- 在statement语句中,即使是相同操作但因为数据内容不一样,所以整个语句本身不能匹配,没有缓存语句的意义.事实是没有数据库会对普通语句编译后的执行代码缓存。这样每执行一次都要对传入的语句编译一次。(语法检查，语义检查，翻译成二进制命令，缓存)PreparedStatement 可以防止 SQL 注入

### Java与SQL对应数据类型转换表

<img src="images/image-20210301143702266.png" alt="image-20210301143702266" style="zoom:80%;" />

### 使用PreparedStatement实现增、删、改操作

```java
package com.dreamcold.excute;

import com.dreamcold.util.JDBCUtil;

import java.sql.Connection;
import java.sql.PreparedStatement;

public class PreparedStatementUtil {
    //通过PreparedStatement增、删、改操作
    public void update(String sql,Object ...args){
        Connection conn=null;
        PreparedStatement ps=null;
        try {
            //获取数据库连接
            conn= JDBCUtil.getConnection();
            //获取PrepareStatement
            ps=conn.prepareStatement(sql);
            //填充占位符
            for (int i=0;i<args.length;i++){
                ps.setObject(i+1,args[i]);
            }
            //执行sql语句
            ps.execute();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            JDBCUtil.close(conn,ps);
        }
    }
}
```

### 使用PreparedStatement实现查询操作

```java
package com.dreamcold.excute;

import com.dreamcold.util.JDBCUtil;

import java.lang.reflect.Field;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.ResultSetMetaData;

public class PreparedStatementUtil {
    //通过PreparedStatement增、删、改操作
    public void update(String sql,Object ...args){
        Connection conn=null;
        PreparedStatement ps=null;
        try {
            //获取数据库连接
            conn= JDBCUtil.getConnection();
            //获取PrepareStatement
            ps=conn.prepareStatement(sql);
            //填充占位符
            for (int i=0;i<args.length;i++){
                ps.setObject(i+1,args[i]);
            }
            //执行sql语句
            ps.execute();
        }catch (Exception e){
            e.printStackTrace();
        }finally {
            JDBCUtil.close(conn,ps);
        }
    }

    //通用的针对于不同表的查询，返回一个对象（version1.0）
    public <T> T getInstance(Class<T> clazz,String sql,Object... args){
        Connection conn=null;
        PreparedStatement ps=null;
        ResultSet rs=null;
        try {
            //获取数据库的连接
            conn= JDBCUtil.getConnection();
            //预编译sql语句，得到PrepareStatment对象
            ps=conn.prepareStatement(sql);
            //填充占位符
            for (int i=0;i<args.length;i++){
                ps.setObject(i+1,args[i]);
            }
            rs=ps.executeQuery();
            //得到结果的元数据集
            ResultSetMetaData rsmd=rs.getMetaData();
            int columnCount=rsmd.getColumnCount();
            if (rs.next()){
                T t=clazz.newInstance();
                for (int i = 0; i < columnCount; i++) {
                    //获取列值
                    Object columnVal=rs.getObject(i+1);
                    //获取列的别名，
                    String columnLabel=rsmd.getColumnLabel(i+1);
                    //使用反射
                    Field field=clazz.getDeclaredField(columnLabel);
                    field.setAccessible(true);
                    field.set(t,columnVal);

                }
                return t;
            }

        }catch (Exception e){
            e.printStackTrace();
        }finally {
            JDBCUtil.close(conn,ps);
        }
        return null;
    }
}
```

> 说明：使用PreparedStatement实现的查询操作可以替换Statement实现的查询操作，解决Statement拼串和
> SQL注入问题。

### ResultSet与ResultSetMetaData

#### ResultSet

- 查询需要调用PreparedStatement 的 executeQuery() 方法，查询结果是一个ResultSet 对象
- ResultSet 对象以逻辑表格的形式封装了执行数据库操作的结果集，ResultSet 接口由数据库厂商提供实现
- ResultSet 返回的实际上就是一张数据表。有一个指针指向数据表的第一条记录的前面。
- ResultSet 对象维护了一个指向当前数据行的游标，初始的时候，游标在第一行之前，可以通过 ResultSet 对象的 next() 方法移动到下一行。调用 next()方法检测下一行是否有效。若有效，该方法返回 true，且指针下移。相当于Iterator对象的 hasNext() 和 next() 方法的结合体

- 当指针指向一行时, 可以通过调用 getXxx(int index) 或 getXxx(int columnName) 获取每一列的值。
  - 例如: getInt(1), getString("name")
  - 注意：Java与数据库交互涉及到的相关Java API中的索引都从1开始。
  - ResultSet 接口的常用方法：
    - boolean next()
    - getString()
    - …

<img src="images/image-20210301151139113.png" alt="image-20210301151139113" style="zoom:80%;" />

#### ResultSetMetaData

- 可用于获取关于 ResultSet 对象中列的类型和属性信息的对象
- ResultSetMetaData meta = rs.getMetaData();
  - getColumnName(int column)：获取指定列的名称
  - getColumnLabel(int column)：获取指定列的别名
  - getColumnCount()：返回当前 ResultSet 对象中的列数。
  - getColumnTypeName(int column)：检索指定列的数据库特定的类型名称。
  - getColumnDisplaySize(int column)：指示指定列的最大标准宽度，以字符为单位。
  - isNullable(int column)：指示指定列中的值是否可以为 null。
  - isAutoIncrement(int column)：指示是否自动为指定列进行编号，这样这些列仍然是只读的。

<img src="images/image-20210301151258141.png" alt="image-20210301151258141" style="zoom:80%;" />

**问题1：得到结果集后, 如何知道该结果集中有哪些列 ？ 列名是什么？**
需要使用一个描述 ResultSet 的对象， 即 ResultSetMetaData
**问题2：关于ResultSetMetaData**

1. 如何获取 ResultSetMetaData： 调用 ResultSet 的 getMetaData() 方法即可
2. 获取 ResultSet 中有多少列：调用 ResultSetMetaData 的 getColumnCount() 方法
3. 获取 ResultSet 每一列的列的别名是什么：调用 ResultSetMetaData 的getColumnLabel() 方法

![image-20210301151415200](images/image-20210301151415200.png)

#### 资源的释放

释放ResultSet, Statement,Connection。

- 数据库连接（Connection）是非常稀有的资源，用完后必须马上释放，如果Connection不能及时正确的关闭将
  导致系统宕机。Connection的使用原则是尽量晚创建，尽量早的释放。
- 可以在finally中关闭，保证及时其他代码出现异常，资源也一定能被关闭。

#### JDBC API小结

**两种思想**
面向接口编程的思想

- ORM思想(object relational mapping)
  - 一个数据表对应一个java类
  - 表中的一条记录对应java类的一个对象
  - 表中的一个字段对应java类的一个属性
  - sql是需要结合列名和表的属性名来写。注意起别名。
    两种技术
- JDBC结果集的元数据：ResultSetMetaData
    - 获取列数：getColumnCount()
    - 获取列的别名：getColumnLabel()
    - 通过反射，创建指定类的对象，获取指定的属性并赋值