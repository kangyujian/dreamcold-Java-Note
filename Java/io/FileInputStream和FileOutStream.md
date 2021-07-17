# FileInputStream和FileOutStream

## 测试FileInputstream和FileOutpuStream的使用

结论:

1. 对于文本文件(. txt,.java,.c..cpp)，使用字符流处理

2. 对于非文本文件( . jpg ，.mp3, . mp4, .avi,.doc.ppt....)， 使用字节流处理

3. 使用字节流FileInputstream处理文本文件， 可能出现乱码

```java
package com.dreamcold.io;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class Demo05 {
    public static void main(String[] args) {
        FileInputStream fis=null;
        try {
            //造文件
            File file=new File("test.txt");

            //造流
            fis=new FileInputStream(file);

            //读取数据
            byte[] buff=new byte[5];
            int len;
            while((len=fis.read(buff))!=-1){
                String str=new String(buff,0,len);
                System.out.println(str);
            }
        }catch (IOException e){
            e.printStackTrace();
        }finally {
            try {
                if (fis!=null){
                    fis.close();
                }
            }catch (IOException e){
                e.printStackTrace();
            }
        }

    }
}
```

结果

![image-20210117200527012](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210117200527012.png)

## 对非文本文件的复制

```java
package com.dreamcold.io;

import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class Demo06 {
    public static void main(String[] args) {
        FileInputStream fis=null;
        FileOutputStream fos=null;
        try {
            File srcFile=new File("image.jpg");
            File destFile=new File("image1.jpg");
            fis=new FileInputStream(srcFile);
            fos=new FileOutputStream(destFile);

            byte[] buff=new byte[5];
            int len;
            while ((len=fis.read(buff))!=-1){
                fos.write(buff,0,len);
            }
        }catch (IOException e){
            e.printStackTrace();
        }finally {
            try {
                if (fis!=null){
                    fis.close();
                }
            }catch (IOException e){
                e.printStackTrace();
            }
            
            try {
                if (fos!=null){
                    fos.close();
                }
            }catch (IOException e){
                e.printStackTrace();
            }
        }

    }
}
```

效果

<img src="https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210117201522740.png" alt="image-20210117201522740" style="zoom:67%;" />

- 该函数可以封装成为一个复制文件的函数
- 注意缓冲的buf大小最好为1024