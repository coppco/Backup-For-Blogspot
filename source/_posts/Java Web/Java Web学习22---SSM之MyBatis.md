---
layout: post
title: Java Web学习22---SSM之MyBatis
comments: true
toc: true
date: 2016-12-29 11:42:58
tags:
	- Java
	- SSM
	- MyBatis
---

SSM（Spring，SpringMVC，MyBatis）其中Spring是一个轻量级的控制反转（IoC）和面向切面（AOP）的容器框架。Spring MVC分离了控制器、模型对象、分派器以及处理程序对象的角色，这种分离让它们更容易进行定制。MyBatis是一个支持普通SQL查询，存储过程和高级映射的优秀持久层框架。

<!--more-->



## <font color=orange> MyBatis </font>

&emsp;&emsp;MyBatis 本是apache的一个开源项目iBatis, 2010年这个项目由apache software foundation 迁移到了google code，并且改名为MyBatis 。2013年11月迁移到Github。 	MyBatis是一个优秀的持久层框架，它对jdbc的操作数据库的过程进行封装，使开发者只需要关注 SQL 本身，而不需要花费精力去处理例如注册驱动、创建connection、创建statement、手动设置参数、结果集检索等jdbc繁杂的过程代码。
&emsp;&emsp;Mybatis通过xml或注解的方式将要执行的各种statement（statement、preparedStatement、CallableStatement）配置起来，并通过java对象和statement中的sql进行映射生成最终执行的sql语句，最后由mybatis框架执行sql并将结果映射成java对象并返回。
&emsp;&emsp;MyBatis是一个优秀的轻量级持久层框架.

### <font color=green> 最初的使用JDB的C方式 </font>

```java
Connection connection = null;
PreparedStatement preparedStatement = null;
ResultSet resultSet = null;

    try {
        Class.forName("com.mysql.jdbc.Driver");
        connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/learnSSM?characterEncoding=utf-8&amp;useSSL=false", "root", "123456");
        String sql = "select * from user where username =?";
        preparedStatement = connection.prepareStatement(sql);
        preparedStatement.setString(1, "熊大");
        resultSet = preparedStatement.executeQuery();

        while (resultSet.next()) {
            System.out.println(resultSet.getString("id"));   
        }
    } catch (Exception e) {

    } finally {
        if (null != resultSet) {
            try {
                resultSet.close();
            } catch (Exception e) {
                
            }
        }

        if (null != preparedStatement) {
            try {
                preparedStatement.close();
                    ;
            } catch (Exception e) {
                
            }
        }

        if (null != connection) {
            try {
                connection.close();
            } catch (Exception e) {
                
            }
        }
    }
}
```
### <font color=green> 使用JDBC的问题 </font>
* 数据库链接创建、释放频繁造成系统资源浪费从而影响系统性能，如果使用数据库链接池可解决此问题。
* Sql语句在代码中硬编码，造成代码不易维护，实际应用sql变化的可能较大，sql变动需要改变java代码。
* 使用PreparedStatement向占有位符号传参数存在硬编码，因为sql语句的where条件不一定，可能多也可能少，修改sql还要修改代码，系统不易维护。
* 对结果集解析存在硬编码（查询列名），sql变化导致解析代码变化，系统不易维护，如果能将数据库记录封装成pojo对象解析比较方便。

## <font color=orange> MyBatis的简单使用 </font>

### <font color=green> MyBatis配置 </font>
* SqlMapConfig.xml
	* 此文件作为mybatis的全局配置文件，配置了mybatis的运行环境等信息。
* Mapper.xml文件即sql映射文件
	* 文件中配置了操作数据库的sql语句。此文件需要在SqlMapConfig.xml中加载。
	
### <font color=green> MyBatis运行流程 </font>
* 通过MyBatis配置信息构造SqlSessionFactory绘画工厂.
* 会话工厂创建sqlSession即会话，操作数据库需要通过sqlSession进行.
* MyBatis底层自定义了Executor执行器接口操作数据库，Executor接口有两个实现，一个是基本执行器、一个是缓存执行器。
* Mapped Statement也是MyBatis一个底层封装对象，它包装了MyBatis配置信息及sql映射信息等。Mapper.xml文件中一个sql对应一个Mapped Statement对象，sql的id即是Mapped Statement的id。
* Mapped Statement对sql执行输入参数进行定义，包括HashMap、基本类型、pojo，Executor通过Mapped Statement在执行sql前将输入的java对象映射至sql中，输入参数映射就是jdbc编程中对preparedStatement设置参数。
* Mapped Statement对sql执行输出结果进行定义，包括HashMap、基本类型、pojo，Executor通过Mapped Statement在执行sql后将输出结果映射至java对象中，输出结果映射过程相当于jdbc编程中对结果的解析处理过程。

### <font color=green> MyBatis简单使用 </font>
#### <font color=blue> 一、导入jar包 </font>
目前MyBatis托管在[Github](https://github.com/)上, [下载地址](https://github.com/mybatis/mybatis-3/releases).
#### <font color=blue> 二、导入log4j的配置文件`log4j.properties` </font>
```
# Global logging configuration
log4j.rootLogger=DEBUG, stdout
# Console output...
log4j.appender.stdout=org.apache.log4j.ConsoleAppender
log4j.appender.stdout.layout=org.apache.log4j.PatternLayout
log4j.appender.stdout.layout.ConversionPattern=%5p [%t] - %m%n
```
#### <font color=blue> 三、SqlMapConfig.xml配置文件 </font>
* <font color=red>properties: 属性(常用)</font>
	* 定义一个`db.properties`文件
```xml
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/r1225?characterEncoding=utf-8
jdbc.username=root
jdbc.password=123456
```
	* 导入property配置文件
```xml
<properties resource="db.properties" />
```
* setting: 全局配置参数
* <font color=red>typeAliases: 类型别名(常用)</font>
	* MyBatis自定义别名
```xml
<typeAliases>
    <!-- 定义单个pojo类别名
        type:类的全路径名称
        alias:别名
    -->
    <typeAlias type="com.coppco.pojo.User" alias="user"/>
    <!-- 使用包扫描的方式批量定义别名,以后别名等于类名,不区分大小写,但是建议按照java命名规则来,首字母小写,以后每个单词的首字母大写-->
    <package name="com.coppco.pojo"/>
</typeAliases>
```
	* MyBatis默认的别名
	
|别名|映射的类型|
|:--:|:--:|:--:|
|_byte |	byte |
|_long |	long |
|_short |	short |
|_int |	int |
|_integer |	int |
|_double |	double |
|_float |	float |
|_boolean |	boolean |
|string |	String |
|byte |	Byte |
|long |	Long |
|short |	Short |
|int |	Integer |
|integer |	Integer |
|double |	Double |
|float |	Float |
|boolean| 	Boolean |
|date |	Date |
|decimal |	BigDecimal |
|bigdecimal |	BigDecimal| 
|map	|Map|

* typeHandlers: 类型处理器
* objectFactory: 对象工厂
* plugins: 插件
* environments: 环境集合属性对象, <font color=red>在整合Spring后配置将废除</font>
	* environment: 环境子属性对象
	* transactionManager: 事务管理器
	* dataSource: 数据源
* <font color=red>mappers: 映射器(常用)</font>
```xml
<mappers>
    <mapper resource="User.xml"/>
    <!--使用class属性引入接口的全路径名称:
        使用规则:
            1. 接口的名称和映射文件名称除扩展名外要完全相同
            2. 接口和映射文件要放在同一个目录下
    -->
    <mapper class="com.coppco.mapper.UserMapper"/>

    <!-- 使用包扫描的方式批量引入Mapper接口
         使用规则:
             1. 接口的名称和映射文件名称除扩展名外要完全相同
             2. 接口和映射文件要放在同一个目录下
    -->
    <package name="com.coppco.mapper"/>
</mappers>
```
	* 使用`<mapper resource="路径">`引入映射文件
	* 使用`<mapper class="接口全路径"/>`引入接口全路径
	* 使用`<package name="包名">`包扫描的方式批量引入
	
#### <font color=blue> 四、Po类作为MyBatis进行sql映射使用 </font>
Po类用在持久层,还可以在增加或者修改的时候,从页面直接传入action中,它里面的Java Bean类名等于表名,属性名等于表的字段名,还有对应的getter、setter方法.

#### <font color=blue> 五、Sql映射文件 </font>
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">
    <!-- namespace: 命名空间，用于隔离sql语句，后面会讲另一层非常重要的作用。-->
    <mapper namespace="test">
        <!-- select: 查询
             parameterType: 指定传入参数类型
             resultType: 指定结果类型
             #{}占位符: 占位作用, 如果传入的是基本数据类型(String, long double int boolen float), #{}中变量名可以随便写
             ${}拼接符: 字符串原样拼接, 如果传入的是基本数据类型(String, long double int boolen float), ${}中变量名必须是value
            注意: 拼接符有sql注入的风险, 所以需要慎重
        -->
        <select id="fingUserById" parameterType="java.lang.Integer" resultType="com.coppco.pojo.User">
            select * from user where id = #{id}
        </select>

        <select id="fingUserByUserName" parameterType="java.lang.String" resultType="com.coppco.pojo.User">
            select * from user where username like '%${value}%'
        </select>

        <!--如果传入的是pojo类型,  那么#{}中的变量必须是属性.属性.属性-->
        <insert id="insetUser" parameterType="com.coppco.pojo.User">
            <!--要返回数据库自增主键-->
            <selectKey keyProperty="id" order="AFTER" resultType="java.lang.Integer">
                select LAST_INSERT_ID()
            </selectKey>
            insert into user (username, birthday, sex, address) values(#{username}, #{birthday}, #{sex}, #{address})
        </insert>
</mapper>
```
* #{}占位符: 占位
	* 如果传入的是基本类型,那么#{}中的变量名称可以随意写
	* 如果传入的参数是pojo类型,那么#{}中的变量名称必须是pojo中的属性.属性
* ${}拼接符:字符串原样拼接
	* 如果传入的是基本类型,那么${}中的变量名必须是value
	* 如果传入的参数是pojo类型,那么${}中的变量名称必须是pojo中的属性.属性.属性...
	* 注意:使用拼接符有可能造成sql注入,在页面输入的时候或aciotn中可以加入校验,不可输入sql关键字,不可输入空格等
* parameterType: 传入参数类型
* resultType: 输出参数类型
* 动态SQL, 例如我们查询时的条件, 可以借助`if`或者`foreach`标签
```xml
<select id="findUserByUsernameAndSex" parameterType="user" resultType="user">
    select * from user
    <where>
        <if test="username != null and username !=''">
            and username like '%${username}%'
        </if>
        <if test="sex != null and sex !=''">
            and sex=#{sex}
        </if>
    </where>
</select>
```
	* `where`标签: 会自定添加where关键字, 会自动去掉第一个条件的关键字
	* `if`标签: 会根据条件拼接条件
* 在Sql映射文件中可以通过`sql`标签来定义条件, 通过`include`引用
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.coppco.mapper.UserMapper">
    <sql id="where_user">
        <where>
            <if test="username != null and username !=''">
                and username like '%${username}%'
            </if>

            <if test="sex != null and sex !=''">
                and sex=#{sex}
            </if>
        </where>
    </sql>
    
    <select id="findUser" parameterType="user" resultType="user">
        select * from user
        <include refid="where_user"/>
    </select>
</mapper>
```
* `foreach`标签: 用于遍历
```xml
<foreach collection="" item="" open="" close="" separator="">
</foreach>
```
	* collection: 需要遍历的集合
	* item: 每次遍历出对象
	* open: 循环开始拼接的字符串
	* close: 循环结束拼接的字符串
	* separator: 循环中拼接的分隔符

#### <font color=blue> 六、加载映射文件</font>
需要在MyBatis的配置文件中引入Sql映射文件.
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!-- 和spring整合后 environments配置将废除-->
    <environments default="development">
        <environment id="development">
            <!-- 使用jdbc事务管理-->
            <transactionManager type="JDBC" />
            <!-- 数据库连接池 使用MyBatis中的连接池 -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver" />
                <property name="url" value="jdbc:mysql://localhost:3306/learnMyBatis?characterEncoding=utf-8" />
                <property name="username" value="root" />
                <property name="password" value="123456" />
            </dataSource>
        </environment>
    </environments>
    <!-- 加载配置文件 -->
    <mappers>
        <mapper resource="sqlmap/User.xml"/>
    </mappers>
</configuration>
```

#### <font color=blue> 七、测试</font>
```java
<!--使用原生Dao模式-->
//获取资源文件
Reader reader = Resources.getResourceAsStream("SqlMapConfig.xml");
//创建会话工厂
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader);
//创建会话
SqlSession sqlSession = factory.openSession();
/**
 * 第一个参数 :  namespace+sql的id
 * 第二个参数: 传入的参数
 */
User one = (User) sqlSession.selectOne("test.fingUserById", 1);
sqlSession.close();

<!--使用Mapper接口代理模式-->
//获取资源文件
Reader reader = Resources.getResourceAsStream("SqlMapConfig.xml");
//创建会话工厂
SqlSessionFactory factory = new SqlSessionFactoryBuilder().build(reader);
//创建会话
SqlSession sqlSession = factory.openSession();
//反射出接口
UserMapper mapper = sqlSession.getMapper(UserMapper.class);
//调用接口里面的方法
List<Orders> ordersAndUsers = mapper.findOrdersAndUsers();
```

### <font color=orange> MyBatis的原生Dao </font>
只需Sql映射文件即可, 调用时根据`namespace`+sql语句的`id`调用.
需要Sql映射文件, 同时编写Dao接口和实现类, 在实现类中通过sqlSession的selectOne等方法调用.
### <font color=orange> MyBatis的Mapper接口代理 </font>


* dao接口文件
	* Dao接口文件和映射文件必须在同一个目录中
	* Dao接口文件名称和映射文件名称除了后缀名必须一样
	* <font color=red>如果是Maven, 映射文件放在`src/main/java`中时, 默认不会打包进相应的jar或war包中</font>
		* 解决方式1: 在pom.xml中
```xml
<build>
    <!--这样也可以把所有的xml文件，打包到相应位置。-->
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
```
		* 解决方式2: 在pom.xml中使用插件(下面插件二选一即可)
```xml
</build>
    <plugins>
    <!-- 利用此plugin，把源代码中的xml文件，到相应位置，这里主要是为了打包Mybatis的mapper.xml文件 -->
        <plugin>
            <groupId>org.codehaus.mojo</groupId>
            <artifactId>build-helper-maven-plugin</artifactId>
            <version>1.8</version>
            <executions>
                <execution>
                    <id>add-resource</id>
                    <phase>generate-resources</phase>
                    <goals>
                        <goal>add-resource</goal>
                    </goals>
                    <configuration>
                        <resources>
                            <resource>
                                <directory>src/main/java</directory>
                                <includes>
                                    <include>**/*.xml</include>
                                </includes>
                            </resource>
                        </resources>
                    </configuration>
                </execution>
            </executions>
        </plugin>
        <!--插件二选一即可-->
        <plugin>
            <artifactId>maven-resources-plugin</artifactId>
            <version>2.5</version>
            <executions>
                <execution>
                    <id>copy-xmls</id>
                    <phase>process-sources</phase>
                    <goals>
                        <goal>copy-resources</goal>
                    </goals>
                    <configuration>
                        <outputDirectory>${basedir}/target/classes</outputDirectory>
                        <resources>
                            <resource>
                                <directory>${basedir}/src/main/java</directory>
                                <includes>
                                    <include>**/*.xml</include>
                                </includes>
                            </resource>
                        </resources>
                    </configuration>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```
* 映射文件xml
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd"> 
<mapper namespace="com.coppco.mapper.UserMapper">
    <select id="fingUserById" parameterType="java.lang.Integer" resultType="com.coppco.pojo.User">
        select * from user where id = #{id}
    </select>
    <select id="findUserByUsername" parameterType="java.lang.String" resultType="com.coppco.pojo.User">
        select * from user where username like '%${value}%'
    </select>
    <insert id="insetUser" parameterType="com.coppco.pojo.User">
        insert into user (username, birthday, sex, address) values(#{username}, #{birthday}, #{sex}, #{address})
    </insert>
</mapper>
```
	* 映射文件中 `namespace` 等于 接口的全路径名称
	* 映射文件中sql语句的`id`等于接口的方法名称
	* 映射文件中传入参数类型`parameterType`等于接口方法的传入参数类型
	* 映射文件中返回结果类型`resultType`等于接口方法的返回结果类型
* SqlMapConfig.xml中导入Sql映射文件
```xml
<mappers>
    <mapper resource="User.xml"/>
    <!--
        使用class属性引入接口的全路径名称:
        使用规则:
        1. 接口的名称和映射文件名称除扩展名外要完全相同
        2. 接口和映射文件要放在同一个目录下
    -->
    <mapper class="com.coppco.mapper.UserMapper"/>

    <!-- 使用包扫描的方式批量引入Mapper接口
        使用规则:
        1. 接口的名称和映射文件名称除扩展名外要完全相同
        2. 接口和映射文件要放在同一个目录下
    -->
    <package name="com.coppco.mapper"/>
</mappers>
```

### <font color=orange> MyBatis的关联查询 </font>

#### <font color=orange> 一对一: 自动映射 </font>
在映射文件中使用`resultType`或`parameterType`表示自动映射.
```xml
<select id="fingUserById" parameterType="integer" resultType="user">
    select * from user where id = #{id}
</select>
```

#### <font color=orange> 一对一: 手动映射 </font>
在映射文件中需要使用`resultMap`或`parameterMap`表示手动映射, 需要自定义`parameterMap`、`resultMap`.
```xml
    <!-- 定义一个resultMap, id: resultMap的唯一表示,  type: 查询的结果放入的指定对象中 -->
    <resultMap id="findAllResultMap" type="orders">
        <!--手动映射需要指定 数据库中字段和pojo中的属性名-->

        <!--id标签: 指定主键的对应关系. colum: 数据库字段, property: pojo的属性-->
        <id column="id" property="id"/>
        <!--result标签: 指定非主键的对应关系-->
        <result column="user_id" property="userId"/>
        <result column="number" property="number"/>
        <result column="createtime" property="createtime"/>
        <result column="note" property="note"/>

        <!--association: 指定单个对象的对应关系-->
        <association property="user" javaType="user">
            <id column="uid" property="id"/>
            <result column="username" property="username"/>
            <result column="birthday" property="birthday"/>
            <result column="sex" property="sex"/>
            <result column="address" property="address"/>
        </association>
    </resultMap>

    <select id="findOrdersAndUsers" resultMap="findAllResultMap">
        select a.*, b.id uid, b.username,birthday, sex, address  from orders a, user b where a.user_id = b.id
    </select>
```
#### <font color=orange> 一对多: 手动映射 </font>
在映射文件中需要使用`resultMap`或`parameterMap`表示手动映射, 需要自定义`parameterMap`、`resultMap`.
```
    <!--一对多: 手动映射-->
    <resultMap type="user" id="userAndOrdersResultMap">
        <id column="id" property="id"/>
        <result column="username" property="username"/>
        <result column="birthday" property="birthday"/>
        <result column="sex" property="sex"/>
        <result column="address" property="address"/>

        <!-- 指定对应的集合对象关系映射
        property:将数据放入User对象中的ordersList属性中
        ofType:指定ordersList属性的泛型类型
         -->
        <collection property="ordersList" ofType="orders">
            <id column="oid" property="id"/>
            <result column="user_id" property="userId"/>
            <result column="number" property="number"/>
            <result column="createtime" property="createtime"/>
        </collection>
    </resultMap>
    <select id="findUserAndOrders" resultMap="userAndOrdersResultMap">
        select a.*, b.id oid ,user_id, number, createtime
        from user a, orders b where a.id = b.user_id
    </select>
```


## <font color=orange> Spring与MyBatis整合 </font>

### <font color=green> 原生Dao接口方式 </font>
#### <font color=green> 一、导入相关的jar包 </font>

首先需要导入Spring、MyBatis、mysql-connecter-java、mybatis-spring以及c3p0等相关jar包

#### <font color=green> 二、相关配置文件 </font>
* log4j配置文件: `log4j.properties`
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
log4j.appender.LOGFILE.File=xxx
log4j.appender.LOGFILE.Append=true
log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
log4j.appender.LOGFILE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n

```
* 数据库配置文件: `db.properties`
```xml
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/r1225?characterEncoding=utf-8
jdbc.username=root
jdbc.password=123456
```
* Spring配置文件: `applicationContext.xml`
这里数据库连接池需要交给Spring来管理, 同时SqlSessionFactory由`spring-mybatis`中的`org.mybatis.spring.SqlSessionFactoryBean`提供.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-3.0.xsd">


    <!--导入数据库文件-->
    <context:property-placeholder location="classpath:db.properties"/>

    <!-- 注解扫描 -->
    <context:component-scan base-package="com.coppco"/>

    <!--连接池 c3p0-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"></property>
        <property name="jdbcUrl" value="${jdbc.url}"></property>
        <property name="user" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>

    <!-- 整合Mybatis会话工厂 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--配置文件-->
        <property name="configLocation" value="classpath:SqlMapConfig.xml"/>
        <!--数据源-->
        <property name="dataSource" ref="dataSource"/>
        <!-- Sql映射文件, 也可以在MyBatis配置文件中加载 -->
        <property name="mapperLocations" value="classpath:com/coppco/**/*.xml" />
    </bean>

</beans>
```
	* 此处的Sql映射文件也可以在`SqlMapConfig.xml`中导入
* Mybatis配置文件: `SqlMapConfig.xml`
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--别名-->
    <typeAliases>
        <!--包扫描-->
        <package name="com.coppco.pojo"/>
    </typeAliases>
    
    <mappers>
        <!--导入Sql映射文件-->
        <mapper resource="User.xml"/>
    </mappers>
</configuration>

```
#### <font color=green> 三、编写pojo类、接口Dao、DaoImpl实现类以及Sql映射文件 </font>
* pojo类: 普通java bean
* Dao实现类: 继承SqlSessionDaoSupport并实现Dao接口, 注入`sqlSessionFactory`, 方法中通过SessionFactory获取Session, 再通过Sql映射文件中namespace+Sql语句id来调用.
```java
@Component(value = "userDao")
@Scope(value = "prototype")
public class UserDaoImpl extends SqlSessionDaoSupport implements UserDao {

    @Resource(name = "sqlSessionFactory")
    private SqlSessionFactory factory;

    @PostConstruct
    private void initialize() {
        setSqlSessionFactory(this.factory);
    }

    public User findUserById(Integer id) {
        //sqlSesion是线程不安全的,所以它的最佳使用范围在方法体内
        SqlSession openSession = this.getSqlSession();
        User user = openSession.selectOne("test.fingUserById", id);
        //整合后会话归spring管理,所以不需要手动关闭.
        //openSession.close();
        return user;
    }
}
```
* Sql映射文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<mapper namespace="test">
    <select id="fingUserById" parameterType="java.lang.Integer" resultType="com.coppco.pojo.User">
        select * from user where id = #{id}
    </select>
</mapper>
```

#### <font color=green> 四、测试类 </font>

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")

public class MyTest {

    @Resource(name = "userDao")
    private UserDao userDao;

    @Test
    public void testYuanShengDao() {
        User user = userDao.findUserById(1);
        System.out.println(user);
    }
}
```

### <font color=green> Mapper接口代理 </font>
#### <font color=green> 一、导入相关的jar包 </font>

首先需要导入Spring、MyBatis、mysql-connecter-java、mybatis-spring以及c3p0等相关jar包

#### <font color=green> 二、相关配置文件 </font>
* log4j配置文件: `log4j.properties`
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
log4j.appender.LOGFILE.File=xxx
log4j.appender.LOGFILE.Append=true
log4j.appender.LOGFILE.layout=org.apache.log4j.PatternLayout
log4j.appender.LOGFILE.layout.ConversionPattern=%d{ISO8601} %-6r [%15.15t] %-5p %30.30c %x - %m\n

```
* 数据库配置文件: `db.properties`
```xml
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/r1225?characterEncoding=utf-8
jdbc.username=root
jdbc.password=123456
```
* Spring配置文件: `applicationContext.xml`
这里数据库连接池需要交给Spring来管理, 同时SqlSessionFactory由`spring-mybatis`中的`org.mybatis.spring.SqlSessionFactoryBean`提供.
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
	http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
	http://www.springframework.org/schema/aop
	http://www.springframework.org/schema/aop/spring-aop-3.0.xsd
	http://www.springframework.org/schema/tx
	http://www.springframework.org/schema/tx/spring-tx-3.0.xsd
	http://www.springframework.org/schema/context
	http://www.springframework.org/schema/context/spring-context-3.0.xsd">


    <!--导入数据库文件-->
    <context:property-placeholder location="classpath:db.properties"/>

    <!-- 注解扫描 -->
    <context:component-scan base-package="com.coppco"/>

    <!--连接池 c3p0-->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"></property>
        <property name="jdbcUrl" value="${jdbc.url}"></property>
        <property name="user" value="${jdbc.username}"></property>
        <property name="password" value="${jdbc.password}"></property>
    </bean>

    <!-- 整合Mybatis会话工厂 -->
    <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
        <!--配置文件-->
        <property name="configLocation" value="classpath:SqlMapConfig.xml"/>
        <!--数据源-->
        <property name="dataSource" ref="dataSource"/>
        <!-- Sql映射文件, 也可以在MyBatis配置文件中加载, 使用了包扫描可以去掉映射文件的配置 -->
        <!--<property name="mapperLocations" value="classpath:com/coppco/**/*.xml" />-->
    </bean>
    
    <!--配置单个Mapper-->
    <!--<bean class="org.mybatis.spring.mapper.MapperFactoryBean" id="userMapper">
        <property name="mapperInterface" value="com.coppco.mapper.UserMapper"/>
        <property name="sqlSessionFactory" ref="sqlSessionFactory"/>
    </bean>-->
    

    <!--使用包扫描的方式配置Mapper, 引用的时候可以使用类名(首字母小写)-->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <!--多个包, 使用英文下的逗号隔开, -->
        <property name="basePackage" value="com.coppco.mapper, com.coppco.pojo"/>
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
    </bean>
</beans>
```
	* 如果使用了包扫描配置Mapper, 那么`sqlSessionFactory`中和`SqlMapConfig.xml`中都不用添加Sql配置文件的配置.
* Mybatis配置文件: `SqlMapConfig.xml`
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
    <!--别名-->
    <typeAliases>
        <!--包扫描-->
        <package name="com.coppco.pojo"/>
    </typeAliases>
    
    <mappers>
        <!--导入Sql映射文件-->
        <!--导入Sql映射文件: 单个 -->
        <!--<mapper resource="User.xml"/>-->

        <!--<package name="com.coppco.mapper"/>-->
    </mappers>
</configuration>

```
#### <font color=green> 三、编写pojo类、Mapper接口以及Sql映射文件 </font>
* pojo类: 普通java bean
* Mapper接口以及Sql映射文件需要满足上面说的Mapper接口的条件(同一个目录、名称一致), Maven项目还需注意sql配置文件不会打进包中.
* Mapper接口
```java
public interface UserMapper {
    public User fingUserById(Integer id);
}
```
* Sql映射文件
```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!-- 配置Mapper
     1、映射文件中 namespace 等于 接口的全路径名称
     2、映射文件中sql语句的id等于接口的方法名称
     3、映射文件中传入参数类型等于接口方法的传入参数类型
     4、映射文件中返回结果类型等于接口方法的返回结果类型
-->


<mapper namespace="com.coppco.mapper.UserMapper">
    <select id="fingUserById" parameterType="integer" resultType="user">
      select * from user where id = #{id}
    </select>
</mapper>
```
#### <font color=green> 四、测试类 </font>

```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext.xml")

public class MyTest {

    @Resource(name = "userMapper")
    private UserMapper userMapper;

    @Test
    public void testMapper() {
        User user = userMapper.fingUserById(1);
        System.out.println(user);
    }
}
```


## <font color=orange> MyBatis之逆向工程 </font>
使用MyBatis的逆向工程可以自动生成Mapper接口和Sql映射文件.

### <font color=orange> 1、导入相关jar包 </font>
* log4j-1.2.16.jar
* mybatis-3.2.3.jar
* mybatis-generator-core-1.3.2.jar
* mysql-connector-java-5.1.28.jar
### <font color=orange> 2、log4j.propertiesl </font>
```xml
log4j.rootLogger=DEBUG, Console
#Console
log4j.appender.Console=org.apache.log4j.ConsoleAppender
log4j.appender.Console.layout=org.apache.log4j.PatternLayout
log4j.appender.Console.layout.ConversionPattern=%d [%t] %-5p [%c] - %m%n
log4j.logger.java.sql.ResultSet=INFO
log4j.logger.org.apache=INFO
log4j.logger.java.sql.Connection=DEBUG
log4j.logger.java.sql.Statement=DEBUG
log4j.logger.java.sql.PreparedStatement=DEBUG
```
### <font color=orange> 3、配置generatorConfig.xml </font>
```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">
<generatorConfiguration>
    <context id="testTables" targetRuntime="MyBatis3">
        <commentGenerator>
            <!-- 是否去除自动生成的注释 true：是 ： false:否 -->
            <property name="suppressAllComments" value="true"/>
        </commentGenerator>
        <!--数据库连接的信息：驱动类、连接地址、用户名、密码 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/taotao" userId="root"
                        password="123456">
        </jdbcConnection>
        <!-- <jdbcConnection driverClass="oracle.jdbc.OracleDriver"
            connectionURL="jdbc:oracle:thin:@127.0.0.1:1521:yycg"
            userId="yycg"
            password="yycg">
        </jdbcConnection> -->
        <!-- 默认false，把JDBC DECIMAL 和 NUMERIC 类型解析为 Integer，为 true时把JDBC DECIMAL 和
            NUMERIC 类型解析为java.math.BigDecimal -->
        <javaTypeResolver>
            <property name="forceBigDecimals" value="false"/>
        </javaTypeResolver>
        <!-- targetProject:生成PO类的位置, targetProject: 存放生成文件的目录  -->
        <javaModelGenerator targetPackage="com.coppco.po"
                            targetProject="./src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
            <!-- 从数据库返回的值被清理前后的空格 -->
            <property name="trimStrings" value="true"/>
        </javaModelGenerator>
        <!-- targetProject:mapper映射文件生成的位置 -->
        <sqlMapGenerator targetPackage="com.coppco.mapper"
                         targetProject="./src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
        </sqlMapGenerator>
        <!-- targetPackage：mapper接口生成的位置 -->
        <javaClientGenerator type="XMLMAPPER"
                             targetPackage="com.coppco.mapper"
                             targetProject="./src/main/java">
            <!-- enableSubPackages:是否让schema作为包的后缀 -->
            <property name="enableSubPackages" value="false"/>
        </javaClientGenerator>
        <!-- 指定数据库表 -->
        <table schema="" tableName="user">
            //重新命名
            <columnOverride column="create_time" property="createTime" />
		 </table>
        <table schema="" tableName="tb_content_category"></table>
        <table schema="" tableName="tb_item"></table>
        <table schema="" tableName="tb_item_cat"></table>
        <table schema="" tableName="tb_item_desc"></table>
        <table schema="" tableName="tb_item_param"></table>
        <table schema="" tableName="tb_item_param_item"></table>
        <table schema="" tableName="tb_order"></table>
        <table schema="" tableName="tb_order_item"></table>
        <table schema="" tableName="tb_order_shipping"></table>
        <table schema="" tableName="tb_user"></table>
        <!-- 有些表的字段需要指定java类型
         <table schema="" tableName="">
            <columnOverride column="" javaType="" />
        </table> -->
    </context>
</generatorConfiguration>

```
### <font color=orange> 4、生成mapper文件和接口 </font>
```java
Public static void main(String[] args) throws Exception {
    try {
        List<String> warnings = new ArrayList<String>();
            boolean overwrite = true;
            File configFile = new File("generatorConfig.xml"); 
            ConfigurationParser cp = new ConfigurationParser(warnings);
            Configuration config = cp.parseConfiguration(configFile);
            DefaultShellCallback callback = new DefaultShellCallback(overwrite);
            MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,callback, warnings);
            myBatisGenerator.generate(null);
    } catch (Exception e) {
        e.printStackTrace();
    }
}
```
### <font color=orange> 注意事项 </font>
* Mapper文件内容不覆盖而是追加
	* mapper.xml文件存在时会追加在文件后面, 需要删除文件后重新运行
* Table schema问题
Schma即数据库模式，oracle中一个用户对应一个schema，可以理解为用户就是schema。
当Oralce数据库存在多个schema可以访问相同的表名时，使用mybatis生成该表的mapper.xml将会出现mapper.xml内容重复的问题，结果导致mybatis解析错误。
解决方法：在table中填写schema，如下：
`<table schema="XXXX" tableName=" " >`
XXXX即为一个schema的名称，生成后将mapper.xml的schema前缀批量去掉，如果不去掉当oracle用户变更了sql语句将查询失败。
快捷操作方式：mapper.xml文件中批量替换：“from XXXX.”为空

Oracle查询对象的schema可从dba_objects中查询，如下：
`select * from dba_objects`