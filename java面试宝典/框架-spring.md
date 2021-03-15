9.1 什么是IOC，以及其原理？

9.2 IOC和DI的区别？ 

9.3 什么是aop

9.4 SpringBean实例化方式

9.5 springBean的生命周期

9.6 Spring的事物管理

9.7 Spring事物的隔离级别和传播行为

9.8 BeanFactory和ApplicationContext有什么区别？

9.9 Spring Bean的作用域之间有什么区别？

9.10 JDK动态代理和Gglib动态代理的区别

9.11 BeanFactory与FactoryBean的区别

 9.12 spring在bean的创建过程中解决循环依赖

9.13 @Transactional使用

9.14 获取spring容器的常用的4种方式

9.15 Spring支持三种依赖注入方式



11.1 什么是 Spring Boot？

11.2 Spring Boot 的核心配置文件有哪几个？它们的区别是什么？

11.3 Spring Boot 的配置文件有哪几种格式？它们有什么区别？

11.4 Spring Boot 的核心注解是哪个？它主要由哪几个注解组成的？

11.5 开启 Spring Boot 特性有哪几种方式？

11.6 Spring Boot 需要独立的容器运行吗？

11.7 运行 Spring Boot 有哪几种方式？

11.8 Spring Boot 自动配置原理是什么？

11.9 如何在 Spring Boot 启动的时候运行一些特定的代码？

11.10 Spring Boot 有哪几种读取配置的方式？

11.11 Spring Boot 支持哪些日志框架？推荐和默认的日志框架是哪个？

11.12 SpringBoot 实现热部署有哪几种方式？

11.13 你如何理解 Spring Boot 配置加载顺序？

11.14 Spring Boot 2.X 有什么新特性？与 1.X 有什么区别？
————————————————
版权声明：本文为CSDN博主「hzau_itdog」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/hzau_itdog/article/details/86745671


设计模式和设计原则

六大设计原则
常用设计模式
策略模式
模板方法模式
代理模式和委派模式的区别？
动态代理和静态代理的区别 ？
所谓静态也就是在程序运行前就已经存在代理类的字节码文件，代理类和委托类的关系在运行前就确定了。
缺点：
如果要代理的类型很多，势必要为每一种类型的方法都进行代理
如果接口增加一个方法，除了所有实现类需要实现这个方法外，所有代理类也需要实现此方法。显而易见，增加了代码维护的复杂度。
动态代理类的源码是在程序运行期间由JVM根据反射等机制动态的生成
代理模式的优点：程序解耦，功能增强
责任链模式
单例模式
工厂模式
装饰器模式–代码实现流程小米
抽象构件（Component）角色：定义一个抽象接口以规范准备接收附加责任的对象。
具体构件（ConcreteComponent）角色：实现抽象构件，通过装饰角色为其添加一些职责。
抽象装饰（Decorator）角色：继承抽象构件，并包含具体构件的实例，可以通过其子类扩展具体构件的功能。
具体装饰（ConcreteDecorator）角色：实现抽象装饰的相关方法，并给具体构件对象添加附加的责任。
通过构造器传入包装类实例，接口成员变量接收
优点：
装饰器是继承的有力补充，比继承灵活，在不改变原有对象的情况下，动态的给一个对象扩展功能，即插即用
通过使用不用装饰类及这些装饰类的排列组合，可以实现不同效果
装饰器模式完全遵守开闭原则
缺点：
装饰模式会增加许多子类，过度使用会增加程序得复杂性。
在项目中常用的设计模式有哪些（总结）
Spring中Bean的生命周期
Spring Bean 的作用域及适用于哪些场景？
Spring中IoC容器的初始化过程
Spring中的AOP的实现原理
JDK提供的代理实现方式和Cglib实现的区别，使用于哪些情况下？
JDK的动态代理为什么基于接口实现？
public final class $Proxy0 extends Proxy implements Interface
受限制于Java仅支持单继承
Spring中的事务
事务传播机制
事务实现的原理（代理模式、反射机制）
Spring中如何解决循环依赖问题？
Spring中@Bean注解如何解析？
Spring阅读源码入口？
Spring整合Mybatis是，实现原理？
Spring中的定时任务原理？
@Autowried注入的是同一个对象么？
spring中的单例如何解决线程安全问题的？ Bean默认是单例
使用ThreadLocal为线程提供共享变量的副本
ThreadLocal的原理
Spring和SpringBoot 的区别？
ImportSelector 和 ImportBeanDefinitionRegistrar的区别
ImportSelector是将类的全路径名直接注册给Spring的IOC，而ImportBeanDefinitionRegistrar就是通过BeanDefinition注册给spring。
MyBatis的底层就是通过实现了ImportBeanDefinitionRegistrar来实现的，在构建真正需要注册进入到IOC的实现类之前，MyBatis动态实现了我们指定的数据访问层的接口，然后将其构建BeanDefinition并注入到了IOC中
SpringBoot 在启动时如何热加载数据？
Spring Boot 在启动后，会找到全部 CommandLineRunner 接口的实例并运行它们的 run 方法。
可以通过 @Order 注解或者实现 Order 接口来指定 CommandLineRunner 的执行顺序。
Spring Boot 约定优于配置的体现？
项目目录结构
加载指定后缀，指定格式的配置文件
————————————————
版权声明：本文为CSDN博主「态度决定高度，习惯主宰人生」的原创文章，遵循CC 4.0 BY-SA版权协议，转载请附上原文出处链接及本声明。
原文链接：https://blog.csdn.net/weixin_42551200/article/details/109777624
