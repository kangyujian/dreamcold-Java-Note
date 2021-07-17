# IO流原理以及流的分类

## 流的分类

- 按操作数据单位不同分为:字节流(8 bit)， 字符流(16 bit)
- 按数据流的流向不同分为:输入流、输出流
- 按流的角色的不同分为:节点流，处理流

![image-20210116191927305](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210116191927305.png)

- Java的IO流共涉及40多个类，实际上非常规则，都是从如下4个抽象基类派生的。
- 由这四个类派生出来的子类名称都是以其父类名作为子类名后缀。

## IO流体系

![image-20210116192313196](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210116192313196.png)



| 抽象基类     | 节点流(或者文件流) | 缓冲流               |
| ------------ | ------------------ | -------------------- |
| InputStream  | FileInputStream    | BufferedInputStream  |
| OutputStream | FileOutputStream   | BufferedOutputStream |
| Reader       | FileReader         | BufferedReader       |
| Writer       | FileWriter         | Bufferedwriter       |

