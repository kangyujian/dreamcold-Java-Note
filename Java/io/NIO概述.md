# NIO概述

## 什么是NIO

Java NIO (New IO，Non-Blocking 10)是从Java 1.4版本开始引入的一-套新的IO API,可以替代标准的Java I0 API。Nlo与原来的I0有同样的作用和目的，但是使用的方式完全不同，NIO 支持而向缓冲区的(IO是而向流的)、基于通道的IO操作。NIO将以更加高效的方式进行文件的读写操作。

- Java API中提供了两套NIO，一套是针对标准输入输出NIO，另一套就是网收地织NIO

![image-20210119171122604](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210119171122604.png)

## NIO2

随着JDK 7的发布，Java对NIO进行了极大的扩展，增强了对文件处理和文件系统特性的支持，以至于我们称他们为NIO.2。因为NIO提供的一些功能，NIO已经成为文件处理中越来越重要的部分。



## Path、Paths和Files核心API

- 早期的Java只提供了一个File类来访问文件系统，但File 类的功能比较有限，提供的方法性能也不高。而且，大多数方法在出错时仅返回失败，并不会提供异常信息。
- NIO.2为了弥补这种不足，引入了Path接口， 代表-一个平台无关的平台路径，描述了目录结构中文件的位置。Path可以看成是File类的升级版本，实际引用的资源也可以不存在。

![image-20210119171340059](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210119171340059.png)