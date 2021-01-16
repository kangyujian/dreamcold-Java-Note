# FileReader读取文件

## 查看文件的相对路径

<img src="images/image-20210116193512570.png" alt="image-20210116193512570" style="zoom:67%;" />

示例一：查看文件的相对路径

```java
package com.dreamcold.io;

import java.io.File;

public class Demo01 {
    public static void main(String[] args) {
        File file=new File("test.txt");
        System.out.println(file.getAbsolutePath());
    }
}

```

输出:

<img src="images/image-20210116193551825.png" alt="image-20210116193551825" style="zoom:67%;" />



## 读取txt文件

### 写法一

1. 实例化File类的对象， 指明要操作的文件

2. 提供具体的源

3. 数据的读入，read():返回读入的一个字符，如果达到文件末尾返回-1

4. 关闭流

```java
package com.dreamcold.io;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

public class Demo01 {
    public static void main(String[] args) throws IOException {
        // 1.实例化File类的对象， 指明要操作的文件
        File file=new File("test.txt");
       //2.提供具体的源

        FileReader fr=new FileReader(file);
        //3.数据的读入，
        //read():返回读入的一个字符，如果达到文件末尾返回-1

        int data=fr.read();
        while (data!=-1){
            System.out.print((char)data);
            data=fr.read();
        }

        //关闭流

        fr.close();

    }
}
```



结果

![image-20210116194352096](images/image-20210116194352096.png)

### 写法二

```java
package com.dreamcold.io;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

public class Demo01 {
    public static void main(String[] args) throws IOException {
        // 1.实例化File类的对象， 指明要操作的文件
        File file=new File("test.txt");
       //2.提供具体的源

        FileReader fr=new FileReader(file);
        //3.数据的读入，
        //read():返回读入的一个字符，如果达到文件末尾返回-1

        int data;
        while((data=fr.read())!=-1){
            System.out.print((char)data);
        }
        //关闭流

        fr.close();

    }
}
```

输出:

![image-20210116194649605](images/image-20210116194649605.png)

### 写法三-加异常处理

```java
package com.dreamcold.io;

import java.io.File;
import java.io.FileNotFoundException;
import java.io.FileReader;
import java.io.IOException;

public class Demo01 {
    public static void main(String[] args)  {
        FileReader fr=null;
        try {
            // 1.实例化File类的对象， 指明要操作的文件
            File file=new File("test.txt");
            //2.提供具体的源

            fr=new FileReader(file);
            //3.数据的读入，
            //read():返回读入的一个字符，如果达到文件末尾返回-1

            int data;
            while((data=fr.read())!=-1){
                System.out.print((char)data);
            }
            //关闭流
        }catch (IOException e){
            e.printStackTrace();
        }finally {
            try {
                if (fr!=null){
                    fr.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}

```

**说明点:**

1. read()的理解:返回读入的一一个字符。如果达到文件末尾，返回-1
2. 异常的处理:为了保证流资源-定可以执行关闭操作。需要使用try-catch-finally处理
3. 读入的文件一定要存在，否则就会报FileNotFoundException. |



## FileReader的重载方法read

```java
package com.dreamcold.io;

import java.io.File;
import java.io.FileReader;
import java.io.IOException;

public class Demo02 {
    public static void main(String[] args) {
        FileReader fr=null;
        try {
            File file=new File("test.txt");
            fr=new FileReader(file);
            char[] cbuf=new char[5];
            int len;
            while((len=fr.read(cbuf))!=-1){
                for (int i=0;i<len;i++){
                    System.out.print(cbuf[i]);
                }
            }
        }catch (IOException e){
            e.printStackTrace();
        }finally {
            try {
                if (fr!=null){
                    fr.close();
                }
            }catch (IOException e){
                e.printStackTrace();
            }
        }
    }
}

```

结果:

![image-20210116195958903](images/image-20210116195958903.png)

**注意:**

1. 每次读取5个字符数据大小
2. 读取到末尾的时候，可能不够5个字符了，那么我们要确定这次读取有几个字符，用字符数len循环

