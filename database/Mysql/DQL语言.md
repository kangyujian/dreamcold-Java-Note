# DQL语言

## DQL

(Data Query Language ：数据查询语言)

- 所有的查询操作都用它： Select
- 简单的查询，复杂的查询它都能做
- 数据库最核心的语言，最重要的语句
- 使用频率最高的语句

> SELECT语法

```sql
SELECT [ALL | DISTINCT]
{* | table.* | [table.field1[as alias1][,table.field2[as alias2]][,...]]}
FROM table_name [as table_alias]
  [left | right | inner join table_name2]  -- 联合查询
  [WHERE ...]  -- 指定结果需满足的条件
  [GROUP BY ...]  -- 指定结果按照哪几个字段来分组
  [HAVING]  -- 过滤分组的记录必须满足的次要条件
  [ORDER BY ...]  -- 指定查询记录按一个或多个条件排序
  [LIMIT {[offset,]row_count | row_countOFFSET offset}];
   -- 指定查询的记录从哪条至哪条
```

**注意 : [ ] 括号代表可选的 , { }括号代表必选得**

## 指定查询字段

- 查询全部的学生 select 字段 from 表、

```sql
-- 查询全部的学生
-- SELECT 字段 FROM STUDENT;
SELECT * FROM STUDENT;
-- 查看指定的字段
SELECT `STUDENTNO`,`STUDENTNAME` FROM STUDENT;
-- 给字段起别名,也可以给表起别名
SELECT `STUDENTNO` AS 学号,`STUDENTNAME` AS 姓名 FROM STUDENT;
-- 函数
SELECT CONCAT('姓名',StudentName) as 新名字 FROM student;
```

> 有的时候，列名字不是那么的见名知意。我们起别名 AS 字段名as 别名 表名as别名

## 去重字段表达式

作用：去除SELECT查询出来的结果中的重复数据，只显示一条

```sql
-- 发现重复数据，去重
SELECT DISTINCT `STUDENTNO` FROM RESULT;

```

## 数据库的列(表达式)

```sql
--查看系统版本
SELECT VERSION();
--计算结果，用来计算
SELECT 100*3-1 AS 计算结果;
-- 查询自增的步长，变量
SELECT @@AUTO_INCREMENT_INCREMENT;
-- 学员考试的成绩+1分查看
SELECT `STUDENTNO`,`STUDENTRESULT`+1 AS '提分后' FROM RESULT;
```

数据库中的表达式：文本式、列、NULL、函数、计算表达式

`select 表达式 from 表;`

## WHERE条件字段

作用：检索数据中的`符合条件`的值

> 逻辑运算符

![image-20210217200250238](images/image-20210217200250238.png)

```sql
SELECT `STUDENTNO`,`STUDENTRESULT` FROM RESULT;

-- 查看考试成绩在95到100之间的
SELECT `STUDENTNO`,`STUDENTRESULT` FROM RESULT WHERE STUDENTRESULT>=95 AND STUDENTRESULT<=1;
-- AND 可以写成 &&
-- 模糊查询
SELECT `STUDENTNO`,`STUDENTRESULT` FROM RESULT WHERE STUDENTRESULT BETWEEN 95 AND 100;
-- 除了1000号学生之外的同学的成绩
SELECT `STUDENTNO`,`STUDENTRESULT` FROM RESULT STUDENTNO!=1000;
--- != NOT
SELECT `STUDENTNO`,`STUDENTRESULT` FROM RESULT WHERE NOT STUDENT=1000;
```

> 模糊查询：比较运算符

![image-20210217201051585](images/image-20210217201051585.png)

**LIKE**

```sql
-- 模糊查询
-- 1ike结合%(代表0到任意个字符) (一个字符)
SELECT `STUDETNO`,`STUDENTNAME` FROM `STUDENT` WHERE STUDENTNAME LIKE '刘%'
-- 查询姓刘的同学，名字后面只有一个字的
SELECT `STUDETNO`,`STUDENTNAME` FROM `STUDENT` WHERE STUDENTNAME LIKE '刘_'
-- 查询姓刘的同学,后面只有两个字的
SELECT `STUDETNO`,`STUDENTNAME` FROM `STUDENT` WHERE STUDENTNAME LIKE '刘__'
-- 查询姓名中含有 嘉 字的
SELECT `STUDETNO`,`STUDENTNAME` FROM `STUDENT` WHERE STUDENTNAME LIKE '%嘉%'
-- 查询姓名中含有特殊字符的需要使用转义符号 '\'
-- 自定义转义符关键字: ESCAPE ':'
```

**IN**

```sql
-- 查询学号为1000,1001,1002的学生姓名
SELECT STUDENTNO,STUDENTNAME FROM STUDENT WHERE STUDENTNO IN (1000,1001,1002)
-- 查询地址在北京,南京,河南洛阳的学生
SELECT studentno,studentname,address FROM student WHERE address IN ('北京','南京','河南洛阳');
```

**NULL**

```sql
-- 查询出生日期没有填写的同学
-- 不能直接写=NULL , 这是代表错误的 , 用 is null
SELECT STUDENTNAME FROM STUDENT WHERE BORNDATE IS NOT NULL;

-- 查询出生日期填写的同学
SELECT studentname FROM student WHERE BornDate IS NOT NULL;

-- 查询没有写家庭住址的同学(空字符串不等于null)
SELECT studentname FROM student WHERE Address='' OR Address IS NULL;

```

## 连接查询

[参考链接](https://blog.csdn.net/plg17/article/details/78758593)

> JOIN 对比

![image-20210225130652831](images/image-20210225130652831.png)

七种Join：

<img src="images/image-20210225130722659.png" alt="image-20210225130722659" style="zoom:80%;" />

测试

```sql
/*
连接查询
   如需要多张数据表的数据进行查询,则可通过连接运算符实现多个查询
内连接 inner join
   查询两个表中的结果集中的交集
外连接 outer join
   左外连接 left join
       (以左表作为基准,右边表来一一匹配,匹配不上的,返回左表的记录,右表以NULL填充)
   右外连接 right join
       (以右表作为基准,左边表来一一匹配,匹配不上的,返回右表的记录,左表以NULL填充)
       
等值连接和非等值连接

自连接
*/
-- 查询参加了考试的同学信息(学号,学生姓名,科目编号,分数)
SELECT * FROM student;
SELECT * FROM result;

/*思路:
(1):分析需求,确定查询的列来源于两个类,student result,连接查询
(2):确定使用哪种连接查询?(内连接)
*/
SELECT S.STUDENTNO,STUDENTNAME,SUBJECTNO,STUDENTRESULT FROM STUDENT S INNER JOIN RESULT R ON R.STUDENTNO=S.STUDENTNO

-- 右连接(也可实现)
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
RIGHT JOIN result r
ON r.studentno = s.studentno

-- 等值连接
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s , result r
WHERE r.studentno = s.studentno

-- 左连接 (查询了所有同学,不考试的也会查出来)
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
LEFT JOIN result r
ON r.studentno = s.studentno

-- 查一下缺考的同学(左连接应用场景)
SELECT s.studentno,studentname,subjectno,StudentResult
FROM student s
LEFT JOIN result r
ON r.studentno = s.studentno
WHERE StudentResult IS NULL

-- 思考题:查询参加了考试的同学信息(学号,学生姓名,科目名,分数)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON sub.subjectno = r.subjectno
```

> 自连接

```sql
/*
自连接
   数据表与自身进行连接

需求:从一个包含栏目ID , 栏目名称和父栏目ID的表中
    查询父栏目名称和其他子栏目名称
*/

-- 创建一个表
CREATE TABLE `category` (
`categoryid` INT(10) UNSIGNED NOT NULL AUTO_INCREMENT COMMENT '主题id',
`pid` INT(10) NOT NULL COMMENT '父id',
`categoryName` VARCHAR(50) NOT NULL COMMENT '主题名字',
PRIMARY KEY (`categoryid`)
) ENGINE=INNODB AUTO_INCREMENT=9 DEFAULT CHARSET=utf8

-- 插入数据
INSERT INTO `category` (`categoryid`, `pid`, `categoryName`)
VALUES('2','1','信息技术'),
('3','1','软件开发'),
('4','3','数据库'),
('5','1','美术设计'),
('6','3','web开发'),
('7','5','ps技术'),
('8','2','办公信息');

-- 编写SQL语句,将栏目的父子关系呈现出来 (父栏目名称,子栏目名称)
-- 核心思想:把一张表看成两张一模一样的表,然后将这两张表连接查询(自连接)
SELECT a.categoryName AS '父栏目',b.categoryName AS '子栏目'
FROM category AS a,category AS b
WHERE a.`categoryid`=b.`pid`

-- 思考题:查询参加了考试的同学信息(学号,学生姓名,科目名,分数)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON sub.subjectno = r.subjectno

-- 查询学员及所属的年级(学号,学生姓名,年级名)
SELECT studentno AS 学号,studentname AS 学生姓名,gradename AS 年级名称
FROM student s
INNER JOIN grade g
ON s.`GradeId` = g.`GradeID`

-- 查询科目及所属的年级(科目名称,年级名称)
SELECT subjectname AS 科目名称,gradename AS 年级名称
FROM SUBJECT sub
INNER JOIN grade g
ON sub.gradeid = g.gradeid

-- 查询 数据库结构-1 的所有考试结果(学号 学生姓名 科目名称 成绩)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='数据库结构-1'
```



## 排序与分页

```sql
/*============== 排序 ================
语法 : ORDER BY
   ORDER BY 语句用于根据指定的列对结果集进行排序。
   ORDER BY 语句默认按照ASC升序对记录进行排序。
   如果您希望按照降序对记录进行排序，可以使用 DESC 关键字。
   
*/

-- 查询 数据库结构-1 的所有考试结果(学号 学生姓名 科目名称 成绩)
-- 按成绩降序排序
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='数据库结构-1'
ORDER BY StudentResult DESC

/*============== 分页 ================
语法 : SELECT * FROM table LIMIT [offset,] rows | rows OFFSET offset
好处 : (用户体验,网络传输,查询压力)

推导:
   第一页 : limit 0,5
   第二页 : limit 5,5
   第三页 : limit 10,5
   ......
   第N页 : limit (pageNo-1)*pageSzie,pageSzie
   [pageNo:页码,pageSize:单页面显示条数]
   
*/

-- 每页显示5条数据
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='数据库结构-1'
ORDER BY StudentResult DESC , studentno
LIMIT 0,5

-- 查询 JAVA第一学年 课程成绩前10名并且分数大于80的学生信息(学号,姓名,课程名,分数)
SELECT s.studentno,studentname,subjectname,StudentResult
FROM student s
INNER JOIN result r
ON r.studentno = s.studentno
INNER JOIN `subject` sub
ON r.subjectno = sub.subjectno
WHERE subjectname='JAVA第一学年'
ORDER BY StudentResult DESC
LIMIT 0,10
```

## 子查询

```sql
/*============== 子查询 ================
什么是子查询?
   在查询语句中的WHERE条件子句中,又嵌套了另一个查询语句
   嵌套查询可由多个子查询组成,求解的方式是由里及外;
   子查询返回的结果一般都是集合,故而建议使用IN关键字;
*/

-- 查询 数据库结构-1 的所有考试结果(学号,科目编号,成绩),并且成绩降序排列
-- 方法一:使用连接查询
SELECT studentno,r.subjectno,StudentResult
FROM result r
INNER JOIN `subject` sub
ON r.`SubjectNo`=sub.`SubjectNo`
WHERE subjectname = '数据库结构-1'
ORDER BY studentresult DESC;

-- 方法二:使用子查询(执行顺序:由里及外)
SELECT studentno,subjectno,StudentResult
FROM result
WHERE subjectno=(
   SELECT subjectno FROM `subject`
   WHERE subjectname = '数据库结构-1'
)
ORDER BY studentresult DESC;

-- 查询课程为 高等数学-2 且分数不小于80分的学生的学号和姓名
-- 方法一:使用连接查询
SELECT s.studentno,studentname
FROM student s
INNER JOIN result r
ON s.`StudentNo` = r.`StudentNo`
INNER JOIN `subject` sub
ON sub.`SubjectNo` = r.`SubjectNo`
WHERE subjectname = '高等数学-2' AND StudentResult>=80

-- 方法二:使用连接查询+子查询
-- 分数不小于80分的学生的学号和姓名
SELECT r.studentno,studentname FROM student s
INNER JOIN result r ON s.`StudentNo`=r.`StudentNo`
WHERE StudentResult>=80

-- 在上面SQL基础上,添加需求:课程为 高等数学-2
SELECT r.studentno,studentname FROM student s
INNER JOIN result r ON s.`StudentNo`=r.`StudentNo`
WHERE StudentResult>=80 AND subjectno=(
   SELECT subjectno FROM `subject`
   WHERE subjectname = '高等数学-2'
)

-- 方法三:使用子查询
-- 分步写简单sql语句,然后将其嵌套起来
SELECT studentno,studentname FROM student WHERE studentno IN(
   SELECT studentno FROM result WHERE StudentResult>=80 AND subjectno=(
       SELECT subjectno FROM `subject` WHERE subjectname = '高等数学-2'
  )
)

/*
练习题目:
   查 C语言-1 的前5名学生的成绩信息(学号,姓名,分数)
   使用子查询,查询郭靖同学所在的年级名称
*/
```

