---
layout: post
title: Spring Boot
comments: true
toc: true
date: 2017-01-20 10:43:59
tags:
	- Java
	- Spring
	- Spring Boot
---

<img src="http://oak4eha4y.bkt.clouddn.com/Spring_Boot_pic.jpg" alt="hello" style="width: 40%; text-align: center; display: block; margin-top:30px; margin-bottom:30px"/>

## <font color=orange>什么是Spring Boot</font>

随着动态语言的流行(Ruby、Groovy、Scala、Node.js、Python),Java开发显得格外笨重: 繁多的配置、低下的开发效率、复杂的部署流程.

Spring Boot应用而生, 它使用"习惯优于配置"的理念可以快速的搭建一个项目.使用Spring Boot很容易创建一个独立运行的(运行jar、内嵌Servlet容器)、基于Spring的项目.

<!--more-->

## <font color=orange>Spring Boot核心功能</font>

### <font color=orange>独立运行的Spring项目</font>
Spring Boot可以以jar包的形式独立运行, 运行一个Spring Boot项目只需通过`java -jar xxx.jar`来运行.
### <font color=orange>内嵌Servlet容器</font>
Spring Boot可以选择内嵌Tomcat、Jetty或者Undertow, 这样我们无需以war包的形式部署项目.
### <font color=orange>提供starter简化Maven配置</font>
Spring提供了一系列的starter pom来简化Maven的依赖加载, 例如, 当你使用了spring-boot-starter-web时会自动加载相关jar包.
### <font color=orange>自动配置Spring</font>
Spring Boot会根据在类路径中的jar包、类, 为jar包里面的类自动配置Bean, 我们也可以自定义自动配置.
### <font color=orange>准生成的应用监控</font>
Spring Boot提供基于http、ssh、telnet对运行时的项目进行监控.
### <font color=orange>无代码生成和xml配置</font>
Spring Boot可以不借助代码来实现, 可以通过条件注解来实现, 这是Spring 4.x提供的新特性.
Spring 4.x提倡使用Java配置和注解配置组合, 而Spring Boot不需要任何xml配置即可实现Spring的所有配置.

## <font color=orange>Spring Boot快速开始</font>
### <font color=orange>使用网站快速构建项目</font>
* 1、[快速构建网址](http://start.spring.io/)
	* 1.1、这里填写项目信息, 选择Maven项目, Java语言, Spring Boot的版本.
	* 1.2、选择项目所用的技术, 这里每一项技术都是Spring boot的starter pom.
	* 1.3、Generate Project下载项目

### <font color=orange>使用开发工具快速构建</font>
* Eclipse
	* 新建一个项目选择`Spring Starter Project`
	* 填写项目信息和使用的技术
* Intellij IDEA
	* 新建一个项目选择`Spring Initializr`
	* 填写项目信息和使用的技术

### <font color=orange>手工创建Spring Boot项目</font>
* 首先创建Maven项目
* 修改pom.xml, 添加Spring Boot的父级依赖
```xml
<parent>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-parent</artifactId>
    <version>2.0.2.RELEASE</version>
    <relativePath/> <!-- lookup parent from repository -->
</parent>
```
* 修改pom.xml, 在dependencies添加Web支持的`starter pom`
```
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-web</artifactId>
    </dependency>
</dependencies>
```
* 修改pom.xml, 添加Spring Boot的编译插件
```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
        </plugin>
    </plugins>
</build>
```

## <font color=orange>Spring Boot CLI</font>
Spring Boot CLI是Spring Boot提供的控制台命令工具.在Spring Boot CLI中可以跑Groovy脚本
[下载地址](https://repo.spring.io/milestone/org/springframework/boot/spring-boot-cli)

### <font color=orange>Windows下安装</font>
下载Spring-boot-cli, 设置环境变量和path即可

### <font color=orange>Linux下安装</font>
```
brew tap pivotal/tap
brew install springboot
```
### <font color=orange>验证安装是否成功</font>
```
spring --version
```

### <font color=orange>创建项目</font>
* 查看现有技术列表
```
spring init --list
```
* 初始化项目
```
spring init --build=maven --java-version=1.8 --dependencies=web --packaging=jar --boot-version=2.0.2.RELEASE --groupId=com.coppco --artifactId=springboot
```

### <font color=orange>Spring Boot CLI发布一个简单服务</font>
* 新建`hello.groovy`文件, 内容如下
```java
@RestController
public class TeacherController {

	@RequestMapping(value = "/getTeacherInfo")
	public Teacher getInfo() {
		Teacher teacher = new Teacher("张三", "29");
		return teacher;
	}
}

class Teacher {
	private String name;
	private String age;

	public Teacher(String name, String age) {
		this.name = name;
		this.age = age;
	}


	public Teacher() {
	}

	public String getName() {
		return name;
	}

	public void setName(String name) {
		this.name = name;
	}

	public String getAge() {
		return age;
	}

	public void setAge(String age) {
		this.age = age;
	}
}
```
* 运行该脚本
```
spring run hello.groovy -- --server.port=9000
```
* 访问对应的url即可
```
http://localhost:8080/getTeacherInfo
```

## <font color=orange>Spring Boot 详解</font>
### <font color=orange>入口类和@SpringBootApplication</font>
* 入口类
Spring Boot通常默认有一个名为`*Application`的入口类(当然也可以更改为其他名称), 入口类有一个main方法, 这个方法其实就是一个标准的Java应用的入口方法. 在main方法中`SpringApplication.run(xxx.class, args)`启动Spring Boot应用项目.
```java
@SpringBootApplication
@ImportResource("classpath:applicationContext.xml")
public class Starter {
    public static void main(String[] args) {
        SpringApplication.run(Starter.class, args);
    }
}
```

* @SpringBootApplication
`@SpringBootApplication`是一个组合注解, 它组合了`@SpringBootConfiguration`、`@EnableAutoConfiguration`、`@ComponentScan`. 而`@SpringBootConfiguration`也是一个组合注解: 它组合了`@Configuration`.
若不使用`@SpringBootApplication`, 则可以使用`@EnableAutoConfiguration`、`@ComponentScan`和`@Configuration`代替.
<font color=red>Spring Boot会自动扫描`@SpringBootApplication`注解所在类的同级包以及下级包里的Bean. 建议入口类放置在`groupId+arctifactId`组合的包名下.</font>
	* @EnableAutoConfiguration
		* 让Spring Boot根据类路径中的jar包依赖为当前项目进行自动配置
		* 例如: 当添加了`spring-boot-starter-web`依赖, 会自动添加`Tomcat`、`Spring MVC`的依赖.
* @ImportResource
虽然Spring 4.x完全可以不使用xml方式来配置相关配置, 但是我们任然希望项目的一些配置使用xml方式来进行配置(如数据库相关的配置), 那么我们可以使用该注解引入配置文件.

### <font color=orange>关闭特定的自动配置</font>
使用@SpringBootApplication注解的exclude参数: 
```java
//关闭数据库默认自动配置
@SpringBootApplication(exclude = {DataSourceAutoConfiguration.class})
```

### <font color=orange>定制Banner</font>
* 修改Banner
	* 在Spring Boot启动时会有一个默认图案
	* 在`src/main/resources`下新建一个banner.txt
	* 通过[图像生成网站](http://patorjk.com)生成字符, 并拷贝到banner.txt中
	* 再次启动项目
* 关闭Banner
最新版本在main方法中修改: 
```java
//方式1: 
SpringApplication application = new SpringApplication(Starter.class);
        application.setBannerMode(Banner.Mode.OFF);
        application.run(args);

//方式2:
new SpringApplicationBuilder(Starter.class)
                .bannerMode(Banner.Mode.OFF)
                .run(args);
```

如果是比较老的版本: 
```java
//方式1: 
SpringApplication application = new SpringApplication(Starter.class);
application.setShowBanner(false);
application.run(args);

//方式2:
new SpringApplicationBuilder(Starter.class)
                .showBanner(false)
                .run(args);
```

### <font color=orange>Spring Boot的配置文件</font>
Spring Boot使用一个全局的配置文件`application.properties`或者`application.yml`放置在`src/main/resources`中.

Spring Boot不仅支持常规的properties配置文件, 还支持yaml语言的配置文件.而Intellij IDEA只对Spring Boot的properties配置提供自动提示功能, 所以推荐使用properties进行配置.

#### <font color=orange>更改默认配置</font>

如将Tomcat默认端口号8080更改为9090, 并将默认访问路径从`/`更改为`/hello`.
最新版本的Spring Boot在`application.properties`中添加: 
```
#端口
server.port=9090
#访问路径
server.servlet.context-path=/hello
```

其他的很多属性都可以修改.

#### <font color=orange>Spring Boot使用xml配置</font>
以前方式的xml配置也是支持的,  在入口类上使用`@ImportResource`注解
```java
@ImportResource("classpath:applicationContext.xml")
```

### <font color=orange>starter pom</font>
Spring Boot为我们提供了简化企业级开发绝大多数场景的starter pom, 只要使用了对应的starter pom, Spring Boot就会自动配置Bean.

|名称|描述|
|:---:|:---:|
|spring-boot-starter|SpringBoot核心starter，包含自动配置、日志、yaml配置文件的支持|
|spring-boot-starter-actuator|用于使用Spring Boot的Actuator，它提供了production ready功能来帮助你监控和管理应用程序|
|spring-boot-starter-activemq|用于使用Apache ActiveMQ实现JMS消息|
|spring-boot-starter-amqp|用于使用Spring AMQP和Rabbit MQ|
|spring-boot-starter-aop|使用Spring-AOP和AspectJ支持面向切面编程|
|spring-boot-starter-artemis|使用Apache Artemis实现JMS消息|
|spring-boot-starter-batch|对Spring Batch的支持|
|spring-boot-starter-cache|用于使用Spring框架的缓存支持|
|spring-boot-starter-cloud-connectors|对云平台（Cloud Foundry、Heroku）提供的服务提供简化的连接方式|
|spring-boot-starter-data-cassandra|用于使用分布式数据库 - Cassandra和Spring Data Cassandra|
|spring-boot-starter-data-couchbase|用于使用基于文档的数据库Couchbase和Spring Data Couchbase|
|spring-boot-starter-data-elasticsearch|通过Spring Data Elasticsearch提供对Elasticsearch的支持|
|spring-boot-starter-data-jpa|对JPA的支持，包含Spring-data-jpa、Spring-orm和Hibernate|
|spring-boot-starter-data-ldap|
|spring-boot-starter-data-mongodb|用于使用基于文档的数据库MongoDB和Spring Data MongoDB|
|spring-boot-starter-data-neo4j|用于使用图数据库Neo4j和Spring Data Neo4j|
|spring-boot-starter-data-redis|通过Spring-data-redis提供对Redis的支持|
|spring-boot-starter-data-rest|通过Spring-data-rest-webmvc将Spring Data repository暴露为REST形式的服务|
|spring-boot-starter-data-solr|通过Spring-data-solr提供对Apache Solr的支持|
|spring-boot-starter-freemarker|对freemarker模板引擎的支持|
|spring-boot-starter-groovy-templates|对groovy模板引擎的支持|
|spring-boot-starter-hateoas|用于使用Spring MVC和Spring HATEOAS实现基于超媒体的RESTful web应用|
|spring-boot-starter-integration|对系统集成框架Spinrg-integration的支持|
|spring-boot-starter-jdbc|对JDBC数据库的支持|
|spring-boot-starter-jersey|用于使用JAX-RS和Jersey构建RESTful web应用，可使用spring-boot-starter-web替代|
|spring-boot-starter-jooq|用于使用JOOQ访问SQL数据库，可使用springboot-starter-data-jpa或spring-boot-starter-jdbc替代|
|spring-boot-starter-jta-atomikos|通过atomikos对分布式事务的支持|
|spring-boot-starter-jta-bitronix|通过bitronix对分布式事务的支持|
|spring-boot-starter-jta-narayana|Spring Boot Narayana JTA Starter|
|spring-boot-starter-log4j2|支持使用Log4J日志框架|
|spring-boot-starter-logging|用于使用Logback记录日志，默认的日志starter|
|spring-boot-starter-mail|用于使用Java Mail和Spring框架email发送支持|
|spring-boot-starter-mobile|对Spring mobile的支持|
|spring-boot-starter-mustache|对mustache模板引擎的支持|
|spring-boot-starter-security|对Spring sercurity的支持|
|spring-boot-starter-social-facebook|通过Spring-social-facebook提供对Facebook的支持|
|spring-boot-starter-social-linkin|通过Spring-social-linkin提供对Linkin的支持|
|spring-boot-starter-social-twitter|通过Spring-social-twitter提供对Twitter的支持|
|spring-boot-starter-test|对常用的测试框架JUnit、Hamcrest和Mockito的支持，包含Spring-tests模块|
|spring-boot-starter-thymeleaf|对thymeleaf引擎的支持，包含于Spring整合的配置|
|spring-boot-starter-tomcat|SpringBoot默认的Servlet容器|
|spring-boot-starter-jetty|使用Jetty作为Servlet容器|
|spring-boot-starter-undertow|使用Undertow作为Servlet容器|
|spring-bootstarter-remote-shell|用于通过SSH，使用CRaSH远程shell监控，管理你的应用|
|spring-boot-starter-validation|用于使用Hibernate Validator实现Java Bean校验|
|spring-boot-starter-web|用于使用Spring MVC构建web应用，包括RESTful。Tomcat是默认的内嵌容器|
|spring-boot-starter-web-services|对Spring Web Services的支持|
|spring-boot-starter-websocket|对websocket开发的支持|



### <font color=orange>外部配置</font>
#### <font color=orange>命令行参数配置</font>
Spring Boot可以是基于jar包运行的, 打成jar包运行的可以修改端口等
```
java -jar xx.jar --service.port=9090
```

#### <font color=orange>常规属性配置</font>

##### <font color=orange>Spring Boot导入资源文件方式1</font>
直接在`application.properties`中添加字段即可, 然后使用`@Value`取即可
##### <font color=orange>Spring Boot导入资源文件方式2</font>
新建xxx.properties, 使用`@PropertySource`注解导入, 然后再使用`@Value`取即可
```java
@PropertySource("classpath:xxx.properties")
```

#### <font color=orange>类型安全的配置</font>
除了`@Value`注解注入每个值外, Spring Boot还提供使用`@ConfigurationProperties`将properties中的属性和一个Bean及其属性关联.

* 首先在入口类添加哪个类进行配置
```java
@EnableConfigurationProperties({Teacher.class})
```
* 如果入口类没有添加`@EnableConfigurationProperties`需要在Bean类那边添加`@Configuration`或者`@Component`
```java
@Configuration
//或者
@Component
@ConfigurationProperties(prefix = "teacher")
public class Teacher {

}
```
* 在Bean类上边添加`@ConfigurationProperties`并指定前缀
* 有时候会出现中文编码问题
	* 在`application.properties`中添加相关配置
```
#设置spring-boot 编码格式
banner.charset=UTF-8
server.tomcat.uri-encoding=UTF-8
spring.http.encoding.charset=UTF-8
spring.http.encoding.enabled=true
spring.http.encoding.force=true
spring.messages.encoding=UTF-8
```
	* 修改properties的文件编码类型为utf-8
	* Intellij IDEA依次打开`File` -> `Settings` -> `Editor` -> `File Encodings`, 将`Properties Files (*.properties)`下的`Default encoding for properties files`设置为`UTF-8`，将`Transparent native-to-ascii conversion`前的勾选上。
	* 重启Intellij IDEA即可.

### <font color=orange>日志配置</font>
Spring Boot支持Java Util Logging、Log4J、Log4J2和Logback作为日志框架, 默认情况下, Spring Boot使用Logback作为日志框架.
在`application.properties`中修改相关配置: 
```
#日志级别
logging.level.org.springframework.web=DEBUG
#日志文件
logging.file=log.txt
```

### <font color=orange>Profile配置</font>
Profile是Spring 用来针对不同的环境对不同的配置提供支持的, 全局Profile配置使用`application-{profile}.properties`(如application-dev.properties和application-product.properties), 通过在`application.properties`中设置来激活对应的环境.
```
#激活
spring.profiles.active=dev
#spring.profiles.active=product
```

## <font color=orange>Spring Boot的Web开发</font>
Spring Boot提供了spring-boot-starter-web为Web开发予以支持, 它为我们提供了嵌入的Tomcat以及Spring MVC的依赖. 而Web相关的自动配置在`spring-boot-autoconfigure`的`org.springframework.boot.autoconfigure.web`下

### <font color=orange>模板引擎</font>
Spring Boot提供了大量模板引擎, 包含FreeMarker、Groovy、Thymeleaf、Velocity和Mustache. Spring Boot推荐使用Thymeleaf作为模板引擎, 因为Thymeleaf提供了完美的Spring MVC支持.
#### <font color=orange>Thymeleaf基础知识</font>
* 首先添加依赖
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-thymeleaf</artifactId>
</dependency>
```
* 通过`xmlns:th=http://wwww.thymeleaf.org`命名空间, 将静态页面转为动态页面, 需要进行动态处理的元素将使用`th:`为前缀.
* 引用Web静态资源: `@{jquery-1.10.2.min.js}`
* 访问model中的属性: 通过`${name}`
* 迭代: 使用`th:each="person:${people}"`
* 判断: `${not #lists.isEmpty(people)}`判断perple是否为空, 还支持`>`、`<`、`>=`等
* JavaScript中访问model: `[[${name}]]`

#### <font color=orange>与SpringMVC的整合</font>
* xml配置方式
```xml
<bean id="templateResolver" class="org.thymeleaf.templateresolver.ServletContextTemplateResolver">
    <constructor-arg name="servletContext" ref="servletContext">
    </constructor-arg>
    <!--前缀-->
    <property name="prefix" value="/WEB-INF/"/>
    <!--后缀-->
    <property name="suffix" value=".html"/>
    <property name="templateMode" value="HTML5"/>
</bean>

<bean id="engine" class="org.thymeleaf.spring4.SpringTemplateEngine">
    <property name="templateResolver" ref="templateResolver"/>
</bean>

<bean class="org.thymeleaf.spring4.view.ThymeleafViewResolver">
    <property name="templateEngine" ref="engine"/>
</bean>
```
* Java配置方式, 在配置类中
```java
@Autowired
private ServletContext servletContext;

@Bean
public ServletContextTemplateResolver templateResolver() {
    ServletContextTemplateResolver servletContextTemplateResolver = new ServletContextTemplateResolver(servletContext);
    servletContextTemplateResolver.setPrefix("/WEB-INF/");
    servletContextTemplateResolver.setSuffix(".html");
    servletContextTemplateResolver.setTemplateMode("HTML5");
    return servletContextTemplateResolver;
}

@Bean
public SpringTemplateEngine templateEngine() {
    SpringTemplateEngine engine = new SpringTemplateEngine();
    engine.setTemplateResolver(templateResolver());
    return engine;
}

@Bean
public ThymeleafViewResolver thymeleafViewResolver() {
    ThymeleafViewResolver resolver = new ThymeleafViewResolver();
    resolver.setTemplateEngine(templateEngine());
    return resolver;
}
```

#### <font color=orange>与Spring Boot的整合</font>
Spring Boot和Thymeleaf整合很简单, Spring Boot会通过`org.springframework.autoconfigure.thymeleaf`包对Thymeleaf进行自动配置, 通过ThymeleafProperties源码我们可以看出, 在`application.properties`中可以通过`spring.thymeleaf`开头进行配置, 没有时会有默认值.
```java
@ConfigurationProperties("spring.thymeleaf")
public class ThymeleafProperties {
    private static final Charset DEFAULT_ENCODING = StandardCharsets.UTF_8;
    public static final String DEFAULT_PREFIX = "classpath:/templates/";
    public static final String DEFAULT_SUFFIX = ".html";
}
```

### <font color=orange>Web相关配置</font>
#### <font color=orange>Spring Boot相关的自动配置</font>
* 自动配置的ViewResolver
	* ContentNegotiatingViewResolver: 代理给不同的ViewResolver来处理不同的view
	* BeanNameViewResolver: 会根据返回的字符串找对应的Bean的视图进行渲染
	* InternalResourceViewResolver: 很常用的ViewResolver, 通过设置前缀、后缀等得到实际页面
* 自动配置的静态资源, 在自动配置类的`addResourceHandlers`方法中定义了如下自动配置
	* 类路径文件: `/static`、`/public`、`/resources`和`/META-INF/resources`文件夹下的静态文件直接映射为`/**`, 可以通过`http:/localhost:8080/**`访问.
	* webjar: webjar就是将常用的脚本框架封装在jar包中的jar包, 把`/META-INF/resources/webjars/`下的静态文件映射为`/webjar/**`, 可以通过`http:/localhost:8080/webjar/**`访问.
* 自动配置的Formatter和Converter
	* 只要我们定义了Converter、GenericConverter和Formatter接口的实现类的Bean, 这些Bean会自动注册到Spring MVC中.
* 自动配置的HTTPMessageConverters
	* 默认会加载`ByteArrayHttpMessageConverter`、`StringHttpMessageConverter`、`ResourceHttpMessageConverter`、`SourceHTTPMessageConverter`、`AllEncompassingFormHTTPMessageConverter`以及如果jackson的jar存在`MappingJackson2HTTPMessageConverter`和`MappingJackson2XmlHTTPMessageConverter`、gson的jar存在`GsonHTTPMessageConverter`
	* 自己新增时, 在配置类上
```java
@Bean
public HttpMessageConverter fastJsonHttpMessageConverters() {
    //1.需要定义一个Convert转换消息的对象
    FastJsonHttpMessageConverter fastConverter = new FastJsonHttpMessageConverter();
    //2.添加fastjson的配置信息，比如是否要格式化返回的json数据
    FastJsonConfig fastJsonConfig = new FastJsonConfig();
    fastJsonConfig.setSerializerFeatures(SerializerFeature.PrettyFormat);
    //3.在convert中添加配置信息
    fastConverter.setFastJsonConfig(fastJsonConfig);
    return fastConverter;
}

@Override
public void extendMessageConverters(List<HttpMessageConverter<?>> converters) {
    super.extendMessageConverters(converters);
    converters.add(fastJsonHttpMessageConverters());
}
```
* 静态首页的支持, 将静态index.html放在如下目录时, 会自定映射
	* `classpath:/META-INF/resources/index.html`
	* `classpath:/resources/index.html`
	* `classpath:/static/index.html`
	* `classpath:/public/index.html`
	* 其他目录可以在配置类中
```java
@Override
public void addViewControllers(ViewControllerRegistry registry) {
    registry.addViewController("/index").setViewName("index");
    registry.addViewController("/upload").setViewName("upload");
    registry.addViewController("/async").setViewName("async");
}
```
#### <font color=orange>Spring Boot自定义配置</font>
如果Spring Boot自动配置不满足我们的需求, 那么我们可以自己定义MVC的配置.
* 使用`@Configuration`注解和`@EnableWebMvc`注解
* 定义配置类继承`WebMvcConfigurerAdapter`重新相关方法(新版本实现`WebMvcConfigurer`接口), 无需`@EnableWebMvc`注解

#### <font color=orange>注册Servlet、Filter、Listener</font>
* 方式1、当时使用嵌入式Servlet容器时, 通过声明Spring Boot自动注册
```java
@Bean
pulic xxServlet xxServlet() {
    return new xxSevlet();
}
@Bean
pulic xxFilter xxFilter() {
    return new xxFilter();
}
```
* 方式2、注册ServletRegistrationBean、FilterRegistrationBean、ServletListenerRegistrationBean
```java
@Bean
public ServletRegistrationBean servletRegistrationBean() {
    return new ServletRegistrationBean(new XXServlet(), "/xx/*");
}

@Bean
public FilterRegistrationBean filterRegistrationBean() {
    FilterRegistrationBean fileterBean = new FilterRegistrationBean();
    fileterBean.setFilter(new XXFilter());
    fileterBean.setOrder(1);
    return fileterBean;
}
```

## <font color=orange>Spring Boot容器配置</font>
Spring Boot默认容器是Tomcat, 当然也可以使用Jetty、Undertow等, 配置属性在`org.springframework.boot.autoconfigure.web.ServerProperties`中做了定义, 我们只需要在`application.properties`中做配置即可, 通用的配置是以`server`作为前缀, 而Tomcat配置是以`server.tomcat`作为前缀.
### <font color=orange>`application.properties`配置文件</font>
```
server.port=8080 #配置程序端口, 默认8080
server.session-timeout= #用户session过期时间, 秒为单位
server.context-path=/ #访问路径, 默认/
server.tomcat.uri-encoding=UTF-8 #配置Tomcat编码
```
### <font color=orange>代码配置</font>
通用的配置类可以实现`EmbeddedServletContainerCustomizer`接口的类, 若要直接配置Tomcat、Jetty、Undertow则可以直接定义`TomcatEmbeddedServletContainerFactory`、`JettyEmbeddedServletContainerFactory`、`UndertowEmbeddedServletContainerFactory`的Bean.

* 新建类(如果该类在配置类中, 注意需要添加`static`)
```java
@Component
public class CustomServletContainer implements EmbeddedServletContainerCustomizer {
    @Override
    public void customize(ConfigurableEmbeddedServletContainer container) {
        containner.setPort(8090);
    }
}
```

* 使用特定的Bean
```java
@Bean
public EmbeddedServletContainerFactory servletContainer() {
    TomcatEmbeddedServletContainerFactory factory = new TomcatEmbeddedServletContainerFactory;
    factory.setPort(8090);
    return factory;
}
```

### <font color=orange>内置Tomcat不支持JSP</font>
内置容器是Tomcat不支持JSP页面, 需要添加额外的包才能支持:
```
<dependency>
    <groupId>org.apache.tomcat.embed</groupId>
    <artifactId>tomcat-embed-jasper</artifactId>
    <scope>provided</scope>
</dependency>
```


### <font color=orange>替换Tomcat</font>
Spring Boot默认使用Tomcat作为内嵌Servlet容器, 如果需要替换其他容器, 那么可以在`pom.xml`移除Tomcat的依赖并添加其他容器的依赖.
```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
    <exclusions>
        <exclusion>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-tomcat</artifactId>
        </exclusion>
    </exclusions>
</dependency>
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-jetty</artifactId>
    <!--<artifactId>spring-boot-starter-undertow</artifactId>-->
</dependency>
```


### <font color=orange>配置证书SSL</font>
* 生成证书
	* 自签名证书(浏览器会显示未认证): 使用JDK工具生成`keytool -genkey -alias tomcat -keyalg RSA`
	* 认证的SSL证书(推荐): [阿里云](https://common-buy.aliyun.com/?spm=5176.7968328.911106.btn1.16a41232t0DJYw&commodityCode=cas#/buy)、腾讯云都可以免费生成.
* Spring Boot配置SSL
	* 将生成`.keystore`拷贝到项目目录中
	* 在`application.properties`中添加SSL配置
```
server.prot=8443
server.ssl.key-store=classpath:.keystore
server.ssl.key-store-password=xxxxx
server.ssl.keyStoreType=JKS
server.ssl.keyAlias:tomcat
```
* http转向https
在入口类中添加: 
```java
@SpringBootApplication
public class Starter {

    // 这是spring boot 1.5.X以下版本的 添加了这个，下一个就不用添加了
    @Bean
    public EmbeddedServletContainerFactory servletContainer() {
        TomcatEmbeddedServletContainerFactory tomcat = new TomcatEmbeddedServletContainerFactory(){
            @Override
            protected void postProcessContext(Context context) {
                SecurityConstraint securityConstraint = new SecurityConstraint();
                securityConstraint.setUserConstraint("CONFIDENTIAL");
                SecurityCollection collection = new SecurityCollection();
                collection.addPattern("/*");
                securityConstraint.addCollection(collection);
                context.addConstraint(securityConstraint);
            }
        };
        tomcat.addAdditionalTomcatConnectors(httpConnector()); // 添加http
        return tomcat;
    }
    // 这是spring boot 2.0.X版本的 添加这个，上一个就不用添加了
    @Bean
    public ServletWebServerFactory servletContainer() {
        TomcatServletWebServerFactory factory = new TomcatServletWebServerFactory(){
            @Override
            protected void postProcessContext(Context context) {
                SecurityConstraint securityConstraint = new SecurityConstraint();
                securityConstraint.setUserConstraint("CONFIDENTIAL");
                SecurityCollection collection = new SecurityCollection();
                collection.addPattern("/*");
                securityConstraint.addCollection(collection);
                context.addConstraint(securityConstraint);
            }
        };
        factory.addAdditionalTomcatConnectors(httpConnector());
        return factory;
    }
    //配置http
    private Connector httpConnector() {
        Connector connector = new Connector("org.apache.coyote.http11.Http11NioProtocol");
        Http11NioProtocol http11NioProtocol = (Http11NioProtocol) connector.getProtocolHandler();
        connector.setScheme("http");
        connector.setPort(8080);
        connector.setSecure(false);
        connector.setRedirectPort(8443);
        //设置最大线程数
        http11NioProtocol.setMaxThreads(100);
        //设置初始线程数  最小空闲线程数
        http11NioProtocol.setMinSpareThreads(20);
        //设置超时
        http11NioProtocol.setConnectionTimeout(5000);
        return connector;
    }
}
```

### <font color=orange>Favicon配置</font>
Spring Boot提供了一个默认的Favicon.
#### <font color=orange>关闭Favicon</font>
```
spring.mvc.favicon.enabled=false
```
#### <font color=orange>自定义Favicon</font>
只需要把自己的`favicon.ico`(文件名不能更改)文件放在`类路径根目录`、`类路径META-INF/resources/`、`类路径resources/`、`类路径static/`或者`类路径public/`下即可.

### <font color=orange>WebSocket</font>
请移步[这里](), 关于WebSocket的服务器端以及iOS连接Websocket的实现.

## <font color=orange>Spring Boot中的事务</font>
### <font color=orange>Spring中的事务</font>
* 编程式事务(不推荐)
* 基于TransactionProxyFactoryBean的事务(不推荐)
* 基于AspectJ的声明式事务
	* 使用`xml配置文件`方式
		* 配置事务管理器Bean
```xml
<!--配置事务管理器-->
<bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
	<property name="dataSource" ref="dataSource"/>
</bean>
```
		* 配置事务通知(事务的增强)
```
<tx:advice id="txAdvice" transaction-manager="transactionManager">
    <tx:attributes>
        <tx:method name="save*" propagation="REQUIRED"/>
        <tx:method name="insert*" propagation="REQUIRED"/>
        <tx:method name="delete*" propagation="REQUIRED"/>
        <tx:method name="update*" propagation="REQUIRED"/>
        <tx:method name="find*" propagation="SUPPORTS" read-only="true"/>
        <tx:method name="select*" propagation="SUPPORTS" read-only="true"/>
        <tx:method name="get*" propagation="SUPPORTS" read-only="true"/>
    </tx:attributes>
</tx:advice>
```
		* 配置切面
```
<!--切面-->
<aop:config>
    <aop:advisor advice-ref="txAdvice" pointcut="execution(* com.coppco.service.*.*(..))"/>
</aop:config>
```
	* 使用`注解`方式
		* 配置事务管理器Bean
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
		* 在需要使用事务的类上添加注解`@Transactional`即可
```
@Transactional(propagation = Propagation.REQUIRED)
@Service
public class AccountService {
}
```

### <font color=orange>Spring Boot中的事务</font>
使用`@EnableTransactionManagement`注解在配置类上来开启声明式事务的支持, Spring容器会自动扫描注解了`@Transactional`的方法和类.
* 注解在类上: 所有public方法都开启事务
* 注解在方法上: 该方法开启注解
* 同时类上和方法上都存在: 类级别的注解会重载方法级别的注解

## <font color=orange>Spring Boot开发部署</font>

### <font color=orange>Spring Boot热部署</font>
当我们修改了类或者配置文件时, 需要生效会重新运行, 会很麻烦. 
Spring Boot1.3版本以后可以使用热部署方便很多.
#### <font color=orange>模板热部署</font>
Spring Boot模板引擎默认都是开启缓存的, 可以在`application.properties`中关闭缓存
```
#thymeleaf缓存
spring.thymeleaf.cache=false
#FreeMarker缓存
spring.freemarker.cache=false
#Groovy缓存
spring.groovy.template.cache=false
#Velocity缓存
spring.velocity.cache=false
```
#### <font color=orange>Java类和配置文件</font>
* 1、pom.xml添加Maven依赖
```
<!-- 热部署 -->
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-devtools</artifactId>
    <optional>true</optional>
</dependency>
```
* 2、pom.xml中插件
```
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <fork>true</fork>//该配置必须
            </configuration>
        </plugin>
    </plugins>
</build>
```
* 3、开启Intellij IDEA自动Build功能 
	* `setting` -- `Build, Execution, Deployment` -- `Compiler` -- `Build project automatically`勾选上
* 4、`command + option + shift + /` -- `Registry` -- `compiler.automake.allow.when.app.running` -- 勾选即可.
* 5、重新编译后, 在修改后保存时会重新加载.

### <font color=orange>Spring Boot常规部署</font>
#### <font color=orange>jar包形式</font>
* 打包: 当我们新建Spring Boot项目的时候, 选择打包方式是`jar`, 只需使用maven插件
```
mvn package
```
* 运行: 
```
java -jar xx.jar
```
* Linux下运行软件通常把它注册为服务, 需要修改`pom.xml`后重新打包
```
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <fork>true</fork>
                <executable>true</executable>
            </configuration>
        </plugin>
    </plugins>
</build>
```
* Linux下部署
	* 安装JDK
```
rpm -ivh jdk-8u51-linux-x64.rpm
```
	* 基于`init.d`(CentOS 6.6)注册服务
		* 注册服务, springbootDemo就是服务名, 项目日志在`/home/log/springbootDemo.log`下
```
sudo ln -s /home/apps/springbootDemo-0.0.1-SNAPSHOT.jar /etc/init.d/springbootDemo
```
		* 启动服务
```
#启动服务
service springbootDemo start
#停止服务
service springbootDemo stop
#服务状态
service springbootDemo status
#开机启动
chkconfig springbootDemo on
```
	* 基于`systemd(CnetOS 7)`注册服务
		* 注册服务, 在`/etc/systemd/system/`目录下新建`springbootDemo.service`, 实际中需要修改Description和ExexStart
```
[Unit]
Description=springbootDemo 
After=syslog.target

[service]
ExexStart= /usr/bin/java -jar /home/apps/springbootDemo-0.0.1-SNAPSHOT.jar

[Install]
WantedBy=multi-user.target
```
		* 相关命令
```
#启动服务
systemctl start springbootDemo.service
#停止服务
systemctl stop springbootDemo.service
#服务状态
systemctl status springbootDemo.service
#开机启动
systemctl enable springbootDemo.service
#日志
journalctl -u pringbootDemo.service
```
#### <font color=orange>war包形式</font>
* 如果`pom.xml`文件打包方式为`war`, 可以直接使用Maven插件
```
mvn package
```
* 如果`pom.xml`文件打包方式为`jar`
	* 首先修改`pom.xml`, 把打包方式改为war
```
<packaging>war</packaging>
```
	* 覆盖默认的容器依赖
```
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-tomcat</artifactId>
    <scope>provided</scope>
</dependency>
```
	* 新增ServletInitializer类
```
public class ServletInitializer extends SpringBootServletInitializer {
    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder application) {
        return application.sources(xxx.class);
    }
}

```