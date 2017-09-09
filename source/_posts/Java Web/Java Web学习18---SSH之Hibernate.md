---
layout: post
title: Java Web学习18---SSH之Hibernate
comments: true
date: 2017-03-30 11:42:54
tags:
	- Java
	- Hibernate
---
SSH（Struts，Spring，Hibernate） Struts进行流程控制，Spring进行业务流转，Hibernate进行数据库操作的封装。虽然Hibernate现在很少使用了, 但是可以学习一下它的思想.

<!--more-->

## <font color=orange> Hibernate </font>

&emsp;&emsp;Hibernate是一个开放源代码的对象关系映射框架，它对JDBC进行了非常轻量级的对象封装，使得Java程序员可以随心所欲的使用对象编程思维来操纵数据库。 Hibernate可以应用在任何使用JDBC的场合，既可以在Java的客户端程序使用，也可以在Servlet/JSP的Web应用中使用，最具革命意义的是，Hibernate可以在应用EJB的J2EE架构中取代CMP，完成数据持久化的重任。<font color=red>Hibernate是一个持久层的ORM框架.</font>

* ORM(对象关系映射)	
	* O: 面向对象领域的Object(JavaBean对象)
	* R: 关系数据库里面Relational(表的结构)
	* M: 映射Mapping(XML的配置文件)

* Hibernate的[官网](http://hibernate.org/)
* Hibernate的[下载地址](https://sourceforge.net/projects/hibernate/files/hibernate-orm/)

## <font color=orange> 使用Hibernate </font>
* 手动集成 Hibernate
	* 首先导入Hibernate的必须包
	* 创建数据库和表结构
	* 创建JavaBean类(基本数据类型要使用对应的包装类, 因为包装类默认值是null, 而基本数据类型默认值是0或0.0)
	* 编写映射的配置文件(一般和JavaBean类同一个包中, 名称一般是类名.hbm.xml)
```
<?xml version="1.0" encoding="UTF-8"?>
<!--约束-->
<!DOCTYPE hibernate-mapping PUBLIC
        "-//Hibernate/Hibernate Mapping DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-mapping-3.0.dtd">


<hibernate-mapping>
    <!--配置类和表的映射, name是JavaBean类名, table是表名-->
    <class name="com.coppco.Hibernate.Customer" table="cst_customer">
        <!--配置主键-->
        <!--name是JavaBean的属性, column是表的属性-->
        <id name="cust_id" column="cust_id">
            <!--主键的生成策略: 本地策略-->
            <generator class="native"/>
        </id>
        <!--其他属性-->
        <property name="cust_name" column="cust_name"/>
        <property name="cust_user_id" column="cust_user_id"/>
        <property name="cust_create_id" column="cust_create_id"/>
        <property name="cust_source" column="cust_source"/>
        <property name="cust_industry" column="cust_industry"/>
        <property name="cust_level" column="cust_level"/>
        <property name="cust_linkman" column="cust_linkman"/>
        <property name="cust_phone" column="cust_phone"/>
        <property name="cust_mobile" column="cust_mobile"/>
    </class>
</hibernate-mapping>
```

	* 编写`hibernate.cfg.xml`的核心配置文件(放在src下)
```
<?xml version="1.0" encoding="UTF-8" ?>
<!--hibernate核心配置文件-->
<!DOCTYPE hibernate-configuration PUBLIC
        "-//Hibernate/Hibernate Configuration DTD 3.0//EN"
        "http://www.hibernate.org/dtd/hibernate-configuration-3.0.dtd">

<hibernate-configuration>
    <session-factory>
        <!--必须配置数据库参数和数据库方言-->
        <!--数据库参数-->
        <property name="hibernate.connection.driver_class">com.mysql.jdbc.Driver</property>
        <property name="hibernate.connection.url">jdbc:mysql://192.168.1.154:3306/hibernate_day01</property>
        <property name="hibernate.connection.username">root</property>
        <property name="hibernate.connection.password">123456</property>

        <!--数据库方言-->
        <property name="hibernate.dialect ">org.hibernate.dialect.MySQLDialect</property>

        <!--可配参数-->
        <!--控制台显示SQL语句-->
        <property name="hibernate.show_sql">true</property>
        
        <!--格式化SQL语句-->
        <property name="hibernate.format_sql">true</property>
        
        <!--生成数据库表结构, update: 没有表会创建表,当我们的JavaBean属性改变时, 会为我们更新表结构, valilate: 会做校验, 开发一般使用update-->
        <property name="hibernate.hbm2ddl.auto">update</property>
        
        <!--映射配置文件-->
        <mapping resource="com/coppco/Hibernate/Customer.hbm.xml"></mapping>
    </session-factory>
</hibernate-configuration>
```
	* 测试
```
        //1. 加载配置文件
        Configuration config = new Configuration();
        //默认加载src/Hibernate.cfg.xml文件
        config.configure();

        //手动加载 映射配置文件(使用properties文件或者没有配置mapping的xml)
        config.addResource("com/coppco/Hibernate/Customer.hbm.xml");
			
        //2. 创建sessionFactory
        SessionFactory sf = config.buildSessionFactory();

        //3. session对象
        Session session = sf.openSession();

        //4. 开启事务
        Transaction trandsation = session.beginTransaction();

        //5. 保存
        Customer customer = new Customer();
        customer.setCust_industry("101");
        customer.setCust_name("测试");

        session.save(customer);

        //6. 提交
        trandsation.commit();

        //7. 释放
        session.close();
        sf.close();
```



### <font color=orange> Hibernate配置文件之映射配置文件 </font>
	
* 映射文件，即Stu.hbm.xml的配置文件
	* <class>标签		-- 用来将类与数据库表建立映射关系
		* name			-- 类的全路径
		* table			-- 表名.(类名与表名一致,那么table属性也可以省略)
		* catalog		-- 数据库的名称，基本上都会省略不写
		
	* <id>标签			-- 用来将类中的属性与表中的主键建立映射，id标签就是用来配置主键的。
		* name			-- 类中属性名
		* column 		-- 表中的字段名.(如果类中的属性名与表中的字段名一致,那么column可以省略.)
		* length		-- 字段的程度，如果数据库已经创建好了，那么length可以不写。如果没有创建好，生成表结构时，length最好指定。
		
	* <property>		-- 用来将类中的普通属性与表中的字段建立映射.
		* name			-- 类中属性名
		* column		-- 表中的字段名.(如果类中的属性名与表中的字段名一致,那么column可以省略.)
		* length		-- 数据长度
		* type			-- 数据类型（一般都不需要编写，如果写需要按着规则来编写）
			* Hibernate的数据类型	type="string"
			* Java的数据类型		type="java.lang.String"
			* 数据库字段的数据类型	<column name="name" sql-type="varchar"/>



### <font color=orange>Hibernate配置文件之核心配置文件</font>
	
* 核心配置文件的两种方式
	* 第一种方式是属性文件的形式，即properties的配置文件
		* hibernate.properties
			* hibernate.connection.driver_class=com.mysql.jdbc.Driver
		* 缺点
			* 不能加载映射的配置文件，需要手动编写代码去加载
		
	* 第二种方式是XML文件的形式，开发基本都会选择这种方式<font color=red>(推荐)</font>
		* hibernate.cfg.xml
			* <property name="hibernate.connection.driver_class" >com.mysql.jdbc.Driver</property>
		* 优点
			* 格式比较清晰
			* 编写有提示
			* 可以在该配置文件中加载映射的配置文件（最主要的）
	
*  关于hibernate.cfg.xml的配置文件方式
	* 必须有的配置
		* 数据库连接信息:
			* hibernate.connection.driver_class  			-- 连接数据库驱动程序
			* hibernate.connection.url   					-- 连接数据库URL
			* hibernate.connection.username  				-- 数据库用户名
			* hibernate.connection.password   			-- 数据库密码
		* 方言: hibernate.dialect   						-- 操作数据库方言
	* 可选的配置
		* hibernate.show_sql							-- 显示SQL
		* hibernate.format_sql							-- 格式化SQL
		* hibernate.hbm2ddl.auto						-- 通过映射转成DDL语句
			* create				-- 每次都会创建一个新的表.---测试的时候
			* create-drop			-- 每次都会创建一个新的表,当执行结束之后,将创建的这个表删除.---测试的时候
			* update				-- 如果有表,使用原来的表.没有表,创建一个新的表.同时更新表结构.
			* validate				-- 如果有表,使用原来的表.同时校验映射文件与表中字段是否一致如果不一致就会报错.
	* 加载映射
		* 如果XML方式：`<mapping resource="cn/itcast/hibernate/domain/User.hbm.xml" />`

### <font color=orange>Hibernate常用的接口和类 </font>

* Configuration类和作用
	1. Configuration类
		* Configuration对象用于配置并且启动Hibernate。
		* Hibernate应用通过该对象来获得对象-关系映射文件中的元数据，以及动态配置Hibernate的属性，然后创建SessionFactory对象。
		
		* 简单一句话：加载Hibernate的配置文件，可以获取SessionFactory对象。
	2. Configuration类的其他应用（了解）
		* 加载配置文件的种类，Hibernate支持xml和properties类型的配置文件，在开发中基本都使用XML配置文件的方式。
			* 如果采用的是properties的配置文件，那么通过Configuration configuration = new Configuration();就可以假装配置文件
				* 但是需要自己手动加载映射文件
				* 例如：config.addResource("cn/itcast/domain/Student.hbm.xml");
			
			* 如果采用的XML的配置文件，通过Configuration configuration = new Configuration().configure();加载配置文件
	
* SessionFactory(重要)
	1. 是工厂类，是生成Session对象的工厂类
	2. SessionFactory类的特点
		* 由Configuration通过加载配置文件创建该对象。
		* SessionFactory对象中保存了当前的数据库配置信息和所有映射关系以及预定义的SQL语句。同时，SessionFactory还负责维护Hibernate的二级缓存。
			* 预定义SQL语句
				* 使用Configuration类创建了SessionFactory对象是，已经在SessionFacotry对象中缓存了一些SQL语句
				* 常见的SQL语句是增删改查（通过主键来查询）
				* 这样做的目的是效率更高
		
		* 一个SessionFactory实例对应一个数据库，应用从该对象中获得Session实例。
		* SessionFactory是线程安全的，意味着它的一个实例可以被应用的多个线程共享。
		* SessionFactory是重量级的，意味着不能随意创建或销毁它的实例。如果只访问一个数据库，只需要创建一个SessionFactory实例，且在应用初始化的时候完成。
		* SessionFactory需要一个较大的缓存，用来存放预定义的SQL语句及实体的映射信息。另外可以配置一个缓存插件，这个插件被称之为Hibernate的二级缓存，被多线程所共享
	3. 总结
		* 一般应用使用一个SessionFactory,最好是应用启动时就完成初始化。
	
	
* 编写HibernateUtil的工具类
	1. 具体代码如下
```
		public class HibernateUtil {
			private static final Configuration cfg;
			private static final SessionFactory factory;
			static{
				// 给常量赋值 
				// 加载配置文件
				cfg = new Configuration().configure();
				// 生成factory对象
				factory = cfg.buildSessionFactory();
			}
			// 获取Session对象
			public static Session openSession(){
				return factory.openSession();
			}
		}
	
```
	
* Session接口
	1. 概述
		* Session是在Hibernate中使用最频繁的接口。也被称之为持久化管理器。它提供了和持久化有关的操作，比如添加、修改、删除、加载和查询实体对象
		* Session 是应用程序与数据库之间交互操作的一个单线程对象，是 Hibernate 运作的中心
		* Session是线程不安全的
		* 所有持久化对象必须在 session 的管理下才可以进行持久化操作
		* Session 对象有一个一级缓存，显式执行 flush 之前，所有的持久化操作的数据都缓存在 session 对象处
		* 持久化类与 Session 关联起来后就具有了持久化的能力
	
	2. 特点
		* 不是线程安全的。应避免多个线程使用同一个Session实例
		* Session是轻量级的，它的创建和销毁不会消耗太多的资源。应为每次客户请求分配独立的Session实例
		* Session有一个缓存，被称之为Hibernate的一级缓存。每个Session实例都有自己的缓存
	
	3. 常用的方法
		* save(obj)					-- 保存数据
		* delete(obj)					-- 删除数据(需要先查询)  
		* get(Class,id)					-- 只能查询一条语句(通过主键)
		* update(obj)				-- 更新数据(需要先查询) 
		* saveOrUpdate(obj)					-- 保存或者修改（如果没有数据，保存数据。如果有，修改数据）
		* createQuery() 					-- HQL语句的查询的方式
	
* Transaction接口
	1. Transaction是事务的接口
	2. 常用的方法
		* commit()				-- 提交事务
		* rollback()			-- 回滚事务
	
	3. 特点
		* Hibernate框架默认情况下事务不自动提交.需要手动提交事务
		* 如果没有开启事务，那么每个Session的操作，都相当于一个独立的事务

		
### <font color=orange>Hibernate的持久化类</font>

	
* 持久化类:就是一个Java类（咱们编写的JavaBean），这个Java类与表建立了映射关系就可以成为是持久化类。
	* 持久化类 = JavaBean + xxx.hbm.xml
	* 编写规则
		1. 提供一个无参数 public访问控制符的构造器				-- 底层需要进行反射.
		2. 提供一个标识属性，映射数据表主键字段					-- 唯一标识OID.数据库中通过主键.Java对象通过地址确定对象.持久化类通过唯一标识OID确定记录
		3. 所有属性提供public访问控制符的 set或者get 方法
		4. 标识属性应尽量使用基本数据类型的包装类型

### <font color=orange>自然主键、代理主键和主键城生成策略<font>
* 创建表的时候
	* 自然主键:对象本身的一个属性.创建一个人员表,每个人都有一个身份证号.(唯一的)使用身份证号作为表的主键.自然主键.（开发中不会使用这种方式）
	* 代理主键:不是对象本身的一个属性.创建一个人员表,为每个人员单独创建一个字段.用这个字段作为主键.代理主键.（开发中推荐使用这种方式）
* 创建表的时候尽量使用代理主键创建表
* Hibernate中主键策略
	* increment: 适用于short、int和long类型的主键,但是不是数据库自动增长机制.先查询后设置`结果+1`作为主键.(并发时可能出现问题)
	* identity: 适用于short、int和long类型的主键, 底层是使用数据库的自动增长机制.(ORACLE没有自动增长)
	* sequence: 适用于short、int和long类型的主键, 底层使用的序列的增长方式.
	* uuid: 适用于使用char、varchar类型作为主键.
	* native: 本地策略, 根据数据库的不同, 使用不同的策略(使用较多)
		* 如果使用MySQL, 使用`identity`
		* 如果使用ORACLE, 使用`sequence`
	* assigned: 主键的生成不用Hibernate管理, 必须手动设置主键.

### <font color=orange>持久化对象的状态</font>
	
* Hibernate的持久化类
	* 持久化类:Java类与数据库的某个表建立了映射关系.这个类就称为是持久化类.
		* 持久化类 = Java类 + hbm的配置文件
	
* Hibernate的持久化类的状态
	* Hibernate为了管理持久化类：将持久化类分成了三个状态
		* 瞬时态:Transient  Object
			* 没有持久化标识OID, 没有被纳入到Session对象的管理.
			
		* 持久态:Persistent Object
			* 有持久化标识OID,已经被纳入到Session对象的管理.
			
		* 脱管态:Detached Object
			* 有持久化标识OID,没有被纳入到Session对象的管理.

	
### <font color=orange>Hibernate持久化对象的状态的转换</font>
	
* 瞬时态	-- 没有持久化标识OID, 没有被纳入到Session对象的管理
	* 获得瞬时态的对象
		* User user = new User()
	* 瞬时态对象转换持久态
		* save()/saveOrUpdate();
	* 瞬时态对象转换成脱管态
		* user.setId(1)
	
* 持久态	-- 有持久化标识OID,已经被纳入到Session对象的管理
	* 获得持久态的对象
		* get()/load();
	* 持久态转换成瞬时态对象
		* delete();  --- 比较有争议的，进入特殊的状态(删除态:Hibernate中不建议使用的)
	* 持久态对象转成脱管态对象
		* session的close()/evict()/clear();
	
* 脱管态	-- 有持久化标识OID,没有被纳入到Session对象的管理
	* 获得托管态对象:不建议直接获得脱管态的对象.
		* User user = new User();
		* user.setId(1);
	* 脱管态对象转换成持久态对象
		* update();/saveOrUpdate()/lock();
	* 脱管态对象转换成瞬时态对象
		* user.setId(null);
* 注意：持久态对象有自动更新数据库的能力!!!

### <font color=orange>Hibernate的一级缓存</font>

* 什么是缓存？
	* 其实就是一块内存空间,将数据源（数据库或者文件）中的数据存放到缓存中.再次获取的时候 ,直接从缓存中获取.可以提升程序的性能！
	
* Hibernate框架提供了两种缓存
	* 一级缓存	-- 自带的不可卸载的.一级缓存的生命周期与session一致.一级缓存称为session级别的缓存.
	* 二级缓存	-- 默认没有开启，需要手动配置才可以使用的.二级缓存可以在多个session中共享数据,二级缓存称为是sessionFactory级别的缓存.
	
* Session对象的缓存概述
	* Session接口中,有一系列的java的集合,这些java集合构成了Session级别的缓存(一级缓存).将对象存入到一级缓存中,session没有结束生命周期,那么对象在session中存放着
	* 内存中包含Session实例 --> Session的缓存（一些集合） --> 集合中包含的是缓存对象！
	
* 证明一级缓存的存在，编写查询的代码即可证明
	* 在同一个Session对象中两次查询，可以证明使用了缓存
	
* Hibernate框架是如何做到数据发生变化时进行同步操作的呢？
	* 使用get方法查询User对象
	* 然后设置User对象的一个属性，注意：没有做update操作。发现，数据库中的记录也改变了。
	* 利用快照机制来完成的（SnapShot）

### <font color=orange>事务相关的概念</font>
	
* 什么是事务
	* 事务就是逻辑上的一组操作，组成事务的各个执行单元，操作要么全都成功，要么全都失败.
	* 转账的例子：冠希给美美转钱，扣钱，加钱。两个操作组成了一个事情！
	
* 事务的特性
	* 原子性	-- 事务不可分割.
	* 一致性	-- 事务执行的前后数据的完整性保持一致.
	* 隔离性	-- 一个事务执行的过程中,不应该受到其他的事务的干扰.
	* 持久性	-- 事务一旦提交,数据就永久保持到数据库中.
	
* 如果不考虑隔离性:引发一些读的问题
	* 脏读			-- 一个事务读到了另一个事务未提交的数据.
	* 不可重复读	-- 一个事务读到了另一个事务已经提交的update数据,导致多次查询结果不一致.
	* 虚读			-- 一个事务读到了另一个事务已经提交的insert数据,导致多次查询结构不一致.
	
* 通过设置数据库的隔离级别来解决上述读的问题
	* 未提交读:以上的读的问题都有可能发生.
	* 已提交读:避免脏读,但是不可重复读，虚读都有可能发生.
	* 可重复读:避免脏读，不可重复读.但是虚读是有可能发生.
	* 串行化:以上读的情况都可以避免.
	
* 如果想在Hibernate的框架中来设置隔离级别，需要在hibernate.cfg.xml的配置文件中通过标签来配置
	* 通过：hibernate.connection.isolation = 4 来配置
	* 取值
		* 1: Read uncommitted isolation
		* 2: Read committed isolation
		* 4: Repeatable read isolation
		* 8: Serializable isolation

### <font color=orange>绑定本地的Session</font>
	
* 之前在讲JavaWEB的事务的时候，需要在业务层使用Connection来开启事务，
	* 一种是通过参数的方式传递下去
	* 另一种是把Connection绑定到ThreadLocal对象中
	
* 现在的Hibernate框架中，使用session对象开启事务，所以需要来传递session对象，框架提供了ThreadLocal的方式
	* 需要在hibernate.cfg.xml的配置文件中提供配置
		* `<property name="hibernate.current_session_context_class">thread</property>`
		
	* 重新HibernateUtil的工具类，使用SessionFactory的getCurrentSession()方法，获取当前的Session对象。并且该Session对象不用手动关闭，线程结束了，会自动关闭。
```
			public static Session getCurrentSession(){
				return factory.getCurrentSession();
			}
```
	* 注意：想使用getCurrentSession()方法，必须要先配置才能使用。

### <font color=orange>Hibernate框架的查询方式</font>


* Query查询接口
	* 具体的查询代码如下
```
		// 1.查询所有记录
		/*Query query = session.createQuery("from Customer");
		List<Customer> list = query.list();
		System.out.println(list);*/
		
		// 2.条件查询:
		/*Query query = session.createQuery("from Customer where name = ?");
		query.setString(0, "李健");
		List<Customer> list = query.list();
		System.out.println(list);*/
		
		// 3.条件查询:
		/*Query query = session.createQuery("from Customer where name = :aaa and age = :bbb");
		query.setString("aaa", "李健");
		query.setInteger("bbb", 38);
		List<Customer> list = query.list();
		System.out.println(list);*/
	
```
	
* Criteria查询接口（做条件查询非常合适)
	* 具体的查询代码如下
```
		// 1.查询所有记录
		/*Criteria criteria = session.createCriteria(Customer.class);
		List<Customer> list = criteria.list();
		System.out.println(list);*/
		
		// 2.条件查询
		/*Criteria criteria = session.createCriteria(Customer.class);
		criteria.add(Restrictions.eq("name", "李健"));
		List<Customer> list = criteria.list();
		System.out.println(list);*/
		
		// 3.条件查询
		/*Criteria criteria = session.createCriteria(Customer.class);
		criteria.add(Restrictions.eq("name", "李健"));
		criteria.add(Restrictions.eq("age", 38));
		List<Customer> list = criteria.list();
		System.out.println(list);*/
```

## <font color=orange>Hibernate的关联关系映射之一对多映射</font>

* JavaBean的编写
	* 一表中: 新建一个Set属性, 并且需要初始化
```
private Set<Linkman> linkmans = new HashSet<>(); 
```
	* 多表中: 需要新建一个一表的属性, 不能初始化.
```
private Customer customer;
```
* 配置文件的设置
	* 多方配置文件中: 
```
<!--配置多方: name是当前的JavaBean中的属性, class是当前属性的全路径, column: 外键字段-->
<many-to-one name="customer" class="com.coppco.domain.Customer" column="lkm_cust_id"></many-to-one>
```
	* 一方配置文件中:
```
 <!--配置一方-->
        <set name="linkmans">
            <!--外键字段-->
            <key column="lkm_cust_id"></key>
            <!--set里面的类型的全路径-->
            <one-to-many class="com.coppco.domain.Linkman" />
        </set>
```
* 保存数据
	* 双向关联保存
```
c.getLinkmans().add(l1);
c.getLinkmans().add(l2);

l1.setCustomer(c);
l2.setCustomer(c);
```
	* 级联保存数据, 只保存一方数据, 会把相关数据都保存到数据库, 那么就需要配置级联, 需要在一方、多方(可以只写一个, 也可以都写)中使用`cascade`.
```
<!--配置一方-->
<set name="linkmans" cascade="save-update">
	<!--外键字段-->
	<key column="lkm_cust_id" />
	<!--set里面的类型的全路径-->
	<one-to-many class="com.coppco.domain.Linkman" />
</set>

<!--配置多方: name是当前的JavaBean中的s属性, 当前属性的全路径, column: 外键字段-->
        <many-to-one name="customer" class="com.coppco.domain.Customer" column="lkm_cust_id" cascade="save-update" />
```
	* 级联删除数据, 可以删除相关的数据(很少使用, 一般使用逻辑删除)
```
<!--配置一方-->
<set name="linkmans" cascade="save-update,delete">
	<!--外键字段-->
	<key column="lkm_cust_id" />
	<!--set里面的类型的全路径-->
	<one-to-many class="com.coppco.domain.Linkman" />
</set>

<!--配置多方: name是当前的JavaBean中的s属性, 当前属性的全路径, column: 外键字段-->
        <many-to-one name="customer" class="com.coppco.domain.Customer" column="lkm_cust_id" cascade="save-update,delete" />
```	
	* 一对多时, 让一方放弃主键维护, 减少SQL语句. 添加`interse="true"`, true是放弃主键维护.
```
<!--配置一方-->
<set name="linkmans" cascade="save-update" inverse="true">
	<!--外键字段-->
	<key column="lkm_cust_id" />
	<!--set里面的类型的全路径-->
	<one-to-many class="com.coppco.domain.Linkman" />
</set>
```

## <font color=orange>Hibernate的关联关系映射之多对多映射</font>

* JavaBean的编写: 
	* 两个多表中, 需要写一个`Set`集合并初始化.
* 配置文件的编写
```
<!--name是JavaBean中Set集合的属性名, table是中间表名称, -->
<set name="roles" table="sys_user_role">
	<!--column是当前对象在中间表的外键名称-->
	<key column="uid" /> 
	<!--class是Set中存放对象的全路径, column是集合对象在中间表中的外键名称-->
	<many-to-many class="" column="" />
```
* 级联保存、删除和放弃主键维护: 和一对多表一样



## <font color=orange> Hibernate框架的查询方式 </font>

* 唯一标识OID的检索方式
	* session.get(对象.class,OID);
* 对象的导航的方式
	* 通俗点就是可以一直使用`.`来获取属性
* HQL的检索方式
	* Hibernate Query Language	-- Hibernate的查询语言
	* HQL基本的查询格式
		* `session.createQuery("from Customer").list();`
	* 使用别名的方式
		* `ession.createQuery("from Customer c").list();`
		* `session.createQuery("select c from Customer c").list();`
	* 排序查询: 和SQL中一致
	* 分页查询
		* setFirstResult(a)		-- 从哪条记录开始，如果查询是从第一条开启，值是0
		* setMaxResults(b)		-- 每页查询的记录条数
	* 带条件的查询
		* setParameter("?号的位置，默认从0开始","参数的值"); 不用考虑参数的具体类型
		* 按位置绑定参数的条件查询（指定下标值，默认从0开始）
		* 按名称绑定参数的条件查询（HQL语句中的 ? 号换成 :名称 的方式）
	* HQL的投影查询: 查询某一字段的值或者某几个字段的值
		* `List<Object[]> list = session.createQuery("select c.cust_name,c.cust_level from Customer c").list();`
		* `List<Customer> list = session.createQuery("select new Customer(c.cust_name,c.cust_level) from Customer c").list();`
			* 直接把查询的数据封装成对象
			* 前提是: 该对象提供该数据的构造方法
	* 聚合函数查询
		* `List<Number> list = session.createQuery("select count(c) from Customer c").list();`
		* `List<Number> list = session.createQuery("select sum(c.cust_id) from Customer c").list();`
* QBC的检索方式
	* Query By Criteria	-- 条件查询
	* 简单查询
		* `List<Customer> list = session.createCriteria(Customer.class).list();`
	* 排序查询
		* `criteria.addOrder(Order.desc("lkm_id"));`
	* 分页查询
		* setFirstResult(a)		-- 从哪条记录开始，如果查询是从第一条开启，值是0
		* setMaxResults(b)		-- 每页查询的记录条数
	* 条件查询
		* 条件查询使用Criteria接口的add方法，用来传入条件
			* Restrictions.eq			-- 相等
			* Restrictions.gt			-- 大于号
			* Restrictions.ge			-- 大于等于
			* Restrictions.lt			-- 小于
			* Restrictions.le			-- 小于等于
			* Restrictions.between		-- 在之间
			* Restrictions.like			-- 模糊查询
			* Restrictions.in			-- 范围
			* Restrictions.and			-- 并且
			* Restrictions.or			-- 或者
	* 聚合函数查询
		* `criteria.setProjection(Projections.rowCount());`
	* 离线条件查询
		* 离线条件查询使用的是DetachedCriteria接口进行查询，离线条件查询对象在创建的时候，不需要使用Session对象，只是在查询的时候使用Session对象即可。
		* `DetachedCriteria criteria = DetachedCriteria.forClass(Linkman.class);`
* SQL检索方式, 使用SQL语句来进行查询
	* `List<Customer> list = session.createQuery("from Customer c left join fetch c.linkmans").list();`

	
### <font color=orange>Hibernate的延迟加载</font>

* 延迟加载先获取到代理对象，当真正使用到该对象中的属性的时候，才会发送SQL语句，是Hibernate框架提升性能的方式
* 类级别的延迟加载
	* Session对象的load方法默认就是延迟加载
	* 使类级别的延迟加载失效
		* 在<class>标签上配置lazy=”false”
		* Hibernate.initialize(Object proxy);
* 关联级别默认是延迟加载