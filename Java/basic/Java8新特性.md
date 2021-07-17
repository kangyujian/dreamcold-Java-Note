# Java8新特性

## Java8介绍

### 关于Java8

1. Java 8(又称为jdk 1.8)是Java语言开发的一个主要版本。
2. Java 8是oracle公司于2014年3月发布，可以看成是自Java5以来最具革命性的版本。Java 8为Java语言、编译器、类库、开发工具与JVM带来了大量新特性。

### Java8的特性

- 速度更快
- 代码更少(增加了新的语法: Lambda表达式)
- 强大的Stream API
- 便于并行
- 最大化减少空指针异常: Optional
- Nashorn引擎，允许在JVM上运行JS应用

## Lambda表达式

### 为什么使用Lambda表达式

Lambda是一个匿名函数，我们可以把Lambda表达式理解为是一-段可以传递的代码(将代码像数据一样进行传递)。使用它可以写出更简洁、更灵活的代码。作为-种更紧凑的代码风格，使Java的语自表达能力得到了提升。

### Lambda表达式举例

1. **示例一：匿名类中的函数表示**

```java
package com.dreamcold.java8;

/**
 * Lambda表达式的使用举例
 */
public class LambdaTest01 {
    public static void main(String[] args){
        Runnable r1=new Runnable() {
            @Override
            public void run() {
                System.out.println("我爱北京");
            }
        };
        r1.run();
        System.out.println("=========使用lambda表达式=========");
        Runnable r2=()->{
            System.out.println("我爱天津");
        };
        r2.run();
    }
}
```

效果：

![image-20210213132218795](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210213132218795.png)

2. **示例二：比较器中传入比较函数**

```java
package com.dreamcold.java8;

import java.util.Comparator;
/**
 * Lambda表达式的使用举例
 */
public class LambdaTest02 {
    public static void main(String[] args) {
        Comparator<Integer> com1=new Comparator<Integer>(){
            @Override
            public int compare(Integer o1, Integer o2) {
                return Integer.compare(o1,o2);
            }
        };
        System.out.println(com1.compare(12, 21));
        System.out.println("========使用lambda表达式========");
        Comparator<Integer> com2=(o1,o2)->Integer.compare(o1,o2);
        System.out.println(com2.compare(32, 21));
        System.out.println("=========使用::方法引用==================");
        Comparator<Integer> com3=Integer::compare;
        System.out.println(com3.compare(21,12));
    }
}
```

效果：

![image-20210213133243966](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210213133243966.png)

### Lambda表达式的使用

1. 举例: (o1,o2) -> Integer.compare(o1,02);

2. 格式:

```
->lambda操作符戚箭头操作符
->左边: lambda形参列表(其实就是 接口中的抽象方法的形参列表),如果参数列表只有一个参数可以省略()
->右边: lambda体 (其实就是 重写的抽象方法的方法体),如果Lambda体中仅仅只有一条执行语句，可能是return语句，可以省略一对{},以及return关键字
```

3. Lambda 表达式的使用: (分为6 种情况介绍)

- 语法格式一。语法格式无参

```java
Runnable r1 =() -> {Systen.out.println("Hello Lambda!");};
```

- 语法格式二。Lambda需要一个参数， 但是没有返回值.

```java
Consumner<String> con = (String str) -> (System.out.println(str););
```

- 语缺格式三。数据类型可以省略，因为可由编评器推断得出。称为“类型推断”

```java
Consumer<String> con = (str) -> {System.out.println(str);};
```

- 语法格式四。Lambda若只需要一个参 数时，参数的小括号可以省略

```java
Consumer<String> con = str -> {System.out.println(str);};
```

- 语法格式五，Lambda雷要两个或以上的参数，多条执行语句，并且可以有返回值

```java
Comparator<Integer> com = (xy) > {
    System.out.println("实现函数式接口方法! ");
    return Integer.compare(x,y);
};
```

- 语认格式六。当Lambda体只有一条语句时. return与大括号若有``都可以省略

```java
Comparator< Integer> com = (x,y) > Integer.compare(x,y);
```

4. .Lambda表达式的本质:作为接口的实例

### 六种Lambda表达式实例

**示例一：语法格式一。语法格式无参**

```java
package com.dreamcold.java8;

/**
 * Lambda表达式的使用举例
 */
public class LambdaTest01 {
    public static void main(String[] args){
        Runnable r1=new Runnable() {
            @Override
            public void run() {
                System.out.println("我爱北京");
            }
        };
        r1.run();
        System.out.println("=========使用lambda表达式=========");
        Runnable r2=()->{
            System.out.println("我爱天津");
        };
        r2.run();
    }
}
```

效果：

![image-20210213171719729](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210213171719729.png)

示例二：语法格式二: Lambda 需要一个参数，但是没有返回值。

```java
package com.dreamcold.java8;

import java.util.function.Consumer;

public class LambdaTest03 {
    public static void main(String[] args) {
        Consumer<String> consumer=new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };
        consumer.accept("Hello");
        System.out.println("===========使用lambda替换===========");
        Consumer<String> con1=(String s)->{
            System.out.println(s);
        };
        consumer.accept("World");
    }
}
```

效果：

![image-20210213172324685](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210213172324685.png)

示例四：数据类型可以省略，因为可由编译器推断得出，称为“类型推断”

```java
package com.dreamcold.java8;

import java.util.function.Consumer;

public class LambdaTest04 {
    public static void main(String[] args) {
        Consumer<String> consumer=new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };
        consumer.accept("Hello");
        System.out.println("===========使用lambda替换===========");
        Consumer<String> consumer1=(s)->{
            System.out.println(s);
        };
        consumer1.accept("World");
    }
}
```

效果：

![image-20210213172736161](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210213172736161.png)

示例五：类型推断示例

```java
package com.dreamcold.java8;

import java.util.ArrayList;

public class Demo01 {
    public static void main(String[] args) {
        ArrayList<String> list=new ArrayList<>();//ArrayList<String> list=new ArrayList<String>();
        int[] arr={1,2,3};// int[] arr=new int[]{1,2,3};
    }
}
```

效果：

![image-20210213173423368](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210213173423368.png)

示例六：语法格式四。Lambda若只需要一个参 数时，参数的小括号可以省略

```java
package com.dreamcold.java8;

import java.util.function.Consumer;

public class LambdaTest05 {
    public static void main(String[] args) {
        Consumer<String> consumer=new Consumer<String>() {
            @Override
            public void accept(String s) {
                System.out.println(s);
            }
        };
        consumer.accept("Hello");
        System.out.println("===========使用lambda替换===========");
        Consumer<String> consumer1=s->{
            System.out.println(s);
        };
        consumer1.accept("World");
    }
}
```

效果：

![image-20210214130225140](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210214130225140.png)

示例七：语法格式五: Lambda 需要两个或以上的参数，多条执行语句，并且可以有返回值

```java
package com.dreamcold.java8;

import java.util.Comparator;

public class LambdaTest06 {
    public static void main(String[] args){
        Comparator<Integer> com1=new Comparator<Integer>() {
            @Override
            public int compare(Integer o1, Integer o2) {
                System.out.println(o1);
                System.out.println(o2);
                return o1.compareTo(o2);
            }
        };
        System.out.println(com1.compare(12,21));
        System.out.println("========================");
        Comparator<Integer> com2=(o1,o2)->{
            System.out.println(o1);
            System.out.println(o2);
            return o1.compareTo(o2);
        };
        System.out.println(com2.compare(12,6));
    }
}
```

效果：

![image-20210214130741434](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210214130741434.png)

示例八：方法体只有一条语句时，return 与大括号若有，都可以省略。

```java
package com.dreamcold.java8;

import java.util.Comparator;

public class LamdaTest07 {
    public static void main(String[] args) {
        Comparator<Integer> com1=(o1,o2)->o1.compareTo(o2);
        System.out.println(com1.compare(1,12));
    }
}
```

## 函数式(Functional)接口

### 什么是函数式接口？

- 只包含一个抽象方法的接口，称为函数式接口。
- 你可以通过Lambda表达式来创建该接口的对象。(若 Lambda表达式抛出一个受检异常(即:非运行时异常)，那么该异常需要在目标接口的抽象方法上进行声明)。
- 我们可以在一个接口上使用@FunctionalInterface注解，这样做可以检查它是否是一个函数式接口。同时javadoc也会包含一条声明，说明这个
- 接口是一个函数式接口。在java.util.function包下定义了Java 8的丰富的函数式接口

### Java内置的四大函数式接口

![image-20210214131758476](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210214131758476.png)



## 方法引用与构造器引用

### 什么是方法引用

所谓方法引用，是指如果某个方法签名和接口恰好一致，就可以直接传入方法引用。

因为`Comparator<String>`接口定义的方法是`int compare(String, String)`，和静态方法`int cmp(String, String)`相比，除了方法名外，方法参数一致，返回类型相同，因此，我们说两者的方法签名一致，可以直接把方法名作为Lambda表达式传入：

```java
package com.dreamcold.java8;

import java.util.Arrays;
import java.util.Comparator;

public class Demo06 {
    public static void main(String[] args) {
        String[] things={"hello","world","kangyujian"};
        //原始的情况
        Arrays.sort(things, new Comparator<String>() {
            @Override
            public int compare(String o1, String o2) {
                return o1.compareTo(o2);
            }
        });
        //lambda表达式的写法
        Arrays.sort(things,(a,b)->{
            return a.compareTo(b);
        });
        //引用类方法
        Arrays.sort(things,Demo06::cmp);
        //引用实例方法,String类中compareTo是实例方法，但是之所以可以传入是因为该实例方法包含了默认参数this
        Arrays.sort(things,String::compareTo);

    }

    public static int cmp(String s1,String s2){
        return s1.compareTo(s2);
    }
}
```

### 构造方法引用

```java
package com.dreamcold.java8;

import java.util.Arrays;
import java.util.List;
import java.util.stream.Collectors;

class Person {
    String name;
    public Person(String name) {
        this.name = name;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                '}';
    }
}

public class Demo07 {
    public static void main(String[] args) {
        String[] names={"xiaoming","xiaohong","xiaoli"};
        List<String> list = Arrays.asList(names);
        List<Person> personList=list.stream().map(Person::new).collect(Collectors.toList());
        for (int i = 0; i < personList.size() ;i++) {
            System.out.println(personList.get(i));
        }
    }
}
```

效果：

![image-20210318191419202](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210318191419202.png)

后面我们会讲到`Stream`的`map()`方法。现在我们看到，这里的`map()`需要传入的FunctionalInterface的定义是：

```java
@FunctionalInterface
public interface Function<T, R> {
    R apply(T t);
}
```

把泛型对应上就是方法签名`Person apply(String)`，即传入参数`String`，返回类型`Person`。而`Person`类的构造方法恰好满足这个条件，因为构造方法的参数是`String`，而构造方法虽然没有`return`语句，但它会隐式地返回`this`实例，类型就是`Person`，因此，此处可以引用构造方法。构造方法的引用写法是`类名::new`，因此，此处传入`Person::new`。

## 强大的Stream API

### 什么是Stream API？

- Java8中有两大最为重要的改变。第一个是Lambda表达式:另外- 一个则是Stream API。
- Stream API (` java. util.stream`)把真正的函数式编程风格引入到Java中。这是目前为止对Java类库最好的补充，因为Stream API可以极大提供Java程序员的生产力，让程序员写出高效率、干净、简洁的代码。Stream是Java8中处理集合的关键抽象概念，它可以指定你希望对集合进行的操作，可以执行非常复杂的查找、过滤和映射数据等操作。使用
  **Stream API对集合数据进行操作，就类似于使用SQL执行的数据库香询。也可以使用Stream API来并行执行操作**。
- 简言之，StreamAPl 提供了一种高效且易于使用的处理数据的方式。

### 为什么要采用Stream API？

- 实际开发中，项目中多数数据源都来自于Mysql, Oracle等。 但现在数据源可以更多了，有MongDB, Radis等， 而这些NoSQL的数据就需要Java层面去处理。

- Stream和Collection集合的区别: **Collection 是一种静态的内存数据结构，而Stream是有关计算的。**前者是主要面向内存，存储在内存中，后者主要是面向CPU,通过CPU实现计算

### Stream到底是什么？

Stream是数据渠道，用于操作数据源(集合、数组等)所生成的元素序列。“集合讲的是数据，Stream讲的是计算!”

注意:

- Stream自己不会存储元素。
- Stream不会改变源对象。相反，他们会返回一个持有结果的新Stream.
- Stream操作是延迟执行的。这意味着他们会等到需要结果的时候才执行。

### Stream操作的三个步骤

- 创建Stream:一个数据源(如:集合、数组)，获取一个流
- 中间操作:一个中间操作链，对数据源的数据进行处理
- 终止操作(终端操作):一旦执行终止操作，就执行中间操作链，并产生结果，之后，不会再被使用

![image-20210521214821835](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210521214821835.png)

### 创建Stream流

#### 前提准备

创建Person类:

```java
public class Student {
    private Integer age;
    private String name;
    private Integer score;
    //getter、setter方法
    //有参构造器，无参构造器
}
```

#### 方式一：通过集合

Java8中的Collection接口被扩展，提供了两个获取流的方法:

- `default Stream<E> stream()`:返回一个顺序流
- `default Stream<E> parallelStream()`:返回一一个并行流

```java
package com.dreamcold.java8.test;

import java.util.ArrayList;
import java.util.List;
import java.util.stream.Collectors;
import java.util.stream.Stream;
public class Demo01 {
    public static void main(String[] args) {
        List<Student> students=new ArrayList<>();
        students.add(new Student(12,"xiaoming",90));
        students.add(new Student(12,"xiaohong",85));
        students.add(new Student(12,"xiaosong",72));
        students.add(new Student(12,"xiaoli",67));
        students.add(new Student(12,"xiaoai",89));
        //default stream<E> stream() :返回一个顺序流
        Stream<Student> stream = students.stream();
        //default Stream<E> parallelStream() :返回一个并行流
        Stream<Student> studentStream = students.parallelStream();
    }
}
```

#### 方式二：通过数组

```java
package com.dreamcold.java8.test;
import java.util.Arrays;
import java.util.stream.IntStream;
import java.util.stream.Stream;

public class Demo02 {
    public static void main(String[] args) {
        int[] arr=new int[]{1,2,3,4,5,6};
        IntStream stream = Arrays.stream(arr);
        //调用Arrays类的static <T> Stream<T> stream(T[] array): 返回一个流
        Student s1=new Student(12,"xiaoming",90);
        Student s2=new Student(12,"xiaohong",85);
        Student[] students=new Student[]{s1,s2};
        Stream<Student> stream1 = Arrays.stream(students);
    }
}
```

#### 方式三：通过Stream的of()

可以调用Stream类静态方法of(),通过显示值创建一一个流。它可以接收任意数量的参数。

`public static<T> Stream<T> of(... values)`:返回一个流

```java
  Stream<Integer> integerStream = Stream.of(1, 2, 3, 4, 5, 6, 7, 8);
```

#### 方式四：创建无限流

可以使用静态方法Stream.iterate() 和Stream.generate(),创建无限流。

- 迭代:

```java
public static<T> Stream<T> iterate(final T seed, final UnaryOperator<T> f)
```

- 生成

```java
public static<T> Stream<T> generate(Supplier<T> s)
```

示例:

```java
 //遍历前10个偶数
Stream.iterate(0,t->t+2).limit(10).forEach(System.out::println);
//生成随机数
Stream.generate(Math::random).limit(10).forEach(System.out::println);
```

### Stream流的中间操作

#### 筛选与切片

多个中间操作可以连接起来形成一个流水线，除非流水线上触发终止操作，否则中间操作不会执行任何的处理!而在终止操作时一次性全部处理，称为“惰性求值”。

![image-20210522102945159](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210522102945159.png)

1. 示例一：过滤成绩大于80的学生

```java
        List<Student> students=new ArrayList<>();
        students.add(new Student(12,"xiaoming",90));
        students.add(new Student(12,"xiaohong",85));
        students.add(new Student(12,"xiaosong",72));
        students.add(new Student(12,"xiaoli",67));
        students.add(new Student(12,"xiaoai",89));
        students.stream().filter(s->s.getScore()>80).forEach(System.out::println);
```

效果:

````
Student{age=12, name='xiaoming', score=90}
Student{age=12, name='xiaohong', score=85}
Student{age=12, name='xiaoai', score=89}
````

2. 示例二：截断流，使其元素不超过给定的数量

```java
students.stream().limit(3).forEach(System.out::println);
```

效果:

```
Student{age=12, name='xiaoming', score=90}
Student{age=12, name='xiaohong', score=85}
Student{age=12, name='xiaosong', score=72}
```

3. 示例三：跳过元素

```java
students.stream().skip(2).forEach(System.out::println);
```

4. 示例四: 筛选去重

```java
        List<Student> students=new ArrayList<>();
        students.add(new Student(12,"xiaoming",90));
        students.add(new Student(12,"xiaoming",90));
        students.add(new Student(12,"xiaoming",90));
        students.add(new Student(12,"xiaoming",90));
        students.add(new Student(12,"xiaoming",90));
        students.stream().distinct().forEach(System.out::println);
```

效果:

```
Student{age=12, name='xiaoming', score=90}
```

注意:要重写Student类的equals和hashCode方法

```java
 @Override
    public boolean equals(Object o) {
        if (this == o) return true;
        if (!(o instanceof Student)) return false;
        Student student = (Student) o;
        return Objects.equals(getAge(), student.getAge()) &&
                Objects.equals(getName(), student.getName()) &&
                Objects.equals(getScore(), student.getScore());
    }

    @Override
    public int hashCode() {
        return Objects.hash(getAge(), getName(), getScore());
    }
```

#### 映射

![image-20210522104559757](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210522104559757.png)

示例一: 列表中的字符串批量转大写

```java
        List<String> list= Arrays.asList("aa","bb","cc","dd");
        list.stream().map(str->str.toUpperCase()).forEach(System.out::println);
```

效果:

```
AA
BB
CC
DD
```

示例二：获取名称长度大于3的学生姓名

```java
 		List<Student> students=new ArrayList<>();
        students.add(new Student(12,"小红",90));
        students.add(new Student(12,"小明",85));
        students.add(new Student(12,"小明明",72));
        students.add(new Student(12,"小红红",67));
        students.add(new Student(12,"大黑狗",89));
        Stream<String> nameStream= students.stream().map(s -> s.getName());
        nameStream.filter(n->n.length()>3).forEach(System.out::println);
```

示例三:将两个列表中的数字乘方连接到一起

[更多参考](https://www.cnblogs.com/diegodu/p/8794857.html)

```java
        List<String> list1= Arrays.asList("aa","bb","cc","dd");
        List<String> list2= Arrays.asList("ee","ff","gg","hh");
        List<List<String>> lists=new ArrayList<>();
        lists.add(list1);
        lists.add(list2);
        lists.stream().flatMap(s->s.stream().map(x->x.toUpperCase())).forEach(System.out::println);
```

效果:

```java
AA
BB
CC
DD
EE
FF
GG
HH
```

#### 排序

![image-20210522111845930](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210522111845930.png)

示例一:对数字排序

```java
        List<Integer> list= Arrays.asList(1,2,3,4,5,6);
        list.stream().sorted().forEach(System.out::println);
```

效果:

```java
1
2
3
4
5
6
```

示例二：根据成绩对学生排序

```java
        List<Student> students=new ArrayList<>();
        students.add(new Student(12,"xiaoming",90));
        students.add(new Student(12,"xiaohong",85));
        students.add(new Student(12,"xiaosong",72));
        students.add(new Student(12,"xiaoli",67));
        students.add(new Student(12,"xiaoai",89));
        //报错:Student cannot be cast to java.lang.Comparable
        //因为Student类没有实现Comparable接口
        // students.stream().sorted().forEach(System.out::println);
        students.stream().sorted((e1,e2)->{
            return e1.getScore().compareTo(e2.getScore());
        }).forEach(System.out::println);
```

效果:

```
Student{age=12, name='xiaoli', score=67}
Student{age=12, name='xiaosong', score=72}
Student{age=12, name='xiaohong', score=85}
Student{age=12, name='xiaoai', score=89}
Student{age=12, name='xiaoming', score=90}
```

### Stream的终止操作

- 终端操作会从流的流水线生成结果。其结果可以是任何不是流的值
- 例如: `List`、`Integer`, 共至是`void`流进行了终止操作后，不能再次使用。

#### 匹配与查找

![image-20210522114627201](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210522114627201.png)

示例一：检查是否所有学生的年龄都大于10岁

```java
  		List<Student> students=new ArrayList<>();
        students.add(new Student(12,"xiaoming",90));
        students.add(new Student(12,"xiaohong",85));
        students.add(new Student(12,"xiaosong",72));
        students.add(new Student(12,"xiaoli",67));
        students.add(new Student(12,"xiaoai",89));
        boolean isAllAgeOldThan10 = students.stream().allMatch(x -> x.getAge() > 10);
        System.out.println(isAllAgeOldThan10);
```

效果:

```
true
```

示例二：检查是否没有学生的姓名以"xiao"开头

```java
boolean isExistNameStartWithXiao = students.stream().noneMatch(x -> x.getName().startsWith("xiao"));//true
```

示例三：返回第一个元素

```java
      Optional<Student> first = students.stream().findFirst();
```

效果:

```
Optional[Student{age=12, name='xiaoming', score=90}]
```

示例四：返回任意的学生

```java
Optional<Student> any = students.stream().findAny();
```

效果:

```
Optional[Student{age=12, name='xiaoming', score=90}]
```

示例五：返回成绩大于70的学生的个数

````java
long count = students.stream().filter(x -> x.getScore() > 70).count();//4
````

示例六： 获取学生中的最高分

```java
Optional<Integer> max = students.stream().map(x -> x.getScore()).max(Integer::compareTo);
```

效果:

```
Optional[90]
```

#### 规约

备注: map 和reduce的连接通常称为map-reduce模式，因Google用它来进行网络搜索而出名。

![image-20210522120632666](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210522120632666.png)

示例一：计算1-10的自然数的和

````java
   		List<Integer> list= Arrays.asList(1,2,3,4,5,6,7,8,9,10);
        Integer sum=list.stream().reduce(0,Integer::sum);
        System.out.println(sum);//55
````

示例二： 计算学生成绩总和

```java
  Integer sumScore = students.stream().map(x -> x.getScore()).reduce(0, Integer::sum);
        System.out.println(sumScore);
```

#### 收集

![image-20210522121811762](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210522121811762.png)

- Collector接口中方法的实现决定了如何对流执行收集的操作(如收集到List. Set.Map).
- 另外，Collectors 实用类提供了很多静态方法，可以方便地创建常见收集器实例，具体方法与实例如下表:

示例一：查找分数大于70的学生

```java
   List<Student> employeeList= students.stream().filter(x -> x.getScore() > 70).collect(Collectors.toList());
       
```

效果:

```
Student{age=12, name='小红', score=90}
Student{age=12, name='小明', score=85}
Student{age=12, name='小明明', score=72}
Student{age=12, name='大黑狗', score=89}
```

## Optional类

### Optional类简介

- 到目前为止，臭名昭著的空指针异常是导致Java应用程序失败的最常见原因。以前，为了解决空指针异常，Google公司著名的Guava项目引入了Optional类，Guava通过使用检查空值的方式来防止代码污染，它鼓励程序员写更干净的代
  码。受到Google Guava的启发，Optional类已经成为Java 8类库的一部分。

- `Optional<T>`类(`java.util.Optional`)是一-个容器类，它可以保存类型T的值，代表这个值存在。或者仅仅保存null,表示这个值不存在。原来用`null`表示一个值不存在，现在`Optional`可以更好的表达这个概念。并且可以避免空指针异常。
- Optional类的Javadoc描述如下:这是一个可以为null的容器对象。如果值存在则isPresent)方法会返回true,调用get()方法 会返回该对象。

### Optional类的方法

Optional提供很多有用的方法，这样我们就不用显式进行空值检测。

- 创建`Optional`类对象的方法:
  - `Optional.of(T t) `:创建一一个 Optional实例，t必须非空;
  - `Optional.empty()`:创建一个空的 Optional实例
  - `Optional.ofNullable(T t)`: t河以为null
- 判断`Optional`容器中是否包含对象:
    - `boolean isPresent()` :判断是否包含对象
    - `void ifPresent(Consumer<? super T> consumer)`:如果有值，就执行Consumer接口的实现代码，并且该值作为参数传给它。
- 获取`Optional`容器的对象:
    - `T get()`:如果调用对象包含值，返回该值，否则抛异常
    - `T orElse(T other)`如果有值则将其返回，否则返回指定的other对象。
    - `T orElseGet(Supplier<? extends T> other)`:如果有值则将其返回，否则返回由Supplier接口实现提供的对象。
    - `T orElse Throw(Supplier<? extends X> exceptionSupplier)`:如果有值则将其返回，否则抛出由Supplier接口实现提供的异常。

示例1:  使用of方法创建Optional对象

```java
Girl girl=new Girl();
Optional<Girl> girlOptional = Optional.of(girl);
```

此时故意将girl传值为`null`会报错:

```java
Girl girl=new Girl();
girl=null;
Optional<Girl> girlOptional = Optional.of(girl);
```

效果:

![image-20210522152832847](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210522152832847.png)

示例2：传入允许`null`值:

```java
  		Girl girl=new Girl();
        girl=null;
        Optional<Girl> girlOptional = Optional.ofNullable(girl);
        System.out.println(girlOptional);
```

效果:

```java
Optional.empty
```

示例3：Optional的应用场景

```java
 public static void main(String[] args) {
        Boy boy=new Boy();
        boy=null;
        String girlName=getGirlName(boy);
        System.out.println(girlName);
    }

    public static String getGirlName(Boy boy){
        return boy.getGirl().getName();
    }
```

效果:出现了空指针异常

```java
Exception in thread "main" java.lang.NullPointerException
	at com.dreamcold.java4.OptionalTest.getGirlName(OptionalTest.java:14)
	at com.dreamcold.java4.OptionalTest.main(OptionalTest.java:9)
```

优化之后的`getGirlName`方法:加入了optional的判断

```java
    public static void main(String[] args) {
        Boy boy=new Boy();
        boy=null;
        String girlName=getGirlName(boy);
        System.out.println(girlName);
    }

    public static String getGirlName(Boy boy){
        if (boy!=null){
            Girl girl=boy.getGirl();
            if (girl!=null){
                return girl.getName();
            }
        }
        return null;
    }
```

采用Optional来防止空指针异常:

```java
    public static String getGirlName(Boy boy){
        Optional<Boy> optionalBoy = Optional.ofNullable(boy);
        Boy boy1 = optionalBoy.orElse(new Boy(new Girl("xiaohong")));
        Girl girl=boy1.getGirl();
        Optional<Girl> optionalGirl = Optional.ofNullable(girl);
        Girl girl1 = optionalGirl.orElse(new Girl("xiaohua"));
        return girl1.getName();
    }
```

[更多细节可以参考](https://binghe.blog.csdn.net/article/details/106447083)

