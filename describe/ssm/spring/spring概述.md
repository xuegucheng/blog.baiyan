## Spring概述

#### Spring原理

* **AOP**：面向切面编程，**它是一种开发理念**，通过 AOP，我们可以把一些非业务逻辑的代码，比如安全检查，监控等代码从业务方法中抽取出来，以非侵入的方式与原方法进行协同。这样可以使原方法更专注于业务逻辑，代码结构会更加清晰，便于维护

>这里特别说明一下，AOP 并非是 Spring 独创，AOP 有自己的标准，也有机构在维护这个标准。Spring AOP 目前也遵循相关标准，所以别认为 AOP 是 Spring 独创的

* **IOC**：控制反转，控制权由对象本身转向容器；由容器根据配置文件去创建实例并创建各个实例之间的依赖关系，其核心是一个bean工厂

#### AOP

1. 通过代理模式为目标对象生产代理对象，并将横切逻辑插入到目标方法执行的前后、横切逻辑其实就是通知（Advice），Spring 提供了5种通知，Spring 需要为每种通知提供相应的实现类。除了以上说的这些，在具体的实现过程中，还要考虑如何将 AOP 和 IOC 整合在一起，毕竟 IOC 是 Spring 框架的根基。除此之外，还有其他一些需要考虑的地方，这里就不一一列举了。

2. 代理的两种方式：

   1. 静态代理：
      - 针对每个具体类分别编写代理类；
      - 针对一个接口编写一个代理类；

   2. 动态代理：
      * 