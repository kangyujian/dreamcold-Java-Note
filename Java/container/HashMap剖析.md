# HashMap剖析

## Map接口

![image-20201223223105667](images/image-20201223223105667.png)

- Map是双列数据，存储key-value对的数据，类似于高中的y=f(x)
- HashMap是Map的最主要的实现类，线程不安全，效率高，可以存储null的key和value
  - 底层最初就是数组加链表实现（JDK7之前）
  - 数组+链表+红黑树（JDK8之后）
- Hashtable作为最古老的实现类，起源于JDK1.0，线程安全效率低
- LinkedHashMap：保证在遍历Map元素的时候，可以按照添加的顺序实现遍历，原理是在原有的HashMap底层原理基础上，添加了一对指针指向了前一元素和后一个元素，对于频繁的遍历操作，LinkedHashMap效率要高于HashMap
- TreeMap:保证按照添加的key-value对进行排序，实现排序遍历，此时考虑key的自然排序或者定制排序，底层使用的是红黑树
- properties：常常用来处理配置文件，key和value都是String类型

**注意：**

1. 典型面试题HashMap的底层实现源代码？
2. HashMap和Hashtable的异同？
3. ConcurrentHashMap与Hashtable的区别？

## Map结构的理解

<img src="images/image-20201224200624886.png" alt="image-20201224200624886" style="zoom:80%;" />

- Map中的key是无序的、不可重复的，使用Set存储所有的key，**key所在的类要重写hashCode()和equals()方法**（HashMap为例）
- Map中的value是无序的、可重复的，使用Collection存储所有的value，**value所在的类要重写equals()**
- 一个键值对：key-value构成一个Entry对象
- Map中的Entry：无序的不可重复的，使用Set存储所有entry

## HashMap的底层实现原理

### 以JDK7说明

```java
Map<Integer,Integer> map=new HashMap<>();
```

1. 实例化之后，底层创建了长度为16的一维数组，数组的类型是Entry ,数组的名称叫做table

```java
map.put(key1,value1);
```

2. 首先调用key1所在类的hashCode()方法，计算key1的哈希值，此哈希值经过某种算法计算以后，得到在Entry数组中的存放位置
   - 情况一，如果此位置上的数据为空此时key1-value1添加成功
   - 如果此位置上的数据不为空（意味着此位置上存放着一个或者多个数据(以链表的形式存在)），比较key1与已经存在的一个或者多个数据的哈希值
     - 如果key1的哈希值与已经存在的数据的哈希值都不相同，key1和value1的键值对添加成功
     - key1哈希值与已经存在的某一个某一个数据(key2-value2)的哈希值相同，继续比较，调用key1所在类的equals(key2)方法，比较
       - 如果equals返回为false:此时key1-value1添加成功(情况2)
       - 如果equals返回true:使用value1替换value2,起到了一个修改的功能(情况3)
   - 补充：关于情况2和情况3：此时key1和value1和原来的数据以链表的形式来存储
3. 在不断的添加过程中会不断的涉及到扩容的问题，默认的扩容方式扩容为原来的2倍，并将原有的数据复制过来

### 以JDK8说明

1. new HashMap():底层没有创建一个长度为16的数组
2. JDK8底层的数组是Node类型的数组，而非Entry数据
3. 首次调用put方法的时候，底层创建长度为16的数组
4. JDK7底层结构只有数组+链表，JDK8中底层结构为：数组+链表+红黑树
5. 当数组某一个索引位置上的元素以链表形式存在的数据个数>8,且当前数据的长度>64,此时此索引位置上的所有数据采用红黑树存储，主要为了提升查找效率



