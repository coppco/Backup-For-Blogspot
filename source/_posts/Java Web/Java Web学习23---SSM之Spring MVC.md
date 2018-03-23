---
layout: post
title: Java Web学习23---SSM之Spring MVC
comments: true
toc: true
date: 2017-01-03 09:11:23
tags:
	- Java
	- SSM
	- Spring MVC
---

SSM（Spring，Spring MVC，MyBatis）其中Spring是一个轻量级的控制反转（IoC）和面向切面（AOP）的容器框架。SpringMVC分离了控制器、模型对象、分派器以及处理程序对象的角色，这种分离让它们更容易进行定制。MyBatis是一个支持普通SQL查询，存储过程和高级映射的优秀持久层框架。

<!--more-->

## <font color=orange>Spring MVC与Struts2的不同</font>
* springmvc的入口是一个servlet即前端控制器，而struts2入口是一个filter过虑器。
* springmvc是基于方法开发(一个url对应一个方法)，请求参数传递到方法的形参，可以设计为单例或多例(建议单例)，struts2是基于类开发，传递参数是通过类的属性，只能设计为多例。
* Struts采用值栈存储请求和响应的数据，通过OGNL存取数据， springmvc通过参数解析器是将request请求内容解析，并给方法形参赋值，将数据和视图封装成ModelAndView对象，最后又将ModelAndView中的模型数据通过reques域传输到页面。Jsp视图解析器默认使用jstl。
## <font color=orange> Spring MVC </font>
* Spring MVC是Spring提供的一个强大而灵活的web框架。
* Spring MVC主要由DispatcherServlet、处理器映射、处理器(控制器)、视图解析器、视图组成。他的两个核心是：
	* 处理器映射：选择使用哪个控制器来处理请求 
	* 视图解析器：选择结果应该如何渲染

### <font color=green> Spring MVC的流程 </font>
* 一、Http请求：客户端请求提交到DispatcherServlet。 
* 二、寻找处理器：由DispatcherServlet控制器查询一个或多个HandlerMapping，找到处理请求的Controller。 
* 三、调用处理器：DispatcherServlet将请求提交到Controller。 
* 四、调用业务处理和返回结果：Controller调用业务逻辑处理后，返回ModelAndView。 
* 五、处理视图映射并返回模型： DispatcherServlet查询一个或多个ViewResoler视图解析器，找到ModelAndView指定的视图。 
* 六、Http响应：视图负责将结果显示到客户端。

### <font color=green> Spring MVC的简单实用 </font>
* 一、导入SpringMVC相关的jar包
* 二、配置SpringMVC核心配置文件
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xmlns:mvc="http://www.springframework.org/schema/mvc"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/mvc
        http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
        http://code.alibabatech.com/schema/dubbo
        http://code.alibabatech.com/schema/dubbo/dubbo.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.0.xsd">

    <!--注解扫描-->
    <context:component-scan base-package="com.coppco.controller"/>

    <!--如果没有配置处理器映射和处理器适配器, 那么每次请求SpringMVC会去默认的DispatcherServlet.properties中查找  -->
    <!--Spring3.1之前的版本-->
    <!--注解形式的处理器映射器-->
    <!--<bean class="org.springframework.web.servlet.mvc.annotation.DefaultAnnotationHandlerMapping"/>
    <!--注解形式的处理器适配器-->
    <!--<bean class="org.springframework.web.servlet.mvc.annotation.AnnotationMethodHandlerAdapter"/>-->

    <!--Spring3.1之后的版本-->
    <!--注解形式的处理器映射器-->
    <!--<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerMapping"/>-->
    <!--注解形式的处理器适配器-->
    <!--<bean class="org.springframework.web.servlet.mvc.method.annotation.RequestMappingHandlerAdapter"/>-->
    
    <!--注解驱动(通常使用这个即可代替上面的写法): 会自动配置最新版的注解的处理器映射器和处理器适配器-->
    <mvc:annotation-driven/>

    <!--配置视图解析器: 可以配置也可以不配置, 配置了以后只写去掉前缀和后缀的页面名称即可-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/"/>
        <!--后缀-->
        <property name="suffix" value=".html"/>
    </bean>
</beans>
```
	* SpringMVC支持注解的方式, 需要开启注解扫描
	* Spring3.1之前版本和后面的版本, 注解方式的处理器映射器和适配器不一样
* 三、在`web.xml`中引入Spring MVC的核心Filter
```xml
<servlet>
    <servlet-name>springMVC</servlet-name>
    <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
    
    <!--如果没有指定SpringMVC的核心配置文件, 会默认查找WEB-INF/<servlet-name>中的内容-servlet.xml配置文件--
    <init-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath*:springMVC.xml</param-value>
    </init-param>
        
    <!--tomcat启动时就加载这个servlet-->
    <load-on-startup>1</load-on-startup>
</servlet>
  
<servlet-mapping>
    <servlet-name>springMVC</servlet-name>
    <!--可以设置
        /         拦截所有(除了jsp)
        /*        拦截所有(包含jsp)
        *.action  拦截.action结尾
    -->
    <url-pattern>*.action</url-pattern>
</servlet-mapping>
```
 * SpringMVC的url-pattern设置值有
 	* `/`: 拦截所有(除了jsp)
 	* `/*`: 拦截所有(包含jsp)
 	* `*.action`: 拦截.action结尾
* 四、Controller类(SpringMVC可以使用注解开发)
```java

@Controller
public class ListController {

    @RequestMapping("/list") //配置访问路径
    public ModelAndView itemList() throws Exception {
        //创建modelandView对象
        ModelAndView modelAndView = new ModelAndView();
        //添加model
        //modelAndView.addObject("itemList", itemList);
        //添加视图
        modelAndView.setViewName("/WEB-INF/jsp/itemList.html");
        return modelAndView;
    }
}
```
	* 需要使用`@Controller`标识此类是一个控制器.
	* 需要使用`@RequestMapping("/list")`来指定Handler方法所对应的url.
* 五、测试
当我们访问`http://xxx.com/list.action即可访问itemList.html页面`

## <font color=orange> SSM整合 </font>

### <font color=orange> 一、导入相关jar包(Maven项目) </font>
Maven项目中`pom.xml`文件内容: 
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>com.coppco</groupId>
  <artifactId>SSM0115</artifactId>
  <packaging>war</packaging>
  <version>1.0</version>
  <name>SSM0115 Maven Webapp</name>
  <url>http://maven.apache.org</url>

  <properties>
    <spring.version>4.2.4.RELEASE</spring.version>
    <mybatis.version>3.4.5</mybatis.version>
    <junit.version>4.9</junit.version>
    <mybatis-spring.version>1.3.1</mybatis-spring.version>
    <mysql.version>5.1.6</mysql.version>
    <log4j.version>1.2.17</log4j.version>
    <c3p0.version>0.9.5.2</c3p0.version>
    <jstl.version>1.2</jstl.version>
    <aspectj.version>1.5.4</aspectj.version>
  </properties>


  <dependencies>
    <!--jsp-->
    <dependency>
      <groupId>jstl</groupId>
      <artifactId>jstl</artifactId>
      <version>${jstl.version}</version>
    </dependency>
    <dependency>
      <groupId>taglibs</groupId>
      <artifactId>standard</artifactId>
      <version>1.1.2</version>
    </dependency>

    <dependency>
      <groupId>aspectj</groupId>
      <artifactId>aspectjweaver</artifactId>
      <version>${aspectj.version}</version>
    </dependency>

    <!--MyBatis整合Spring插件-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis-spring</artifactId>
      <version>${mybatis-spring.version}</version>
    </dependency>

    <!--mysql-->
    <dependency>
      <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
      <version>${mysql.version}</version>
    </dependency>

    <!--数据库连接池-->
    <dependency>
      <groupId>com.mchange</groupId>
      <artifactId>c3p0</artifactId>
      <version>${c3p0.version}</version>
    </dependency>

    <!--junit-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>${junit.version}</version>
      <scope>test</scope>
    </dependency>

    <!--log4j-->
    <dependency>
      <groupId>log4j</groupId>
      <artifactId>log4j</artifactId>
      <version>${log4j.version}</version>
    </dependency>

    <!--mybatis-->
    <dependency>
      <groupId>org.mybatis</groupId>
      <artifactId>mybatis</artifactId>
      <version>${mybatis.version}</version>
    </dependency>

    <!--spring-->
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-aop</artifactId>
      <version>${spring.version}</version>
    </dependency>
    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-test</artifactId>
      <version>${spring.version}</version>
      <scope>test</scope>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-context-support</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-web</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-webmvc</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-orm</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-beans</artifactId>
      <version>${spring.version}</version>
    </dependency>

    <dependency>
      <groupId>org.springframework</groupId>
      <artifactId>spring-core</artifactId>
      <version>${spring.version}</version>
    </dependency>
  </dependencies>

  <build>
    <finalName>SSM0115</finalName>
    <!--解决Maven项目, 默认不会加载java目录下的xml文件-->
    <resources>
      <resource>
        <directory>src/main/resources</directory>
        <includes>
          <include>**/*.properties</include>
          <include>**/*.xml</include>
          <include>**/*.tld</include>
        </includes>
        <filtering>false</filtering>
        <!--这里是false，用true会报 数据库连接 错误-->
      </resource>
      <resource>
        <directory>src/main/java</directory>
        <includes>
          <include>**/*.properties</include>
          <include>**/*.xml</include>
          <include>**/*.tld</include>
        </includes>
        <filtering>false</filtering>
      </resource>
    </resources>
  </build>
</project>
```

### <font color=orange> 二、相关配置文件 </font>

#### <font color=green> log4j配置文件: `log4j.properties` </font>
```xml
# Set root category priority to INFO and its only appender to CONSOLE.
#log4j.rootCategory=INFO, CONSOLE            debug   info   warn error fatal
log4j.rootCategory=error, CONSOLE, LOGFILE

# Set the enterprise logger category to FATAL and its only appender to CONSOLE.
log4j.logger.org.apache.axis.enterprise=FATAL, CONSOLE

# CONSOLE is set to be a ConsoleAppender using a PatternLayout.
log4j.appender.CONSOLE=org.apache.log4j.ConsoleAppender
log4j.appender.CONSOLE.layout=org.apache.log4j.PatternLayout
log4j.appender.CONSOLE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n

# LOGFILE is set to be a File appender using a PatternLayout.
log4j.appender.LOGFILE=org.apache.log4j.FileAppender
log4j.appender.LOGFILE.File=xxx.log
log4j.appender.LOGFILE.Append=true
log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
log4j.appender.LOGFILE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n

```

#### <font color=green> 数据库配置文件: `db.properties` </font>
```xml
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/ssm?characterEncoding=utf-8
jdbc.username=root
jdbc.password=123456
```

#### <font color=green> MyBatis配置文件: `SqlMapConfig.xml` </font>
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--别名-->
    <typeAliases>
        <package name="com.coppco.pojo"/>
    </typeAliases>
    
    <!--<mappers>-->
        <!--<mapper resource="User.xml"/>-->
        <!--<package name="com.coppco.dao"/>-->
    <!--</mappers>-->
</configuration>
```
#### <font color=green> SpringMVC配置文件: `springmvc.xml ` </font>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
        xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
        xmlns:p="http://www.springframework.org/schema/p"
        xmlns:context="http://www.springframework.org/schema/context"
        xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
        xmlns:mvc="http://www.springframework.org/schema/mvc"
        xsi:schemaLocation="http://www.springframework.org/schema/beans
         http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
         http://www.springframework.org/schema/mvc
         http://www.springframework.org/schema/mvc/spring-mvc-4.0.xsd
         http://code.alibabatech.com/schema/dubbo
         http://code.alibabatech.com/schema/dubbo/dubbo.xsd
         http://www.springframework.org/schema/context
         http://www.springframework.org/schema/context/spring-context-4.0.xsd">

    <!--注解扫描-->
    <context:component-scan base-package="com.coppco.controller"/>

    <!--注解驱动: 会自动配置最新版的注解的处理器映射器和处理器适配器-->
    <mvc:annotation-driven/>

    <!--配置视图解析器: 可以配置也可以不配置-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>
</beans>
```

#### <font color=green> Spring-dao配置文件: `applicationContext-dao.xml ` </font>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.0.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-4.0.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
        http://www.springframework.org/schema/util
        http://www.springframework.org/schema/util/spring-util-4.0.xsd">


    <!--加载配置文件-->
    <context:property-placeholder location="classpath*:db.properties"/>

    <!--数据库连接池-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>

    <!--mybatis会话工厂: 使用整合包中的类-->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--数据源-->
        <property name="dataSource" ref="dataSource"/>
        <!--配置文件-->
        <property name="configLocation" value="classpath:SqlMapConfig.xml"/>
        <!--映射文件-->
        <!--<property name="mapperLocations" value="classpath:com/coppco/**/*.xml" />-->
    </bean>

    <!--使用包扫描的方式配置Mapper, 引用的时候可以使用类名(首字母小写)-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--多个包, 使用英文下的逗号隔开, -->
        <property name="basePackage" value="com.coppco.dao"/>
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
    </bean>
</beans>
```
#### <font color=green> Spring-service配置文件: `applicationContext-service.xml ` </font>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
       http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.0.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-4.0.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
        http://www.springframework.org/schema/util
        http://www.springframework.org/schema/util/spring-util-4.0.xsd">


    <!--@Service扫描-->
    <context:component-scan base-package="com.coppco.service"/>

</beans>
```

#### <font color=green> Spring-trans配置文件: `applicationContext-trans.xml ` </font>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        http://www.springframework.org/schema/beans/spring-beans-4.0.xsd
        http://www.springframework.org/schema/context
        http://www.springframework.org/schema/context/spring-context-4.0.xsd
        http://www.springframework.org/schema/aop
        http://www.springframework.org/schema/aop/spring-aop-4.0.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx-4.0.xsd
        http://www.springframework.org/schema/util
        http://www.springframework.org/schema/util/spring-util-4.0.xsd">


    <!--事务-->
    <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

    <!-- 事务扫描开始(开启@Tranctional) -->
    <tx:annotation-driven transaction-manager="transactionManager" />

    <!--通知-->
    <tx:advice id="txAdvice" transaction-manager="transactionManager">
        <tx:attributes>
            <tx:method name="save*" propagation="REQUIRED"/>
            <tx:method name="insert*" propagation="REQUIRED"/>
            <tx:method name="delete*" propagation="REQUIRED"/>
            <tx:method name="update*" propagation="REQUIRED"/>
            <tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
            <tx:method name="get*" propagation="SUPPORTS" read-only="true"/>
        </tx:attributes>
    </tx:advice>

    <!--切面-->
    <aop:config>
        <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.coppco.service.*.*(..))"/>
    </aop:config>
</beans>
```
#### <font color=green> Web项目配置文件: `web.xml` </font>
```xml
<!DOCTYPE web-app PUBLIC
        "-//Sun Microsystems, Inc.//DTD Web Application 2.3//EN"
        "http://java.sun.com/dtd/web-app_2_3.dtd" >

<web-app>
    <display-name>Archetype Created Web Application</display-name>

    <!--加载spring容器-->
    <context-param>
        <param-name>contextConfigLocation</param-name>
        <param-value>classpath*:applicationContext-*.xml</param-value>
    </context-param>
    <listener>
        <listener-class>org.springframework.web.context.ContextLoaderListener</listener-class>
    </listener>
    
    <!--springmvc前的控制器-->
    <servlet>
        <servlet-name>springMvc</servlet-name>
        <servlet-class>org.springframework.web.servlet.DispatcherServlet</servlet-class>
        <init-param>
            <param-name>contextConfigLocation</param-name>
            <param-value>classpath*:springmvc.xml</param-value>
        </init-param>
        <!--在Tomcat启动时就加载-->
        <load-on-startup>1</load-on-startup>
    </servlet>
    <servlet-mapping>
        <servlet-name>springMvc</servlet-name>
        <url-pattern>*.action</url-pattern>
    </servlet-mapping>

</web-app>
```

### <font color=orange> 三、Mapper接口、Sql映射文件和Pojo类 </font>

单表的Mapper接口、Sql映射文件和Pojo类可以通过 MyBatis的逆向工程自动生成, 然后导入到项目中.
#### <font color=green> Pojo类: `Items.java` </font>
```java
package com.coppco.pojo;

import java.util.Date;

public class Items {
    private Integer id;

    private String name;

    private Float price;

    private String pic;

    private Date createtime;

    private String detail;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name == null ? null : name.trim();
    }

    public Float getPrice() {
        return price;
    }

    public void setPrice(Float price) {
        this.price = price;
    }

    public String getPic() {
        return pic;
    }

    public void setPic(String pic) {
        this.pic = pic == null ? null : pic.trim();
    }

    public Date getCreatetime() {
        return createtime;
    }

    public void setCreatetime(Date createtime) {
        this.createtime = createtime;
    }

    public String getDetail() {
        return detail;
    }

    public void setDetail(String detail) {
        this.detail = detail == null ? null : detail.trim();
    }
}
```
#### <font color=green> Mapper接口: `ItemsMapper.java` </font>

```java
package com.coppco.dao;

import java.util.List;
import com.coppco.pojo.Items;
import com.coppco.pojo.ItemsExample;
import org.apache.ibatis.annotations.Param;


public interface ItemsMapper {
    int countByExample(ItemsExample example);

    int deleteByExample(ItemsExample example);

    int deleteByPrimaryKey(Integer id);

    int insert(Items record);

    int insertSelective(Items record);

    List<Items> selectByExampleWithBLOBs(ItemsExample example);

    List<Items> selectByExample(ItemsExample example);

    Items selectByPrimaryKey(Integer id);

    int updateByExampleSelective(@Param("record") Items record, @Param("example") ItemsExample example);

    int updateByExampleWithBLOBs(@Param("record") Items record, @Param("example") ItemsExample example);

    int updateByExample(@Param("record") Items record, @Param("example") ItemsExample example);

    int updateByPrimaryKeySelective(Items record);

    int updateByPrimaryKeyWithBLOBs(Items record);

    int updateByPrimaryKey(Items record);
}
```
#### <font color=green> Mapper映射文件: `ItemsMapper.xml` </font>
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd" >
<mapper namespace="com.coppco.dao.ItemsMapper" >
  <resultMap id="BaseResultMap" type="com.coppco.pojo.Items" >
    <id column="id" property="id" jdbcType="INTEGER" />
    <result column="name" property="name" jdbcType="VARCHAR" />
    <result column="price" property="price" jdbcType="REAL" />
    <result column="pic" property="pic" jdbcType="VARCHAR" />
    <result column="createtime" property="createtime" jdbcType="TIMESTAMP" />
  </resultMap>
  <resultMap id="ResultMapWithBLOBs" type="com.coppco.pojo.Items" extends="BaseResultMap" >
    <result column="detail" property="detail" jdbcType="LONGVARCHAR" />
  </resultMap>
  <sql id="Example_Where_Clause" >
    <where >
      <foreach collection="oredCriteria" item="criteria" separator="or" >
        <if test="criteria.valid" >
          <trim prefix="(" suffix=")" prefixOverrides="and" >
            <foreach collection="criteria.criteria" item="criterion" >
              <choose >
                <when test="criterion.noValue" >
                  and ${criterion.condition}
                </when>
                <when test="criterion.singleValue" >
                  and ${criterion.condition} #{criterion.value}
                </when>
                <when test="criterion.betweenValue" >
                  and ${criterion.condition} #{criterion.value} and #{criterion.secondValue}
                </when>
                <when test="criterion.listValue" >
                  and ${criterion.condition}
                  <foreach collection="criterion.value" item="listItem" open="(" close=")" separator="," >
                    #{listItem}
                  </foreach>
                </when>
              </choose>
            </foreach>
          </trim>
        </if>
      </foreach>
    </where>
  </sql>
  <sql id="Update_By_Example_Where_Clause" >
    <where >
      <foreach collection="example.oredCriteria" item="criteria" separator="or" >
        <if test="criteria.valid" >
          <trim prefix="(" suffix=")" prefixOverrides="and" >
            <foreach collection="criteria.criteria" item="criterion" >
              <choose >
                <when test="criterion.noValue" >
                  and ${criterion.condition}
                </when>
                <when test="criterion.singleValue" >
                  and ${criterion.condition} #{criterion.value}
                </when>
                <when test="criterion.betweenValue" >
                  and ${criterion.condition} #{criterion.value} and #{criterion.secondValue}
                </when>
                <when test="criterion.listValue" >
                  and ${criterion.condition}
                  <foreach collection="criterion.value" item="listItem" open="(" close=")" separator="," >
                    #{listItem}
                  </foreach>
                </when>
              </choose>
            </foreach>
          </trim>
        </if>
      </foreach>
    </where>
  </sql>
  <sql id="Base_Column_List" >
    id, name, price, pic, createtime
  </sql>
  <sql id="Blob_Column_List" >
    detail
  </sql>
  <select id="selectByExampleWithBLOBs" resultMap="ResultMapWithBLOBs" parameterType="com.coppco.pojo.ItemsExample" >
    select
    <if test="distinct" >
      distinct
    </if>
    <include refid="Base_Column_List" />
    ,
    <include refid="Blob_Column_List" />
    from items
    <if test="_parameter != null" >
      <include refid="Example_Where_Clause" />
    </if>
    <if test="orderByClause != null" >
      order by ${orderByClause}
    </if>
  </select>
  <select id="selectByExample" resultMap="BaseResultMap" parameterType="com.coppco.pojo.ItemsExample" >
    select
    <if test="distinct" >
      distinct
    </if>
    <include refid="Base_Column_List" />
    from items
    <if test="_parameter != null" >
      <include refid="Example_Where_Clause" />
    </if>
    <if test="orderByClause != null" >
      order by ${orderByClause}
    </if>
  </select>
  <select id="selectByPrimaryKey" resultMap="ResultMapWithBLOBs" parameterType="java.lang.Integer" >
    select 
    <include refid="Base_Column_List" />
    ,
    <include refid="Blob_Column_List" />
    from items
    where id = #{id,jdbcType=INTEGER}
  </select>
  <delete id="deleteByPrimaryKey" parameterType="java.lang.Integer" >
    delete from items
    where id = #{id,jdbcType=INTEGER}
  </delete>
  <delete id="deleteByExample" parameterType="com.coppco.pojo.ItemsExample" >
    delete from items
    <if test="_parameter != null" >
      <include refid="Example_Where_Clause" />
    </if>
  </delete>
  <insert id="insert" parameterType="com.coppco.pojo.Items" >
    insert into items (id, name, price, 
      pic, createtime, detail
      )
    values (#{id,jdbcType=INTEGER}, #{name,jdbcType=VARCHAR}, #{price,jdbcType=REAL}, 
      #{pic,jdbcType=VARCHAR}, #{createtime,jdbcType=TIMESTAMP}, #{detail,jdbcType=LONGVARCHAR}
      )
  </insert>
  <insert id="insertSelective" parameterType="com.coppco.pojo.Items" >
    insert into items
    <trim prefix="(" suffix=")" suffixOverrides="," >
      <if test="id != null" >
        id,
      </if>
      <if test="name != null" >
        name,
      </if>
      <if test="price != null" >
        price,
      </if>
      <if test="pic != null" >
        pic,
      </if>
      <if test="createtime != null" >
        createtime,
      </if>
      <if test="detail != null" >
        detail,
      </if>
    </trim>
    <trim prefix="values (" suffix=")" suffixOverrides="," >
      <if test="id != null" >
        #{id,jdbcType=INTEGER},
      </if>
      <if test="name != null" >
        #{name,jdbcType=VARCHAR},
      </if>
      <if test="price != null" >
        #{price,jdbcType=REAL},
      </if>
      <if test="pic != null" >
        #{pic,jdbcType=VARCHAR},
      </if>
      <if test="createtime != null" >
        #{createtime,jdbcType=TIMESTAMP},
      </if>
      <if test="detail != null" >
        #{detail,jdbcType=LONGVARCHAR},
      </if>
    </trim>
  </insert>
  <select id="countByExample" parameterType="com.coppco.pojo.ItemsExample" resultType="java.lang.Integer" >
    select count(*) from items
    <if test="_parameter != null" >
      <include refid="Example_Where_Clause" />
    </if>
  </select>
  <update id="updateByExampleSelective" parameterType="map" >
    update items
    <set >
      <if test="record.id != null" >
        id = #{record.id,jdbcType=INTEGER},
      </if>
      <if test="record.name != null" >
        name = #{record.name,jdbcType=VARCHAR},
      </if>
      <if test="record.price != null" >
        price = #{record.price,jdbcType=REAL},
      </if>
      <if test="record.pic != null" >
        pic = #{record.pic,jdbcType=VARCHAR},
      </if>
      <if test="record.createtime != null" >
        createtime = #{record.createtime,jdbcType=TIMESTAMP},
      </if>
      <if test="record.detail != null" >
        detail = #{record.detail,jdbcType=LONGVARCHAR},
      </if>
    </set>
    <if test="_parameter != null" >
      <include refid="Update_By_Example_Where_Clause" />
    </if>
  </update>
  <update id="updateByExampleWithBLOBs" parameterType="map" >
    update items
    set id = #{record.id,jdbcType=INTEGER},
      name = #{record.name,jdbcType=VARCHAR},
      price = #{record.price,jdbcType=REAL},
      pic = #{record.pic,jdbcType=VARCHAR},
      createtime = #{record.createtime,jdbcType=TIMESTAMP},
      detail = #{record.detail,jdbcType=LONGVARCHAR}
    <if test="_parameter != null" >
      <include refid="Update_By_Example_Where_Clause" />
    </if>
  </update>
  <update id="updateByExample" parameterType="map" >
    update items
    set id = #{record.id,jdbcType=INTEGER},
      name = #{record.name,jdbcType=VARCHAR},
      price = #{record.price,jdbcType=REAL},
      pic = #{record.pic,jdbcType=VARCHAR},
      createtime = #{record.createtime,jdbcType=TIMESTAMP}
    <if test="_parameter != null" >
      <include refid="Update_By_Example_Where_Clause" />
    </if>
  </update>
  <update id="updateByPrimaryKeySelective" parameterType="com.coppco.pojo.Items" >
    update items
    <set >
      <if test="name != null" >
        name = #{name,jdbcType=VARCHAR},
      </if>
      <if test="price != null" >
        price = #{price,jdbcType=REAL},
      </if>
      <if test="pic != null" >
        pic = #{pic,jdbcType=VARCHAR},
      </if>
      <if test="createtime != null" >
        createtime = #{createtime,jdbcType=TIMESTAMP},
      </if>
      <if test="detail != null" >
        detail = #{detail,jdbcType=LONGVARCHAR},
      </if>
    </set>
    where id = #{id,jdbcType=INTEGER}
  </update>
  <update id="updateByPrimaryKeyWithBLOBs" parameterType="com.coppco.pojo.Items" >
    update items
    set name = #{name,jdbcType=VARCHAR},
      price = #{price,jdbcType=REAL},
      pic = #{pic,jdbcType=VARCHAR},
      createtime = #{createtime,jdbcType=TIMESTAMP},
      detail = #{detail,jdbcType=LONGVARCHAR}
    where id = #{id,jdbcType=INTEGER}
  </update>
  <update id="updateByPrimaryKey" parameterType="com.coppco.pojo.Items" >
    update items
    set name = #{name,jdbcType=VARCHAR},
      price = #{price,jdbcType=REAL},
      pic = #{pic,jdbcType=VARCHAR},
      createtime = #{createtime,jdbcType=TIMESTAMP}
    where id = #{id,jdbcType=INTEGER}
  </update>
</mapper>
```
### <font color=orange>四、Service接口、Service实现类以及Controller类</font> ###
#### <font color=green> Service接口 </font>
```java
package com.coppco.service;

import com.coppco.pojo.Items;

import java.util.List;

public interface ItemService {
    List<Items> list();
}

```

#### <font color=green> Service实现类 </font>
```java
package com.coppco.service;

import com.coppco.dao.ItemsMapper;
import com.coppco.pojo.Items;
import com.coppco.pojo.ItemsExample;
import org.springframework.stereotype.Service;
import javax.annotation.Resource;
import java.util.List;

@Service(value = "itemService")
public class ItemServiceImpl implements ItemService {

    @Resource(name = "itemsMapper")
    private ItemsMapper itemsMapper;

    public List<Items> list() {
        ItemsExample example = new ItemsExample();
        //包含大文本对象
        return itemsMapper.selectByExampleWithBLOBs(example);
    }
}
```

#### <font color=green> Controller类 </font>
```java
package com.coppco.controller;

import com.coppco.dao.ItemsMapper;
import com.coppco.pojo.Items;
import com.coppco.service.ItemService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.servlet.ModelAndView;

import javax.annotation.Resource;
import java.util.List;

@Controller
public class ItemController {

    @Resource(name = "itemService")
    private ItemService itemService;

    @RequestMapping("/list")
    public ModelAndView items() {
        List<Items> list = itemService.list();

        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("itemList", list);
        modelAndView.setViewName("itemList");
        return modelAndView;
    }
}
```

## <font color=orange> SpringMVC之参数绑定 </font>
### <font color=orange> 通过Request对象获取传递的参数 </font>
* 注解的方式
```java
@Controller
public class ItemController {

    @Autowired
    private  HttpServletRequest request;
}
```
* 通过配置监听
	* 在`web.xml`中配置
```xml
<listener>  
    <listener-class>  
            org.springframework.web.context.request.RequestContextListener  
    </listener-class>  
</listener>  
```
	* 获取Request
```java
HttpServletRequest request = ((ServletRequestAttributes)RequestContextHolder.getRequestAttributes()).getRequest(); 
//获取传递的参数
String id = request.getParameter("id"); 
```
* 在Controller中方法直接获取
```java
@Controller
public class ItemController {

    @RequestMapping("/hello")
    public String hello(HttpServletRequest request, HttpServletResponse response, HttpSession session, Model model) {
        //获取传递的参数
        String id = request.getParameter("id");
        //传递参数
        model.addAttribute("item", "123");
    }
}
```

### <font color=orange> 直接在方法中获取传递的参数(简单类型) </font>
简单参数类型, 名称必须和页面传递的名称一致,不一致可以通过`@RequestParam("xxx")`注解类重新命名
此方法会出现乱码问题, 需要设置编码问题. <font color=red>传递简单类型, 如果不传递会报错, 使用包装类不传递则为null</font>
```java
@Controller
public class ItemController {

    @RequestMapping("/updateitem")
    public String update(Integer id, String name, @RequestParam("price") Float price111, String detail) throws Exception {
        //code
    }
}
//传递的参数是price 转成 price, required=true, 表示参数必须传递, 否则报错, defaultValue默认值
@RequestParam(value="price", required=true, defaultValue="100") Float price11
```

### <font color=orange> 直接在方法中获取传递的Pojo对象 </font>
要求input框的name属性必须是Pojo的属性名.
```java
<input type="text" name="name" value="${item.name }" />

@Controller
public class ItemController {

    @RequestMapping("/updateitem")
    public String update(Items item) throws Exception {
        //code
    }
}
```

### <font color=orange> URL模板变量 </font>
@PathVariable用于将请求URL中的模板变量映射到功能处理方法的参数上。如果形参名称和@RequestMapping中占位符名称一样, 也可以省略@PathVariable中名称.
```java

@Controller
public class ItemController {

    @RequestMapping("/updateitem/{id}")
    public String update(@PathVariable("id") String id, Items item) throws Exception {
        //code
    }
}
```

### <font color=orange> 直接在方法中获取传递的Vo对象 </font>
要求input框的name属性必须是vo的属性名.属性名.
```java
<input type="text" name="items.name" />

public class QueryVo {

    //商品信息
    private Items items;

    //订单信息等

    public Items getItems() {
        return items;
    }

    public void setItems(Items items) {
        this.items = items;
    }
}

@Controller
public class ItemController {

    @RequestMapping("/updateitem")
    public String update(QueryVo query) throws Exception {
        //code
    }
}
```

### <font color=orange> 自定义转换器 </font>
有的类型无法转换成对象, 例如字符串转Date类型.
* 自定义转换器
```java
/**
 * S: source 源
 * T: target 目标
 */
public class GlobalStringToDateConverter implements Converter<String, Date> {
    public Date convert(String source) {

        try {
            Date date = new SimpleDateFormat("yyyy-MM-dd hh:mm:ss").parse(source);
            return date;
        } catch (ParseException e) {
            e.printStackTrace();
        }

        return null;
    }
}
```
* Spring配置文件`springmvc.xml`中配置自定义转换器
```xml
  <!--注解驱动: 会自动配置最新版的注解的处理器映射器和处理器适配器-->
    <mvc:annotation-driven conversion-service="formattingConversionService"/>

    <!--配置自定义转换器-->
    <bean id="formattingConversionService" class="org.springframework.format.support.FormattingConversionServiceFactoryBean">
        <property name="converters">
            <set>
                <!--指定自定义转换器路径-->
                <bean class="com.coppco.controller.converter.GlobalStringToDateConverter"/>
            </set>
        </property>
    </bean>
```

### <font color=orange> 数组和List </font>
可以通过Vo类来接受参数, 私有一个数组或者List属性, input的name是数组的名称或者list的名称[index].属性即可.

## <font color=orange> 解决SpringMVC编码问题 </font>

### <font color=green> Post请求 </font>

在`web.xml`中配置过滤器: 
```xml
<filter>
    <filter-name>CharacterEncodingFilter</filter-name>
    <filter-class>org.springframework.web.filter.CharacterEncodingFilter</filter-class>
    <init-param>
        <param-name>encoding</param-name>
        <param-value>utf-8</param-value>
    </init-param>
</filter>
<filter-mapping>
    <filter-name>CharacterEncodingFilter</filter-name>
    <url-pattern>/*</url-pattern>
</filter-mapping>
```
### <font color=green> Get请求 </font>
* 方式一: 修改tomcat配置文件添加编码与工程编码一致(ISO8859-1是tomcat默认编码)
```xml
<Connector URIEncoding="utf-8" connectionTimeout="20000" port="8080" protocol="HTTP/1.1" redirectPort="8443"/>
```
* 方式二: 对参数进行重新编码
```java
String userName = new 
String(request.getParamter("userName").getBytes("ISO8859-1"),"utf-8")
```

## <font color=orange> springMVC资源映射 </font>
我们配置拦截`/`, 拦截除了jsp的所有请求, 但是一些资源文件例如css和js会放在`src/main/resources`中也会拦截, 所以需要在springmvc的配置文件中配置`资源映射`:
```xml
<!--配置资源映射-->
<mvc:resources location="/css/" mapping="/css/**"/>
<mvc:resources location="/js/" mapping="/js/**"/>
```

## <font color=orange> @RequestMapper注解的使用 </font>

### <font color=orange> URL路径映射 </font>
@RequestMapping(value="/item")或@RequestMapping("/item）
value的值是数组，可以将多个url映射到同一个方法
多个URL以及限定POST请求: 
@RequestMapping(value = {"/search", "/list"}, method = RequestMethod.POST)

### <font color=orange> 窄化请求映射 </font>
在class上添加@RequestMapping(url)指定通用请求前缀， 限制此类下的所有方法请求url必须以请求前缀开头，通过此方法对url进行分类管理。

```java
@RequestMapping放在类名上边，设置请求前缀 
@Controller
@RequestMapping("/item")

方法名上边设置请求映射url：
@RequestMapping放在方法名上边，如下：
@RequestMapping("/queryItem ")
```
那么请求的路径就是: /item/queryItem

## <font color=orange> Controller的返回值 </font>
### <font color=orange> ModelAndView </font>
```java
@RequestMapping("/list")
public ModelAndView items() throws Exception{
    List<Items> list = itemService.list();
    ModelAndView modelAndView = new ModelAndView();
    //指定返回数据
    modelAndView.addObject("itemList", list);
    //指定返回的页面
    modelAndView.setViewName("itemList");
    return modelAndView;
}
```
### <font color=orange> 字符串(经常使用) </font>
```java
@RequestMapping("/list")
public String items(Model model) throws Exception{
    List<Items> list = itemService.list();
    //指定返回数据
    model.addAttribute("itemList", list);
    //返回的页面
    return "itemList";
}
```
### <font color=orange> void(破坏了SpringMVC结构,不建议使用) </font>
```java
@RequestMapping("/list")
public void items(HttpServletRequest request, HTTPServletResponse response) throws Exception{
    List<Items> list = itemService.list();
    //指定返回数据
    request.setAttribut("itemList", list);
    //指定返回页面, 需要完整的html路径
    request.getRequestDispatcher("xxxxxx").forward(request, response);
}
```

## <font color=orange> SpringMVC中的请求转发和重定向 </font>
### <font color=orange> 请求转发 </font>
* 浏览器地址栏不变, request域中数据可以带到下一个方法中
* SpringMVC中, 返回的页面使用`forward:+RequestMapping中的地址+后缀`, 如`forward:list.action`
* 如果是是当前路径, 使用相对路径, 如果不在一个路径下, 使用绝对路径(从项目名开始, 如: `forward:/items/list.action`)
### <font color=orange> 重定向 </font>
* 浏览器地址栏改变, request域中数据不可以带到下一个方法中
* SpringMVC中, 返回的页面使用`redirect:+RequestMapping中的地址+后缀`, 如`redirect:list.action`
* 如果是是当前路径, 使用相对路径, 如果不在一个路径下, 使用绝对路径(从项目名开始, 如: `redirect:/items/list.action`)

## <font color=orange> SpringMVC中异常处理 </font>
为了区别不同的异常通常根据异常类型自定义异常类，这里我们创建一个自定义系统异常，如果controller、service、dao抛出此类异常说明是系统预期处理的异常信息。

### <font color=orange> 自定义异常 </font>
```java
public class CustomerException extends Exception {
    private String message;

    public CustomerException(String message) {
        super(message);
        this.message = message;
    }

    public String getMessage() {
        return message;
    }

    public void setMessage(String message) {
        this.message = message;
    }
}
```
### <font color=orange> 自定义异常处理器 </font>
```java

public class CustomerExceptionResolver implements HandlerExceptionResolver {
    public ModelAndView resolveException(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) {

        e.printStackTrace();
        CustomerException customerException = null;

        //如果抛出的是系统自定义异常则转换
        if(e instanceof CustomerException){
            customerException = (CustomerException)e;
        }else{
            //如果抛出的不是系统自定义异常则重新构造一个系统错误异常。
            customerException = new CustomerException("请与系统管理员联系！");
        }

        ModelAndView modelAndView = new ModelAndView();
        modelAndView.addObject("message", customerException.getMessage());
        modelAndView.setViewName("error");

        return modelAndView;
    }
}
```
### <font color=orange> 在SrpingMVC配置文件中异常处理器配置 </font>
```xml
<!--异常处理器配置-->
<bean id="handlerExceptionResolver" class="com.coppco.exception.CustomerExceptionResolver"/>
```

## <font color=orange> SpringMVC中图片上传 </font>
### <font color=orange> 配置虚拟目录 </font>
在tomcat上配置图片虚拟目录，在tomcat下`conf/server.xml`中添加：
```xml
<Context docBase="F:\upload\temp" path="/pic" reloadable="false"/>
```
### <font color=orange> SpringMVC配置文件配置文件上传解析器 </font>
`CommonsMultipartResolver`解析器依赖`commons-fileupload`和`commons-io`
```xml
<!--配置文件上传解析器-->
<bean id="multipartResolver" class="org.springframework.web.multipart.commons.CommonsMultipartResolver">
    <property name="maxUploadSize">
        <!--配置文件最大大小-->
        <value>5242880</value>
    </property>
</bean>
```

### <font color=orange> 保存图片 </font>
使用`enctype="multipart/form-data"`提交图片, pictureFile为提交时图片的使用的name.
```java
@RequestMapping("/editItemSubmit")
public String editItemSubmit(Items items, MultipartFile pictureFile)throws Exception{
		
    //原始文件名称
    String pictureFile_name =  pictureFile.getOriginalFilename();
    //新文件名称
    String newFileName = UUID.randomUUID().toString()+pictureFile_name.substring(pictureFile_name.lastIndexOf("."));
		
    //上传图片
    File uploadPic = new java.io.File("F:/upload/temp/"+newFileName);
		
    if(!uploadPic.exists()){
        uploadPic.mkdirs();
    }
    //向磁盘写文件
    pictureFile.transferTo(uploadPic);
}
```

## <font color=orange> SpringMVC中的JSON数据交互 </font>
### <font color=orange> @RequestBody </font>
`@RequestBody`注解用于读取http请求的内容(字符串)，通过springmvc提供的HttpMessageConverter接口将读到的内容转换为json、xml等格式的数据并绑定到controller方法的参数上。
### <font color=orange> @ResponseBody </font>
`@ ResponseBody`注解用于将Controller的方法返回的对象，通过HttpMessageConverter接口转换为指定格式的数据如：json,xml等，通过Response响应给客户端。

```java
@RequestMapping(value = "/jsonString", consumes = "application/json")
public @ResponseBody Login json(@RequestBody Login login) {
    return login;
}
```

## <font color=orange> 拦截器 </font>
### <font color=orange> 自定义拦截器 </font>
```java
public class LoginInterceptor implements HandlerInterceptor {
    
 /** 
 * controller执行前调用此方法 
 * 返回true表示继续执行，返回false中止执行
 * 这里可以加入登录校验、权限拦截等
 */
    public boolean preHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o) throws Exception {
        return true;
    }

/**
 * controller执行后但未返回视图前调用此方法
 * 一般可以对数据进行处理
 */
    public void postHandle(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, ModelAndView modelAndView) throws Exception {

    }
/**
 * controller执行后且视图返回后调用此方法
 * 这里可得到执行controller时的异常信息
 * 这里可记录操作日志，资源清理等
 */
    public void afterCompletion(HttpServletRequest httpServletRequest, HttpServletResponse httpServletResponse, Object o, Exception e) throws Exception {

    }
}
```
### <font color=orange> 拦截器配置 </font>
多个拦截器顺序执行, mapping的path设置为`/**`时表示所有请求.

```xml
<!--拦截器 -->
<mvc:interceptors>
    <!--多个拦截器,顺序执行 -->
    <mvc:interceptor>
        <!--/**表示拦截所有请求-->
        <mvc:mapping path="/**"/>
        <bean class="com.coppco.interceptor.LoginInterceptor2"></bean>
    </mvc:interceptor>
    <mvc:interceptor>
        <mvc:mapping path="/**"/>
        <bean class="com.coppco.interceptor.LoginInterceptor2"></bean>
    </mvc:interceptor>
</mvc:interceptors>
```

## <font color=orange> Quartz实现定时任务调度 </font>
### <font color=orange> Quartz框架</font>

Quartz是一个开源你的作业调度框架, 它完全由Java开发, 它提供了巨大的灵活性而不牺牲简单性, 它可以创建一个简单的或复杂的调度.
* Job: 表示一个任务(工作), 要执行的具体内容
* JobDetail: 表示一个具体的可执行的调度程序, Job是这个可执行调度程序所执行的内容.
* Trigger: 表示一个调度参数的配置, 什么时候去调
* Scheduler: 表示一个调度容器, 一个调度容器可以注册多个JobDetail和Trigger.

### <font color=orange> 测试Quartz </font>

#### <font color=orange> 一、引入Quertz框架 </font>

```xml
<dependency>
    <groupId>org.quartz-scheduler</groupId>
    <artifactId>quartz</artifactId>
    <version>2.2.3</version>
</dependency>
```

#### <font color=orange> 二、编写Job类 </font>

```java
public class JobTest {
    public void execute() {
    	System.out.println("执行了调度");
    }
}
```
#### <font color=orange> 三、编写配置文件 </font>
* 创建spring配置文件`applicationContext-job.xml`

```xml
<!--定义任务类-->
<bean id="jobBean" class="com.coppco.job.JobTest" />

<!--任务描述-->
<bean id="jobDetail" class="org.springframework.scheduling.quartz.MethodInvokingJobDetailFactoryBean">
    <property name="targetObject" ref="jobBean" />
    <property name="targetMethod" ref="execute" />
</bean>
<!--触发器-->
<bean id="mailTrigger" class="org.springframework.scheduling.quartz.CronTriggerFactoryBean">
    <property name="jobDetail" ref="jobDetail" />
    <!--每10秒执行一次-->
    <property name="cronExpression" value="0/10 * * * * ?"/>
</bean>

<!--总管理容器-->
<bean id="startQuartz" class="org.springframework.scheduling.quartz.SchedulerFactoryBean">
    <property name="triggers">
        <list>
            <ref bean="mailTrigger" />
        </lsit>
    </property>
</bean>
```
* 并在主配置文件中引入
```xml
<import resource="classpath*:applicationContext-job.xml"/>
```
