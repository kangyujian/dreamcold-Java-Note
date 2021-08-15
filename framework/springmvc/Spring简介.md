# Spring简介

- Spring是分层的Java SE/EE应用full-stack轻量级开源框架,以IoC ( Inverse Of Control :反转控制)和AOP ( Aspect Oriented Programming :面向切面编程)为内核。

-  提供了展现层SpringMVC和持久层Spring JDBCTemplate以及业务层事务管理等众多的企业级应用技术

- 还能整合开源世界众多著名的第三方框架和类库,逐渐成为使用最多的Java EE企业应用开源框架。

# Spring发展历程

- 1997年，IBM提出了 EJB的思想

- 1998年，SUN制定开发标准规范EJB1.0

- 1999年，EJB1.1发布

- 2001年，EJB2.0 发布

- 2003年，EJB2.1 发布

- 2006年，EJB3.0 发布

Rod Johnson ( Spring之父)

-  Expert One-to-One J2EE Design and Development(2002)
-  阐述宁J2EE使用EJB开发设计的优点及解决方案
-  Expert One to One J2EE Development without EJB(2004)
-  阐述了J2EE开发不使用EJB的解决方式(Spring雏形)

# Spring的优势

**1)方便解耦，简化开发**
通过Spring提供的loC容器，可以将对象间的依赖关系交由Spring进行控制，避免硬编码所造成的过度耦合。
用户也不必再为单例模式类、属性文件解析等这些很底层的需求编写代码，可以更专注于上层的应用。
**2) AOP编程的支持**
通过Spring的AOP功能，方便进行面向切面编程,许多不容易用传统OOP实现的功能可以通过AOP轻松实现。
**3)声明式事务的支持**
可以将我们从单调烦闷的事务管理代码中解脱出来,通过声明式方式灵活的进行事务管理，提高开发效率和质量。
**4)方便程序的测试**
可以用非容器依赖的编程方式进行几乎所有的测试工作，测试不再是昂贵的操作，而是随手可做的事情。
**5)方便集成各种优秀框架**
Spring对各种优秀框架(Struts、Hibernate. Hessian. Quartz等) 的支持。
**6)降低JavaEE API的使用难度**
Spring对JavaEE API (如JDBC、JavaMail、 远程调用等)进行了薄薄的封装层，使这些API的使用难度大为降低。

# Spring的体系结构

![image-20210718110926583](https://gitee.com/kangyujian/notebook-images/raw/master/images/image-20210718110926583.png)

