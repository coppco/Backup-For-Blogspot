---
layout: post
title: Java Web学习11---JavaBean、MVC、事务
comments: true
date: 2016-09-29 09:42:02
tags:
	- Java
	- 事务
	- JavaBean
---

&emsp;&emsp;事务，一般是指要做的或所做的事情。在计算机术语中是指访问并可能更新数据库中各种数据项的一个程序执行单元。这些单元要么全都成功，要么全都不成功。

<!--more-->

## <font color=orange>JavaBean</font>
&emsp;&emsp;JavaBean 是一种Java语言写成的可重用组件。为写成JavaBean，类必须是具体的和公共的，并且具有无参数的构造器。JavaBean 通过提供符合一致性设计模式的公共方法将内部域暴露成员属性。众所周知，属性名称符合这种模式，其他Java 类可以通过自身机制发现和操作这些JavaBean 的属性。

* JSP中使用JavaBean
&emsp;&emsp;通过form标签,发送action到jsp文件时, 会封装成JavaBean对象, 使用如下: 页面上组件的名称必须与javaBean的bean属性名称一致.bean属性是get/set后面的名称.

```
	//使用JavaBean对象
	<jsp:useBean id="user" class="cn.itcast.doamin.User"></jsp:useBean>//全限类名
	//设置属性
	<jsp:setProperty property="name" name="user"/>
	//获取属性
	<jsp:getProtery property=’name’ name=’user’/>
```

## <font color=orange> BeanUtils </font>
&emsp;&emsp; BeanUtils 是Apache组织开发了一套用于操作JavaBean的API，这套API考虑到了很多实际开发中的应用场景，因此在实际开发中很多程序员使用这套API操作JavaBean，以简化程序代码的编写
* BeanUtils的使用
	* 1、导入jar包 `commons-logging-1.1.1.jar`和`commons-beanutils-1.8.3.jar`
	* 2、使用: `BeanUtils.populate(javaBean对象, request.getParameterMap());`
* BeanUtils工具类型转换
&emsp;&emsp;在BeanUtils工具中，有默认的类型转换，我们可以在org.apache.commons.beanutils.converters包下查看到它们提供的默认的类型转换器
	* 自定义类型转换
		* 1、创建一个类，实现这个接口org.apache.commons.beanutils.Converter 这个接口就是BeanUtils中所有类型转换器的根接口.
		* 2、重写方法: public Object convert(Class type, Object value) 在这个方法中完成类型转换. 这个方法的返回值就是赋值给javaBean中对应的属性.
			* type: 要转换成的类型		
			* value: 表单传递过来的属性值
		* 3、注册类型转换器: ConvertUtils.register(javaBean对象,要转换成的类型.class);


## <font color=orange> MVC </font>
&emsp;&emsp;MVC全名是Model View Controller，是模型(model)－视图(view)－控制器(controller)的缩写，是一种软件设计典范，用一种业务逻辑、数据、界面显示分离的方法组织代码，将业务逻辑聚集到一个部件里面，在改进和个性化定制界面及用户交互的同时，不需要重新编写业务逻辑。MVC被独特的发展起来用于映射传统的输入、处理和输出功能在一个逻辑的图形化用户界面的结构中。MVC模式最早被Trygve Reenskaug提出，成为施乐帕罗奥多研究中心（Xerox PARC）的Smalltalk语言发明的一种软件设计模式。MVC可对程序的后期维护和扩展提供了方便，并且使程序某些部分的重用提供了方便。而且MVC也使程序简化，更加直观。
&emsp;&emsp;需要注意的是MVC设计模式并不是java语言独有的设计模式，几乎所有的B/S结构的项目都在使用这种设计模式。其中M、V、C分别代表如下含义：
 * M：model层，即模型层，用来维护数据以及提供数据访问方法；
 * V：view层，即视图层，通常由jsp充当，用于展示模型的部分数据或所有数据的可视化视图；
 * C：controller层，即控制层，用于对处理请求。

 
## <font color=orange> 事务 </font>
* MySQL中默认是: 事务自动提交, 每一条SQL语句都是一个事务.
	* 登录MySQL之后运行: `show variables like '%commit%';`
	* `autocommint`值如果是`on`, 那么事务就是打开的.
	* 关闭事务自动提交
		* 方式1: 
			* 运行: `set autocommit = off;(或者set autocommit = 0;)`
		* 方式2: 
			* 运行: `start transaction;`
		* 关闭之后, 所有的SQL语句都在一个事务中, 需要手动管理事务.
	* <font color=orange>MySQL中手动事务处理</font>
		* 开启一个事务: `start transaction;`, <font color=red>一旦手动开启了事务,事务自动提交失效.</font>
		* 提交事务: `commit;`
		* 事务回滚: `rollback;`
* Orange中默认是: 手动事务提交.
* <font color=orange>Java中的事务</font>
	* Connection接口中的方法
		* 设置是否自动提交事务: `void setAutoCommit(boolean autoCommit) throws SQLException;`
		* 事务提交: `void commit() throws SQLException;`
		* 事务回滚: `void rollback() throws SQLException;`
		* 事务回滚到指定回滚点: `void rollback(Savepoint savepoint) throws SQLException;`
			* 设置回滚点: `Savepoint setSavepoint() throws SQLException;`
			* 设置回滚点并指定名称: `Savepoint setSavepoint(String name) throws SQLException;`
* <font color=orange>事务的特性</font>
	* Atomicity: 原子性
		* 表示事务中所有操作是不可再分割的原子单位。事务中所有操作要么全部执行成功，要么全部执行失败；
	* Consistency: 一致性
		* 事务执行后，数据库状态与其它业务规则保持一致。例如转账业务，无论事务执行成功与否，参与转账的两个账号余额之和应该是不变的；
	* Isolation: 隔离性
		* 在并发操作中，不同事务之间应该隔离开来，使每个并发中的事务不会相互干扰；
		* 不考虑隔离性可能导致的问题
			* 脏读: 在一个事务中读取到了另一个事务未提交的数据.
			* 不可重复读: 在一个事务中, 两次查询结果不一致.(针对于update操作)
			* 虚读(幻读): 在一个事务中, 两次查询结果不一致.(针对于insert操作)
	* Durability: 持久性
		* 一旦事务提交成功，事务中所有的数据操作都必须被持久化到数据库中，即使提交事务后，数据库马上崩溃，在数据库重启时，也必须能保证通过某种机制恢复数据。
* <font color=orange>事务的隔离级别,</font> <font color=red>MySQL默认是`Repeatable read`, Oranle默认是`Read committed`</font>
	* Serializable: 可避免脏读、不可重复读、虚读情况的发生。（串行化）
	* Repeatable read：可避免脏读、不可重复读情况的发生。（可重复读）不可以避免虚读
	* Read committed：可避免脏读情况发生（读已提交）
	* Read uncommitted：最低级别，以上情况均无法保证。(读未提交)
* <font color=orange>设置事务隔离级别</font>
	* MySQL中查看当前事务隔离级别
		* `select @@tx_isolation;`
	* MySQL中修改事务隔离级别
		* `set session transaction isolation level read uncommitted;`
	* JDBC中设置事务隔离级别 
		* java.sql.Connection接口中提供
			* `setTransactionIsolation(int level);`
			* 参数可以取 Connection 常量之一
				* Connection.TRANSACTION_READ_UNCOMMITTED
				* Connection.TRANSACTION_READ_COMMITTED
				* Connection.TRANSACTION_REPEATABLE_READ 
				* Connection.TRANSACTION_SERIALIZABLE
* <font color=orange>隔离级别对比</font>
	* 性能
		* Serializable &lt; Repeatable read &lt; read committed &lt; read uncommitted
	* 安全性
		* Serializable &gt; Repeatable read &gt; read committed &gt; read uncommitted
	* 实际开发时常用: `read committed` 或 `repeatable read`
* <font color=orange>ThreadLocal的使用</font>
&emsp;&emsp;该类提供了线程局部 (thread-local) 变量。这些变量不同于它们的普通对应物，因为访问某个变量（通过其 get 或 set 方法）的每个线程都有自己的局部变量，它独立于变量的初始化副本。ThreadLocal 实例通常是类中的 private static 字段，它们希望将状态与某一个线程（例如，用户 ID 或事务 ID）相关联。 它的底层是使用Map实现的.
当我们使用事务的时候, 需要开启事务、事务提交和事务回滚, 那么就需要在service层和dao层用到connection, 可以使用ThreadLocal类, 也可以使用向下传递connection.
	* 常用方法
		* 设置: `Object get()`
		* 获取: `set(Object obj)`
		* 移除: `remove()`
* <font color=orange>DBUtils中的事务</font>
	* QueryRunner类
		* 自动事务, 调用方法时不需要传入Connection, 资源不需我们自己释放. 
			* 构造方法: `new QueryRunner(DataSource ds)`
			* update方法: `update(String sql, Object params)`
		* 手动事务, 调用方法时需要传入Connection, 资源需要我们自己释放.
			* 构造方法: `new QueryRunner()` 
			* update方法: `update(Connection con, String sql, Object parmas)`
			* query方法: 