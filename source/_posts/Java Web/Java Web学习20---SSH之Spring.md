---
layout: post
title: Java Web学习20---SSH之Spring
comments: true
toc: true
date: 2017-04-02 08:12:23
tags:
	- Java
	- Spring
---

SSH（Struts，Spring，Hibernate） Struts进行流程控制，Spring进行业务流转，Hibernate进行数据库操作的封装。Spring是一个开放源代码的设计层面框架，他解决的是业务逻辑层和其他各层的松耦合问题，因此它将面向接口的编程思想贯穿整个系统应用。Spring是一个轻量级的Java 开发框架。

<!--more-->

# <font color=orange>Spring</font>

* Spring是一个开源框架
* Spring是于2003 年兴起的一个轻量级的Java开发框架，由Rod Johnson在其著作Expert One-On-One J2EE Development and Design中阐述的部分理念和原型衍生而来。
* 它是为了解决企业应用开发的复杂性而创建的。框架的主要优势之一就是其分层架构，分层架构允许使用者选择使用哪一个组件，同时为 J2EE 应用程序开发提供集成的框架。
* Spring使用基本的JavaBean来完成以前只可能由EJB完成的事情。然而，Spring的用途不仅限于服务器端的开发。从简单性、可测试性和松耦合的角度而言，任何Java应用都可以	从Spring中受益。
* Spring的核心是控制反转（IoC）和面向切面（AOP）。简单来说，Spring是一个分层的JavaSE/EEfull-stack(一站式) 轻量级开源框架。
* EE开发分成三层结构
	* WEB层		-- Spring MVC
	* 业务层	-- Bean管理:(IoC)
	* 持久层	-- Spring的JDBC模板.ORM模板用于整合其他的持久层框架
	

## <font color=orange>Spring框架的特点</font>
* 方便解耦，简化开发
	* Spring就是一个大工厂，可以将所有对象创建和依赖关系维护，交给Spring管理
* AOP编程的支持
	* Spring提供面向切面编程，可以方便的实现对程序进行权限拦截、运行监控等功能
* 声明式事务的支持
	* 只需要通过配置就可以完成对事务的管理，而无需手动编程
* 方便程序的测试
	* Spring对Junit4支持，可以通过注解方便的测试Spring程序
* 方便集成各种优秀框架
	* Spring不排斥各种优秀的开源框架，其内部提供了对各种优秀框架（如：Struts2、Hibernate、MyBatis、Quartz等）的直接支持
* 降低JavaEE API的使用难度
	* Spring 对JavaEE开发中非常难用的一些API（JDBC、JavaMail、远程调用等），都提供了封装，使这些API应用难度大大降低

## <font color=orange>Spring IoC快速入门</font>

### <font color=green> 什么是IoC? </font>
IoC	是Inverse of Control的缩写.控制反转,将对象的创建权反转给Spring！使用IOC可以解决的程序耦合性高的问题！

### <font color=green> 快速使用</font>
* 1、下载Spring框架的开发包
	* [官网](http://spring.io/)
	* [下载地址](http://repo.springsource.org/libs-release-local/org/springframework/spring)
	* 解压目录
		* docs: ------API和开发规范
		* libs: ------jar包和源码
		* schema: ------约束
* 2、Spring框架IoC核心功能需要导入的jar包
	* `Beans`
	* `Core`: 核心类
	* `Context`
	* `Expression`: 表达式
	* `spring-web-4.2.4.RELEASE.jar`: 整合Web需要
	* `spring-aop.jar`: 使用注解和AOP时需要
	* `com.springsource.org.apache.commons.logging-1.1.1.jar`: log规范
	* `com.springsource.org.apache.log4j-1.2.15.jar`: log4j实现
	* `log4j.properties`: log4j的配置文件到src目录下, 内容如下:

```java
### direct log messages to stdout 控制台###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### direct messages to file  自定义目录###
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=/Users/apple/Desktop/log4j
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### set log levels - for more verbose logging change 'info' to 'debug' 'error' ###

### 哪种模式显示
log4j.rootLogger=debug, stdout
```

* 3、编写Java类
* 4、Spring的配置文件
	* 4.1、在src目录下创建applicationContext.xml的配置文件，名称是可以任意的，但是一般都会使用默认名称！！
	* 4.2、beans schema的约束如下: 
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	#id是唯一标识, 可以为任意字符, class是实现类
	<bean id="userService" class="com.coppco.Demo1.UserServiceImp">
	</bean>
</beans>
```
	* 4.3、Bean标签的配置
		* `id`属性: 给Bean起个名字, 唯一约束, 必须以字母开始，可以使用字母、数字、连字符、下划线、句号、冒号.但是<font color=red>不能出现特殊字符</font>
		* `name`属性: 给Bean起个名字, 但是没有限制, 如果Bean没有id, name可以当id使用
			* 整合其他框架时使用, 如Strust1时必须使用`/`开头
		* `class`属性: Bean对象的全路径
		* `scope`属性
			* singleton: 单例(默认值)
			* prototype: 多例，在Spring框架整合Struts2框架的时候，Action类也需要交给Spring做管理，配置把Action类配置成多例！！
			* request: 应用在Web项目中,每次HTTP请求都会创建一个新的Bean
			* session: 应用在Web项目中,同一个HTTP Session 共享一个Bean
			* globalsession: 应用在Web项目中,多服务器间的session
		* `init-method`: 当Bean被载入到容器时执行的方法
		* `destroy-method`: 当Bean从容器中删除时执行的方法
			* 当scope为singleton时才有效
			* web容器中自动调用, 但是main函数和测试时需要手动调用ClassPathXmlApplicationContext对象的close()方法.
		
* 5、使用Spring工厂方法

```java
//创建工厂, 加载src目录下配置文件
ApplicationContext ac = new ClassPathXmlApplicationContext("applicationContext.xml");
//从工厂获取对象
UserService userService = (UserService) ac.getBean("userService");
```

### <font color=green>Spring中的工厂</font>

* ApplicationContent接口
	* ClassPathXmlApplicationContext实现类    -----加载src目录下配置文件
	* FileSystemXmlApplicationContext实现类   -----加载本地磁盘上的配置文件
* BeanFactory工厂(早期Spring获取Bean的方法)
* ApplicationContent和BeanFactory的区别
	* BeanFactory: 使用延迟加载, 在使用getBean时才会创建对象
	* ApplicationContent加载配置文件时就会创建对象
	* ApplicationContent支持事件传递、Bean自动装配和各种不同应用层的context实现

### <font color=green>依赖注入</font>
Dependency Injection，依赖注入，在Spring框架负责创建Bean对象时，动态的将依赖对象注入到Bean组件中！！

* 属性注入(setter使用的常见)
	* <font color=red>构造函数注入</font>: 需要提供属性和构造方法, 最后在`applicationContext.xml`中配置, `constructor-arg`标签中index是参数下标从0开始, value是值.
```xml
 <bean id="teacher" class="com.coppco.Demo1.Teacher">
        #使用下标
        <constructor-arg index="0" value="张三"/>
        <constructor-arg index="1" value="28"/>
        
        #也可以使用 name-value
        <constructor-arg name="name" value="李四" />
        <constructor-arg name="age" value="28" />
    </bean>
```
	* <font color=red>setter方法注入</font>: 需要提供属性和属性的setter方法, 最后在`applicationContext.xml`中配置, `property`标签中name是属性名, value是值.
```xml
<bean id="userService" class="com.coppco.Demo1.UserServiceImp" scope="singleton" init-method="init" destroy-method="destory">
        <property name="name" value="你好吗?"></property>
    </bean>
```
	* <font color=red>如果该属性是另外一个Bean类, 需要使用ref字段来映射到配置文件中对应类Bean标签的ID值或者name值.</font>
```
#使用构造方法
<constructor-arg index="1" ref="teacher"/>
#使用属性的setter方法
<property name="teacher" ref="teacher"></property>
```
	

* p名称空间注入(Spring2.5版本)
	* 首先在schema的名称空间引入: `xmlns:p="http://www.springframework.org/schema/p"`
	* 然后使用p.属性名="值"
	* 引用类型使用p.属性名-ref="ID值或name值"
* SpEL注入方式(Spring3.0)
* 数组，集合(List,Set,Map)，Properties等的注入
	* 如果是数组或者List集合，注入配置文件的方式是一样的
```xml
		<bean id="collectionBean" class="com.itheima.demo5.CollectionBean">
			<property name="arrs">
				<list>
					<ref bean="xxx"/> //bean
					<value>美美</value>
					<value>小风</value>
				</list>
			</property>
		</bean>
```
	* 如果是Set集合，注入的配置文件方式如下：
```xml
		<property name="sets">
			<set>
				<value>哈哈</value>
				<value>呵呵</value>
			</set>
		</property>
```
	* 如果是Map集合，注入的配置方式如下：
```xml
		<property name="map">
			<map>
				<entry key="老王2" value="38"/>
				<entry key="凤姐" value="38"/>
				<entry key="如花" value="29"/>
				<entry key-ref="如花" value-ref="xxx"/> //bean
			</map>
		</property>
```
	* 如果是properties属性文件的方式，注入的配置如下：
```xml
		<property name="pro">
			<props>
				<prop key="uname">root</prop>
				<prop key="pass">123</prop>
			</props>
		</property>
```

### <font color=green>Spring框架整合WEB </font>

整个项目, Spring的工厂配置文件应该只会创建一次, 当ServletContext对象创建的时候:创建工厂并将工厂存入到ServletContext域中, 使用时再取.ServletContextListener:用来监听ServletContext对象的创建和销毁的监听器.

* 首先需需要导入`引入spring-web-4.2.4.RELEASE.jar`包
* 配置web schema监听器(web.xml)

```xml
<!-- 配置Spring的核心监听器: -->
<listener>
    //初始化监听器默认加载WEB-INF目录下配置文件
    <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
</listener>
//配置src下配置文件
<context-param>
    <param-name>contextConfigLocation</param-name>
    <param-value>classpath:applicationContext.xml</param-value>
</context-param>
```
* 获取工厂
```java
//Struts2中
ServletContext servletContext = ServletActionContext.getServletContext();

//HttpServlet中
ServletContext servletContext = this.getServletContext();

//javax.servlet.Filter中
ServletContext servletContext = config.getServletContext();  

//其他方法中通过HttpRequest获得 

ServletContext servletContext = request.getSession().getServletContext(); 

WebApplicationContext webApplicationContext = WebApplicationContextUtils.getWebApplicationContext(servletContext);

//获取Bean
context.getBean("xxx");
```

## <font color=orange>Spring使用注解</font>

### <font color=green>基本使用</font>

* 1、除了导入基础包, 还需要导入`spring-aop.jar`
* 2、创建`src/ApplicationContext.xml`, 引入context schema约束, 并开启组件扫描
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       //引入注解的约束
       xmlns:context="http://www.springframework.org/schema/context" xsi:schemaLocation="
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd"> <!-- bean definitions here -->

    <!--//开启组件扫描-->
    <context:component-scan base-package="com.coppco"/>
</beans>
```
* 3、在需要管理的类上添加注解
```java
@Component(value="userService")	-- 相当于在XML的配置方式中 <bean id="userService" class="...">, 不加value默认是类名, 但是第一个字母小写
```

### <font color=green>常用注解</font>

* 1、`@Component`: 组件.(作用在类上)
* 2、 Spring中提供@Component的三个衍生注解:(功能目前来讲是一致的)
	* @Controller		-- 作用在WEB层
	* @Service			-- 作用在业务层
	* @Repository		-- 作用在持久层
	* 说明：这三个注解是为了让标注类本身的用途清晰，Spring在后续版本会对其增强
	
* 3、属性注入的注解(说明：使用注解注入的方式,可以不用提供set方法, 在属性上面使用)
	* 如果是注入的普通类型，可以使用value注解
		* @Value			-- 用于注入普通类型
	* 如果注入的是对象类型，使用如下注解
		* @Autowired		-- 默认按类型进行自动装配(可以单独使用, 但是一般不使用)
			* 如果想按名称注入
			* @Qualifier	-- 强制使用名称注入(需要使用时必须和上面的Autowired一起使用)
	* @Resource				-- 相当于@Autowired和@Qualifier一起使用
		* 强调：Java提供的注解
		* 属性使用name属性

### <font color=green>Bean的作用范围和生命周期</font>

* Bean的作用范围注解
	* 注解为@Scope(value="prototype")，作用在类上。值如下：
		* singleton		-- 单例，默认值
		* prototype		-- 多例	
* Bean的生命周期的配置（了解）
	* 注解如下：
		* @PostConstruct	-- 相当于init-method
		* @PreDestroy		-- 相当于destroy-method

### <font color=green>Spring框架整合JUnit单元测试</font>	
* 为了简化了JUnit的测试，使用Spring框架也可以整合测试, 不用每次在测试方法中加载工厂.
* 具体步骤
	* 要求：必须先有JUnit的环境（即已经导入了JUnit4的开发环境, 注意需要<font color=red>JUnit4.9</font>版本以上）！！
	* 步骤一：在程序中引入:`spring-test.jar`
	* 步骤二：在具体的测试类上添加注解
	
```java
//Spring提供的测试类
@RunWith(SpringJUnit4ClassRunner.class)	
//加载配置文件
@ContextConfiguration("classpath:applicationContext.xml")
public class SpringDemo1 {

    @Resource(name="userService")
    private UserService userService;
    
    @Test
    public void demo2(){
        userService.save();
    }
}
```

## <font color=orange>Spring AOP</font>
### <font color=green>什么是AOP?</font>

&emsp;&emsp;AOP是Aspect Oriented Programming的缩写, 意思是<font color=red>面向切面编程</font>, 它是一种编程范式，隶属于软工范畴，指导开发者如何组织程序结构。
&emsp;&emsp;AOP最早由AOP联盟的组织提出的,制定了一套规范。Spring将AOP思想引入到框架中,必须遵守AOP联盟的规范, 利用AOP可以对业务逻辑的各个部分进行隔离，从而使得业务逻辑各部分之间的耦合度降低，提高程序的可重用性，同时提高了开发的效率。
&emsp;&emsp;AOP采取横向抽取机制，取代了传统纵向继承体系重复性代码（性能监视、事务管理、安全检查、缓存）,<font color=red>可以在不修改源代码的前提下，对程序进行增强！！</font>

### <font color=green>Spring框架的AOP的底层实现</font>

Srping框架的AOP技术底层也是采用的代理技术，代理的方式提供了两种: 

* 基于JDK的动态代理
	* 必须是面向接口的，只有实现了具体接口的类才能生成代理对象
* 基于CGLIB动态代理
	* 对于没有实现了接口的类，也可以产生代理，产生这个类的子类的方式

Spring的传统AOP中根据类是否实现接口，来采用不同的代理方式: 

* 如果实现类接口，使用JDK动态代理完成AOP
```java
//这里的userDao实际上是接口UserDao的实现类
UserDao dao = (UserDao) Proxy.newProxyInstance(userDao.getClass().getClassLoader(),userDao.getClass().getInterfaces(), new InvocationHandler() {
    @Override
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        Object obj =  method.invoke(userDao, args);
        if (method.getName().equals("save")) {
            log.info("=======================");
        } else if (method.getName().equals("update")) {
            log.info("-----------------------");
        } else if (method.getName().equals("delete")) {
            log.info("***********************");
        }
        return obj;
    }
});

dao.save();
```
* 如果没有实现接口，采用CGLIB动态代理完成AOP
	* 需要导入`CGLIB.jar`包, 而Spring核心包中已经包含了该包.
```java
//创建CGLIB核心的类
Enhancer enhancer = new Enhancer();
 //设置父类
enhancer.setSuperclass(UserDaoIMP.class);
//设置回调函数
enhancer.setCallback(new MethodInterceptor() {
    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        return methodProxy.invokeSuper(o, objects);
    }
});
//生成代理对象
UserDao dao = (UserDao) enhancer.create();
dao.delete();
```

## <font color=orange>Spring基于AspectJ的AOP的开发</font>

### <font color=green>AOP的相关术语</font>
	
* Joinpoint(连接点)	-- 所谓连接点是指那些被拦截到的点。<font color=red>在Spring中,这些点指的是方法</font>,因为Spring只支持方法类型的连接点
* Pointcut(切入点)		-- 所谓切入点是指我们要对哪些Joinpoint进行拦截的定义
* Advice(通知/增强)	-- 所谓通知是指拦截到Joinpoint之后所要做的事情就是通知.通知分为前置通知,后置通知,异常通知,最终通知,环绕通知(切面要完成的功能)
* Introduction(引介)	-- 引介是一种特殊的通知在不修改类代码的前提下, Introduction可以在运行期为类动态地添加一些方法或Field
* Target(目标对象)		-- 代理的目标对象
* Weaving(织入)		-- 是指把增强应用到目标对象来创建新的代理对象的过程
* Proxy（代理）		-- 一个类被AOP织入增强后，就产生一个结果代理类
* Aspect(切面)			-- 是切入点和通知的结合，以后咱们自己来编写和配置的


### <font color=green>AspectJ的XML方式完成AOP的开发</font>

* 1、导入jar包
	* Spring的基础包Beans、Core、Context、Expression和log4j的相关jar包
	* Spring的AOP开发包
		* 导入Spring的AOP包`spring-aop-4.2.4.RELEASE.jar`
		* AOP规范包: `com.springsource.org.aopalliance-1.0.0.jar`
	* AspectJ开发包
		* `spring-aspects-4.2.4.RELEASE.jar`
		* `com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar`
* 2、配置Spring的配置文件, 引入 aop schema约束
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop.xsd">
    
    <!--//开启注解扫描-->
    <context:component-scan base-package="com.coppco"/>

    <!--配置Bean-->

    <!--配置aop-->
</beans>
```
* 3、创建接口和实现类, 并配置到Spring配置文件中
```
<bean id="customerDao" class="com.coppco.Custom.CustomDaoIMP"/>
```
* 4、创建切面类和方法, 并配置到Spring配置文件中
```
<bean id="customAspect" class="com.coppco.Custom.CustomAspect"/>
```
* 5、AOP的配置
```
<aop:config>
    <!-- 引入切面类 -->
    <aop:aspect ref="customAspect">
        <!-- 定义通知类型、切面类的方法和切入点的表达式 -->
        <aop:before method="log" pointcut="execution(public * com.coppco.Custom.CustomDaoIMP.save(..))"/>
    </aop:aspect>
</aop:config>
```

### <font color=green>切入点的表达式</font>

再配置切入点的时候，需要定义表达式，重点的格式如下：execution(public \* \*(..))，具体展开如下：
* 切入点表达式的格式如下：
	* execution([修饰符] 返回值类型 包名.类名.方法名(参数))
		
* 修饰符可以省略不写，不是必须要出现的。
* 返回值类型是不能省略不写的，根据你的方法来编写返回值。可以使用 \* 代替。
* 包名例如：com.itheima.demo3.BookDaoImpl
	* 首先com是不能省略不写的，但是可以使用 * 代替
	* 中间的包名可以使用 * 号代替
	* 如果想省略中间的包名可以使用 .. 
		
* 类名也可以使用 \* 号代替，也有类似的写法：\*DaoImpl
* 方法也可以使用 \* 号代替
* 参数如果是一个参数可以使用 * 号代替，如果想代表任意参数使用 ..

### <font color=green>AOP的通知类型</font>
	
* 前置通知
	* 在目标类的方法执行之前执行。
	* 配置文件信息：<aop:after method="before" pointcut-ref="myPointcut3"/>
	* 应用：可以对方法的参数来做校验
	
* 最终通知
	* 在目标类的方法执行之后执行，如果程序出现了异常，最终通知也会执行。
	* 在配置文件中编写具体的配置：<aop:after method="after" pointcut-ref="myPointcut3"/>
	* 应用：例如像释放资源
	
* 后置通知
	* 方法正常执行后的通知。		
	* 在配置文件中编写具体的配置：<aop:after-returning method="afterReturning" pointcut-ref="myPointcut2"/>
	* 应用：可以修改方法的返回值
	
* 异常抛出通知
	* 在抛出异常后通知
	* 在配置文件中编写具体的配置：<aop:after-throwing method="afterThorwing" pointcut-ref="myPointcut3"/>	
	* 应用：包装异常的信息
	
* 环绕通知
	* 方法的执行前后执行。
	* 在配置文件中编写具体的配置：<aop:around method="around" pointcut-ref="myPointcut2"/>
	* 要注意：目标的方法默认不执行，需要使用ProceedingJoinPoint对来让目标对象的方法执行。

	
### <font color=green>AspectJ注解方式完成AOP的开发</font>

* 1、导入jar包
	* Spring的基础包Beans、Core、Context、Expression和log4j的相关jar包
	* Spring的AOP开发包
		* 导入Spring的AOP包`spring-aop-4.2.4.RELEASE.jar`
		* AOP规范包: `com.springsource.org.aopalliance-1.0.0.jar`
	* AspectJ开发包
		* `spring-aspects-4.2.4.RELEASE.jar`
		* `com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar`
* 2、配置Spring的配置文件, 引入相关 Schema 约束
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop.xsd
	http://www.springframework.org/schema/tx 
	http://www.springframework.org/schema/tx/spring-tx.xsd">

	<!--开启注解扫描-->
	<context:component-scan base-package="com.coppco"/>

	<!--开启自动代理-->
	<aop:aspectj-autoproxy/>
</beans>
```
* 3、新建接口、实现类和切面类, 并配置注解
	* 实现类注解
```java
//实现类上面注解
@Component(value = "customerImpl")
public class CustomerImpl implements Customer {
    //code
}
```
	* 切面类
		* @Before				-- 前置通知
		* @AfterReturing		-- 后置通知
		* @Around				-- 环绕通知（目标对象方法默认不执行的，需要手动执行）
		* @After				-- 最终通知
		* @AfterThrowing		-- 异常抛出通知
	  
```java
//定义Bean的注解
@Component(value = "aspectCustomer")
//定义切面类的注解
@Aspect 
public class AspectCustomer {
    //AOP配置
    @Before(value = "execution(public void com.coppco.Demo1.CustomerImpl.save())")
    public void log() {
        log.warn("记录日志~~~~~~~~~~~~~~~~~~~~~~");
    }
}
```

* 4、在`applicationContext.xml`中必须开启注解扫描和自动代理
```xml
<!--开启注解扫描-->
<context:component-scan base-package="com.coppco"/>
<!--开启自动代理-->
<aop:aspectj-autoproxy/>
```
* 5、测试, <font color=red>__这里需要注意的是customer一定要写接口的名称, 不能写实现类的类名,不然会报错!!!!!!__</font>
```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")
public class SpringTest {

    @Resource(name = "customerImpl")
    private Customer customer;

    @Test
    public void test1() {
        customer.save();
        customer.update();
    }
}
```
	* 如果写成实现类, 那么会报错`Bean named 'xxxx' must be of type [com.coppco.Demo1.xxx], but was actually of type [com.sun.proxy.$Proxy15]`
	* 这是因为Spring的AOP实现, 使用JDK的动态代理和CGLIB来实现, 如果Target(目标对象)至少实现了一个接口, 那么就会使用JDK的动态代理来实现, 而Target没有实现接口, 会使用CGLIB来实现.

### <font color=green>通用的切入点注解</font>
```java
@Component(value = "myAspectAnno")
@Aspect
public class MyAspectAnno {
    @Before(value="MyAspectAnno.fn()")
    public void log(){
        System.out.println("记录日志...");
    }
    
    @Pointcut(value="execution(public void com.itheima.demo1.CustomerDaoImpl.save())")
    public void fn(){}
    }
}
```

## <font color=orange>Spring框架的JDBC模板技术</font> 

Spring框架中提供了很多持久层的模板类来简化编程，使用模板类编写程序会变的简单
* Spring框架提供的JdbcTemplate模板类
* Spring框架可以整合Hibernate框架，也提供了HibernateTemplate模板类.

### <font color=green>使用JDBC模板类JdbcTemplate </font> 

* 1、创建数据
```
drop database if exists springJDBC;
create database springJDBC character set utf8;
use springJDBC;
create table account(
	id int primary key auto_increment comment '主键id',
	name varchar(20) comment '姓名',
	sex varchar(4) comment '性别'
) comment='用户表';
```
* 2、新建项目导入jar包
	* spring-beans.jar
	* spring-core.jar
	* spring-context.jar
	* spring-expression.jar
	* log4j.jar
	* logging.jar
	* spring-aop.jar
	* spring-jdbc.jar
	* spring-tx.jar
	* spring-test.jar
	* mysql-connector-java.jar
	* junit.jar(需要4.9及以后版本)
* 3、使用连接池类和模板类
	* Spring内置连接池可以使用new的方式创建, 也可以通过配置Spring来创建
		* 原始方式
```java
//Spring提供了内置的连接池
DriverManagerDataSource dataSource = new DriverManagerDataSource();
dataSource.setDriverClassName("com.mysql.jdbc.Driver");
dataSource.setUrl("jdbc:mysql://192.168.1.184:3306/springJDBC");
dataSource.setUsername("root");
dataSource.setPassword("123456");

//JDBC模板类
JdbcTemplate template = new JdbcTemplate(dataSource);
template.update("insert into account VALUES (null, ?, ?)", "熊大", "男");
```
		* Spring管理方式
```xml
	<!--配置连接池, 单例-->
	<bean id="dataSource" class="org.springframework.jdbc.datasource.DriverManagerDataSource" >
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://192.168.1.184/springJDBC"/>
        <property name="username" value="root"/>
        <property name="password" value="123456"/>
	</bean>
	
	<!--JDBC模板类-->
	<bean id="jdbcTemplate" class="org.springframework.jdbc.core.JdbcTemplate">
        <property name="dataSource" ref="dataSource"/>
	</bean>
```

### <font color=green>Spring框架管理开源的连接池</font> 

* 1、管理DBCP连接池
	* 先引入DBCP的2个jar包
		* com.springsource.org.apache.commons.dbcp-1.2.2.osgi.jar
		* com.springsource.org.apache.commons.pool-1.5.3.jar
		
	* 编写配置文件
```
<bean id="dataSource" class="org.apache.commons.dbcp.BasicDataSource">
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="jdbcUrl" value="jdbc:mysql://192.168.1.184/springJDBC"/>
    <property name="username" value="root"/>
    <property name="password" value="123456"/>
</bean>
```
* 2、管理C3P0连接池
	* 先引入C3P0的jar包
		* com.springsource.com.mchange.v2.c3p0-0.9.1.2.jar
		
	* 编写配置文件
```
<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
    <property name="driverClass" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://192.168.1.184/springJDBC"/>
    <property name="user" value="root"/>
    <property name="password" value="123456"/>
</bean>
```

## <font color=orange>Spring框架的事务管理 </font>
### <font color=green>Spring框架的事务管理相关的类和API</font>
* PlatformTransactionManager接口		-- 平台事务管理器.(真正管理事务的类)。该接口有具体的实现类，根据不同的持久层框架，需要选择不同的实现类！
* TransactionDefinition接口			-- 事务定义信息.(事务的隔离级别,传播行为,超时,只读)
* TransactionStatus接口				-- 事务的状态
* 总结：上述对象之间的关系：平台事务管理器真正管理事务对象.根据事务定义的信息TransactionDefinition 进行事务管理，在管理事务中产生一些状态.将状态记录到TransactionStatus中
* PlatformTransactionManager接口中实现类和常用的方法
	* 接口的实现类
		* 如果使用的Spring的JDBC模板或者MyBatis框架，需要选择DataSourceTransactionManager实现类
		* 如果使用的是Hibernate的框架，需要选择HibernateTransactionManager实现类
		
	* 该接口的常用方法
		* void commit(TransactionStatus status) 
		* TransactionStatus getTransaction(TransactionDefinition definition) 
		* void rollback(TransactionStatus status) 
* TransactionDefinition
	* 事务隔离级别的常量
		* static int ISOLATION_DEFAULT 	-- 采用数据库的默认隔离级别
		* static int ISOLATION_READ_UNCOMMITTED 
		* static int ISOLATION_READ_COMMITTED 
		* static int ISOLATION_REPEATABLE_READ 
		* static int ISOLATION_SERIALIZABLE 
 		
	* 事务的传播行为常量（不用设置，使用默认值）
		* 先解释什么是事务的传播行为：解决的是业务层之间的方法调用！！
		* PROPAGATION_REQUIRED（默认值）	-- A中有事务,使用A中的事务.如果没有，B就会开启一个新的事务,将A包含进来.(保证A,B在同一个事务中)，默认值！！
		* PROPAGATION_SUPPORTS			-- A中有事务,使用A中的事务.如果A中没有事务.那么B也不使用事务.
		* PROPAGATION_MANDATORY			-- A中有事务,使用A中的事务.如果A没有事务.抛出异常.
		* PROPAGATION_REQUIRES_NEW（记）-- A中有事务,将A中的事务挂起.B创建一个新的事务.(保证A,B没有在一个事务中)
		* PROPAGATION_NOT_SUPPORTED		-- A中有事务,将A中的事务挂起.
		* PROPAGATION_NEVER 			-- A中有事务,抛出异常.
		* PROPAGATION_NESTED（记）		-- 嵌套事务.当A执行之后,就会在这个位置设置一个保存点.如果B没有问题.执行通过.如果B出现异常,运行客户根据需求回滚(选择回滚到保存点或者是最初始状态)
	
### <font color=green>转账测试案例（强调：简化开发，以后DAO可以继承JdbcDaoSupport类） </font>

* 1、新建项目导入相关jar包
	* Spring中IoC相关jar包以及log4j的jar包: `spring-beans-4.2.4.RELEASE.jar`、`spring-context-4.2.4.RELEASE.jar`、`spring-core-4.2.4.RELEASE.jar`、`spring-expression-4.2.4.RELEASE.jar`、`com.springsource.org.apache.log4j-1.2.15.jar`和`com.springsource.org.apache.commons.logging-1.1.1.jar`
	* Spring中AOP相关jar包和依赖jar包: `spring-aop-4.2.4.RELEASE.jar`、`com.springsource.org.aopalliance-1.0.0.jar`、`spring-aspects-4.2.4.RELEASE.jar`、`com.springsource.org.aspectj.weaver-1.6.8.RELEASE.jar`
	* C3P0的jar包: `com.springsource.com.mchange.v2.c3p0-0.9.1.2.jar`
	* MySQL的驱动包: `mysql-connector-java-5.1.42-bin.jar`
	* Spring中JDBC的jar包: `spring-jdbc-4.2.4.RELEASE.jar`和`spring-tx-4.2.4.RELEASE.jar`
	* Spring中测试的jar包: `spring-test-4.2.4.RELEASE.jar`
	* jutil的jar包: `junit-4.9.jar`
* 2、在`src`目录中拷贝log4j的配置文件`log4j.properties`和Spring的配置文件`applicationContext.xml`
	* log4j.properties
```xml
### direct log messages to stdout ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### direct messages to file mylog.log ###
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=c\:mylog.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### set log levels - for more verbose logging change 'info' to 'debug' ###

log4j.rootLogger=info, stdout
```
	* applicationContext.xml
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop.xsd
	http://www.springframework.org/schema/tx 
	http://www.springframework.org/schema/tx/spring-tx.xsd">

	<!--开启注解扫描-->
	<context:component-scan base-package="com"/>

	<!--开启自定代理-->
	<aop:aspectj-autoproxy/>

	<!--配置c3p0-->
	<bean id="dataSource" class="com.mchange.v2.c3p0.DriverManagerDataSource">
		<property name="driverClass" value="com.mysql.jdbc.Driver"/>
		<property name="jdbcUrl" value="jdbc:mysql://192.168.1.184:3306/springJDBC"/>
		<property name="user" value="root"/>
		<property name="password" value="123456"/>
	</bean>

	<!--配置JDBC模板-->
	<bean id="template" class="org.springframework.jdbc.core.JdbcTemplate">
		<property name="dataSource" ref="dataSource"/>
	</bean>
</beans>

```
* 3、新建AccountService接口以及实现类AccountServiceImpl, AccountDao接口和实现类AccountDaoImpl
在AccountServiceImpl类中需要调用Dao接口,  所以可以通过注入的方式进程管理, Spring中可以选择配置文件, 也可以使用注解的方式.这里使用继承于Object注解: 
```java
@Service(value = "accountService")
public class AccountServiceImpl implements AccountService {

    @Resource(name = "accountDao")
    private AccountDao accountDao;
```

```java
@Repository(value = "accountDao")
public class AccountDaoImpl implements AccountDao{

    @Resource(name = "template")
    private JdbcTemplate template;
}
```

实际开发中Dao类一般是继承于JdbcDaoSupport类, 该类提供了一个JdbcTemplate模板类, 可以通过配置文件注入, 注解的话, 可以通过@PostConstruct注解在初始化时setJdbcTemplate()方法初始化.但是一般是通过设置自定一个私有DataSource, 通过@PostConstruct注解在初始化时setDataSource()方法创建模板类.
```java
@Repository(value = "accountDao")
public class AccountDaoImpl extends JdbcDaoSupport implements AccountDao{

    @Resource(name = "dataSource")
    private DataSource dataSource;

    //这个注解是Bean初始化的时候执行的方法.
    @PostConstruct
    private void initialize() {
        setDataSource(dataSource);
    }
```
通过JdbcDaoSupport源码可以看出, JdbcDaoSupport的setDataSource方法会创建一个模板类, 所以可以通过注入并设置dataSource来进行模板类的创建.
```java
public final void setDataSource(DataSource dataSource) {
        if (this.jdbcTemplate == null || dataSource != this.jdbcTemplate.getDataSource()) {
            this.jdbcTemplate = this.createJdbcTemplate(dataSource);
            this.initTemplateConfig();
        }

    }
```

* Service实现类中代码
```java
@Service(value = "accountService")
public class AccountServiceImpl implements AccountService {

    @Resource(name = "accountDao")
    private AccountDao accountDao;

    @Override
    public void pay(String in, String out, float monery) {

        accountDao.outMonery(out, monery);
        

        accountDao.inMonery(in,monery);
    }
}
```
* Dao实现类中代码
```java
@Repository(value = "accountDao")
public class AccountDaoImpl extends JdbcDaoSupport implements AccountDao{

    @Resource(name = "dataSource")
    private DataSource dataSource;

    @PostConstruct
    private void initialize() {
        setDataSource(dataSource);
    }

    @Override
    public void outMonery(String out, float monery) {
        String sql = "update account set monery = monery -" + monery + "where name ='" + out + "';";

        this.getJdbcTemplate().update(sql);
    }

    @Override
    public void inMonery(String in, float monery) {
        String sql = "update account set monery = monery +" + monery + "where name ='" + in + "';";

        this.getJdbcTemplate().update(sql);
    }

}
```
* 4、<font color=red>事务的添加</font>
在上面的代码中, 直接调用会出现问题, 假如出现异常可能出现转账人钱扣了, 但是收款人的钱没有增加, 所以需要增加事务.Spring提供了模板类 TransactionTemplate，所以手动编程的方式来管理事务，只需要使用该模板类即可！！

	* Spring的编程式事务管理: 通过手动编写代码的方式完成事务的管理（不推荐）
		* 配置一个事务管理器, 使用的Spring的JDBC模板或者MyBatis框架，需要选择DataSourceTransactionManager实现类.
```xml
	<!--配置事务管理器-->
	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"/>
	</bean>
```
		* 配置事务管理的模板
```xml
	<!--配置事务管理模板类-->
	<bean id="transactionTemplate" class="org.springframework.transaction.support.TransactionTemplate">
		<property name="transactionManager" ref="transactionManager"/>
	</bean>
```
		* 在需要进行事务管理的类中,注入事务管理的模板(可以通过配置文件和注解注入两种方式)
```java
@Resource(name = "transactionTemplate")
private TransactionTemplate transactionTemplate;
```
		* 使用事务(AccountServiceImpl中)
```java
transactionTemplate.execute(new TransactionCallbackWithoutResult() {
    @Override
    protected void doInTransactionWithoutResult(TransactionStatus transactionStatus) {
        accountDao.outMonery(out, monery);
        int a = 1 / 0;
        accountDao.inMonery(in,monery);
    }
});
```
	* Spring的声明式事务管理: 通过一段配置的方式完成事务的管理（分为配置文件和注解两种方式）
		* 基于AspectJ的XML方式
			* 配置事务管理器
```xml
	<!--配置事务管理器-->
	<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
		<property name="dataSource" ref="dataSource"/>
	</bean>
```
			* 配置事务增强
```xml
<!-- 配置事务增强 -->
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <!--
        name		：绑定事务的方法名，可以使用通配符，可以配置多个。
        propagation	：传播行为
        isolation	：隔离级别
        read-only	：是否只读
        timeout		：超时信息
        rollback-for：发生哪些异常回滚.
        no-rollback-for：发生哪些异常不回滚.
        -->
    <!-- 哪些方法加事务 -->
        <tx:method name="pay" propagation="REQUIRED"/>
    </tx:attributes>
</tx:advice>
```
			* 配置AOP的切面
				* 如果是自己编写的切面，使用`<aop:aspect>`标签，如果是系统制作的，使用`<aop:advisor>`标签
```xml
<!-- 配置AOP切面产生代理 -->
<aop:config>
    <aop:advisor advice-ref="myAdvice" pointcut="execution(* com.itheima.demo2.AccountServiceImpl.pay(..))"/>
</aop:config>
```

		* <font color=red>基于AspectJ的注解方式(最简单的方式)</font>
			* 配置事务管理器
```xml
<!--配置事务管理器-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>
```
			* 开启注解事务
```xml
<!-- 开启注解事务 -->
<tx:annotation-driven transaction-manager="transactionManager"/>
```
			* 在业务层上添加一个注解:@Transactional, 可以在类上也可以在方法上添加.
			
# <font color=orange>SSH的整合</font>
## <font color=orange>手动整合</font>
* 新建项目, 导入jar包
	* 可以手动导入相关的jar包
* 配置文件
	* 修改`web.xml`, 加入Struts2和Spring的配置
```xml
    <!--配置Struts2-->
    <filter>
        <filter-name>struts2</filter-name>
        <!--配置Struts2, class: 不同版本可能不一样-->
        <filter-class>org.apache.struts2.dispatcher.ng.filter.StrutsPrepareAndExecuteFilter</filter-class>
    </filter>

    <filter-mapping>
        <filter-name>struts2</filter-name>
        <url-pattern>/*</url-pattern>
    </filter-mapping>

    <!-- 配置Spring的核心监听器: -->
    <listener>
        <!--初始化监听器默认加载WEB-INF目录下配置文件-->
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    <!--配置src下配置文件-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath:applicationContext.xml</param-value>
    </context-param>
```
	* 添加`applicationContext.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
	xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xmlns:context="http://www.springframework.org/schema/context"
	xmlns:aop="http://www.springframework.org/schema/aop"
	xmlns:tx="http://www.springframework.org/schema/tx"
	xsi:schemaLocation="http://www.springframework.org/schema/beans 
	http://www.springframework.org/schema/beans/spring-beans.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context.xsd
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop.xsd
	http://www.springframework.org/schema/tx 
	http://www.springframework.org/schema/tx/spring-tx.xsd">
	
</beans>
```
	* 添加`hibernate.cfg.xml`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE hibernate-configuration PUBLIC
	"-//Hibernate/Hibernate Configuration DTD 3.0//EN"
	"http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">
	
<hibernate-configuration>
	
	<session-factory>
		<!-- 必须配置 -->
		<property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
		<property name="hibernate.connection.url">jdbc:mysql://192.168.1.191:3306/hibernate_day04</property>
		<property name="hibernate.connection.username">root</property>
		<property name="hibernate.connection.password">123456</property>
		<property name="hibernate.dialect">org.hibernate.dialect.MySQLDialect</property>
		
		<!-- 可选配置 -->
		<property name="hibernate.show_sql">true</property>
		<property name="hibernate.format_sql">true</property>
		<property name="hibernate.hbm2ddl.auto">update</property>
		
		<!-- 配置C3P0的连接池 -->
		<property name="connection.provider_class">org.hibernate.c3p0.internal.C3P0ConnectionProvider</property>
		
		<!-- 映射配置文件 -->
		<!--<mapping resource="xxxxx"/>-->
	</session-factory>
	
</hibernate-configuration>	
```
	* 添加`log4j.properties`
```xml
### direct log messages to stdout ###
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.Target=System.err
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### direct messages to file mylog.log ###
log4j.appender.file=org.apache.log4j.FileAppender
log4j.appender.file.File=c\:mylog.log
log4j.appender.file.layout=org.apache.log4j.PatternLayout
log4j.appender.file.layout.ConversionPattern=%d{ABSOLUTE} %5p %c{1}:%L - %m%n

### set log levels - for more verbose logging change 'info' to 'debug' ###

log4j.rootLogger=info, stdout
```
	* 添加`struts.xml`
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE struts PUBLIC
	"-//Apache Software Foundation//DTD Struts Configuration 2.3//EN"
	"http://struts.apache.org/dtds/struts-2.3.dtd">
	
<struts>
	<package name="day38" extends="struts-default" namespace="/">
		<!--配置action, 通配符-->
		<action name="customer_*" class="com.coppco.web.action.CustomerAction" method="{1}"></action>
	</package>
</struts>
```
* 测试Struts2
	* 编写Action类, 一般继承ActionSupport类(有一些常量), 实现ModelDriven接口(接受参数, 封装成Model)
	* 私有一个Model属性并初始化, 实现getModel()方法, 返回该私有属性.
```java
package com.coppco.web.action;

import com.coppco.domain.Customer;
import com.coppco.service.CustomerInterface;
import com.opensymphony.xwork2.ActionSupport;
import com.opensymphony.xwork2.ModelDriven;

import javax.annotation.Resource;

public class CustomerAction extends ActionSupport implements ModelDriven<Customer>{

    @Resource(name = "customerImpl")
    private CustomerInterface customerImpl;

    /**
     * 保存客户
     * @return
     */
    public String add() {
        customerImpl.saveCustomer(customer);
        return NONE;
    }

    /**
     * 封装Model
     */
    private Customer customer = new Customer();
    @Override
    public Customer getModel() {
        return customer;
    }
}
```
* 测试Spring
	* 配置文件`applicationContext.xml`中开启注解扫描和自动代理
```xml
	<!--开启注解-->
	<context:component-scan base-package="com.coppco"/>

	<!--自动代理-->
	<aop:aspectj-autoproxy />
```
	* 编写CustomerServiceInterface接口和实现类CustomerServiceImpl, 并为CustomererviceImpl添加注解
```java
package com.coppco.service;

import com.coppco.domain.Customer;
import org.springframework.stereotype.Component;

@Component(value = "customerImpl")
public class CustomerImpl implements CustomerInterface {

    @Override
    public void saveCustomer(Customer customer) {
        System.out.println(customer);
    }
}
```
	* Struts2和Spring的整合
		* 方式1: Action类由Struts管理, 在Action类中注入由Srping管理的Service
			* 注入方式有注解注入、根据name自动注入(需要导入`struts2-spring-plugin-2.3.24.jar`, 设置成员属性和setter方法, 属性名要和该类的id值一样)
		* 方式2: Action类也由Spring管理(常用)
			* struts.xml中需要将全路径换成id值
```xml
<action name="customer_*" class="com.coppco.web.action.CustomerAction" method="{1}"></action>
```
			* Spring默认是单例, 所以需要设置为多例模式.
* Spring整合Hibernate
	* 带有`hibernate.cfg.xml`的配置文件
		* 在`hibernate.cfg.xml`引入映射的配置文件
		* 在applicationContext.xml的配置文件，配置加载hibernate.cfg.xml的配置
```xml
<bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
			<property name="configLocation" value="classpath:hibernate.cfg.xml"/>
		</bean>
```
		* Dao继承于HibernateDaoSupport, 该类有一个HibernateTemplate, 注入sessionFactory即可, 使用注解可以在初始化时执行setSessionFactory()方法.
```java
package com.coppco.dao;

import com.coppco.domain.Customer;
import org.hibernate.SessionFactory;
import org.springframework.orm.hibernate5.support.HibernateDaoSupport;
import org.springframework.stereotype.Component;

import javax.annotation.PostConstruct;
import javax.annotation.Resource;

@Component(value = "customerDao")
public class CustomerDaoImpl extends HibernateDaoSupport implements CustomerDao{


    @Resource(name = "sessionFactory")
    private SessionFactory sessionFactory;

    @PostConstruct
    private void initialize() {
        setSessionFactory(sessionFactory);
    }

    @Override
    public void save(Customer customer) {
        getHibernateTemplate().saveOrUpdate(customer);
    }
}
```
		* 事务开启,在`applicationContext.xml`中配置.注意Hibernate需要使用HibernateTransactionManager, 最后在Service类中添加事务注解`@Transactional`
```xml
	<!--事务管理器-->
	<bean id="transactionManager" class="org.springframework.orm.hibernate5.HibernateTransactionManager">
		<property name="sessionFactory" ref="sessionFactory"/>
	</bean>
	<!--事务注解-->
	<tx:annotation-driven transaction-manager="transactionManager"/>
```
	* 没有`hibernate.cfg.xml`配置文件(推荐)
		* 首先在`applicationContext.xml`中配置连接池
```xml
	<bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
		<property name="driverClass" value="com.mysql.jdbc.Driver"/>
		<property name="jdbcUrl" value="jdbc:mysql://192.168.1.191:3306/day38"/>
		<property name="user" value="root"/>
		<property name="password" value="123456"/>
	</bean>
```
		* 配置LocalSessionFactoryBean, 不能再加载配置文件了, 需要注入
```xml
<bean id="sessionFactory" class="org.springframework.orm.hibernate5.LocalSessionFactoryBean">
		<!--加载连接池-->
		<property name="dataSource" ref="dataSource"/>
		<!--配置其他属性-->
		<property name="hibernateProperties">
			<props>
				<prop key="hibernate.show_sql">true</prop>
				<prop key="hibernate.dialect">org.hibernate.dialect.MySQLDialect</prop>
				<prop key="hibernate.format_sql">true</prop>
				<prop key="hibernate.hbm2ddl.auto">update</prop>
			</props>
		</property>
		<!--配置映射文件-->
		<property name="mappingResources">
			<list>
				<value>com/coppco/domain/Customer.hbm.xml</value>
			</list>
		</property>
	</bean>
```
		* 事务开启,在`applicationContext.xml`中配置.注意Hibernate需要使用HibernateTransactionManager, 最后在Service类中添加事务注解`@Transactional`
```xml
	<!--事务管理器-->
	<bean id="transactionManager" class="org.springframework.orm.hibernate5.HibernateTransactionManager">
		<property name="sessionFactory" ref="sessionFactory"/>
	</bean>
	<!--事务注解-->
	<tx:annotation-driven transaction-manager="transactionManager"/>
```
## <font color=orange>延迟加载问题</font>
使用延迟加载的时候，再WEB层查询对象的时候程序会抛出异常！
* 首先需要查看`javassist.jar`是否重复, 重复删除一个
* Spring框架提供了一个过滤器，让session对象在WEB层就创建，在WEB层销毁.注意的是: 需要在`web.xml`中struts2的核心过滤器之前进行配置.
```xml
<filter>
    <filter-name>OpenSessionInViewFilter</filter-name>
    <filter-class>org.springframework.orm.hibernate5.support.OpenSessionInViewFilter</filter-class>
</filter>
<filter-mapping>
    <filter-name>OpenSessionInViewFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```

# <font color=orange>FastJson的使用</font>
* 导入fastjson的开发jar包fastjson-1.2.8.jar
	* String s = JSON.toJSONString(集合/对象)
		
	* 如果List集合中存入相同引用的对象
		* fastjson默认的情况下是进行循环检测的，去除掉死循环调用的方式
		* 可以使用JSON.toJSONString(p,SerializerFeature.DisableCircularReferenceDetect) 去除循环检测，但是就会出现死循环的效果
		* 最后可以使用注解：@JSONField(serialize=false)对指定的属性不转换成json


# <font color=orange>获取泛型的类型</font>
实际开发中, 我们会抽取公共的Dao接口(主要是一些增删改查的方法, 使用泛型)和它的实现类, 而在查询的时候需要获取泛型的类型.这里提供一个方式获取到泛型的类型.

```java
private Class clazz;

public BaseDaoImpl() {
    Type genType = getClass().getGenericSuperclass();
    if (genType instanceof ParameterizedType) {
        Type[] params = ((ParameterizedType) genType).getActualTypeArguments();
        clazz = (Class) params[0];
    }
}
```
