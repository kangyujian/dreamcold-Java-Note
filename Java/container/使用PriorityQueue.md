# 使用PriorityQueue

## 简介

1. `PriorityQueue`和`Queue`的区别在于，它的出队顺序与元素的优先级有关，对`PriorityQueue`调用`remove()`或`poll()`方法，返回的总是优先级最高的元素。
2. 放入`PriorityQueue`的元素，必须实现`Comparable`接口，`PriorityQueue`会根据元素的排序顺序决定出队的优先级。
3. `PriorityQueue`实现了一个优先队列：从队首获取元素时，总是获取优先级最高的元素。
4. `PriorityQueue`默认按元素比较的顺序排序（必须实现`Comparable`接口），也可以通过`Comparator`自定义排序算法（元素就不必实现`Comparable`接口）
5. PriorityQueue底层是采用堆来实现的



## 参考链接

1. 使用参考
   - https://www.liaoxuefeng.com/wiki/1252599548343744/1265120632401152
   - https://www.cnblogs.com/swiftma/p/6014636.html
2. 原理参考
   - https://www.cnblogs.com/Elliott-Su-Faith-change-our-life/p/7472265.html



