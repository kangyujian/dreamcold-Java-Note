# File类的常用方法

## File类的理解

1. File类的一个对象，代表一个文件或一个文件目录(俗称:文件夹)
2. File类声明在java.io包下
3. File类中涉及到关于文件或文件目录的创建、删除、重命名、修改时间、文件大小等方法，
并未涉及到写入或读取文件内容的操作。如果需要读取或写入文件内容，必须使用IO流来完成。
4. 后续File类的对象常会作为参数传递到流的构造器中，指明读取或写入的"终点".



## File的实例化

### 常用构造器

```java
File(String filePath)
File(String parentPath, String childPath)
File(File parentFile, String childPath )
```



### 路径的分类

- 相对路径:相较于某个路经下,指明的路径..
- 绝对路径:包含盘符在内的文件或文件目录的路径

说明:
 IDEA中:

- 如果大家开发使用JUnit中的单元测试方法测试，相对路径即为当前Module下。
-  如果大家使用main测试，相对路径即为当前的Project下。

### 路径分隔符

- 路径中的每级目录之间用一个路径分隔符隔开。
- 路径分隔符和系统有关:
  - windows和DOS系统默认使用“\”来表示
  - UNIX和URL使用“1”来表示
- Java程序支持跨平台运行，因此路径分隔符要慎用。
- 为了解决这个隐患，File类提供了一 一个常量:

```java
public static final String separator//根据操作系统，动态的提供分隔符。
```



### File类的常用方法

- File类的获取功能：
  - public String getAbsolutePath():获取绝对路径
  - public String getPath() :获取路径
  - public String getName():获取名称
  - public String getParent():获取上层文件目录路径。若无，返回null
  - public long length):获取文件长度(即:字节数)。不能获取目录的长度。
  - public long lastModified() :获取最后一次的修改时间，亳秒值
  - public StringD list():获取指定目录下的所有文件或者文件目录的名称数组
  - public File[] listFiles():获取指定目录下的所有文件或者文件目录的File数组
 - File类的重命名功能
     - public boolean rename To(File dest):把文件重命名为指定的文件路径
- File类的判断功能
  - public boolean isDirectory():判断是否是文件目录
  - public boolean isFile():判断是否是文件
  - public boolean exists() :判断是否存在
  - public boolean canRead():判断是否可读
  - public boolean canWrite():判断是否可写
  - public boolean isHidden() :判断是否隐藏

- File类的创建功能
  - public boolean createNewFile():创建文件。若文件存在，则不创建，返回false
  - public boolean mkdir() :创建文件日录。如果此文件目录存在，就不创建了。
    如果此文件目录的上层目录不存在，也不创建。
  - public boolean mkdirs():创建文件目录。如果上层文件目录不存在，一并创建
  - 注意事项:如果你创建文件或者文件目录没有写盘符路径，那么，默认在项目
    路径下。

- File 类的删除功能
  - public boolean deletel():删除文件或者文件夹
  - 删除注意事项:
    - Java中的删除不走回收站。
    - 要删除-一个文件目录，请注意该文件目录内不能包含文件或者文件目录



