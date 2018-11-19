---
layout: post
title: 消息队列(MQ)的使用
comments: true
toc: true
date: 2017-06-23 13:49:48
tags:
	- Java
	- MQ
---

<img src="http://47.96.147.179/images/java/MQ_header.jpg" alt="hello" style="width: 80%; text-align: center; display: block; margin-top:0px; margin-bottom:0px"/>

## <font color=orange>什么是MQ</font>
消息队列（MQ）是一种应用程序对应用程序的通信方法。应用程序通过写和检索出入列队的针对应用程序的数据（消息）来通信，而无需专用连接来链接它们。消息传递指的是程序之间通过在消息中发送数据进行通信，而不是通过直接调用彼此来通信，直接调用通常是用于诸如远程过程调用的技术。排队指的是应用程序通过队列来通信。队列的使用除去了接收和发送应用程序同时执行的要求。

MQ是一个消息中间件，ActiveMQ、RabbitMQ、kafka
<!--more-->

## <font color=orange> ActiveMQ </font>
### <font color=orange> 什么是ActiveMQ </font>

ActiveMQ 是Apache出品，最流行的，能力强劲的开源消息总线。ActiveMQ 是一个完全支持JMS1.1和J2EE 1.4规范的 JMS Provider实现,尽管JMS规范出台已经是很久的事情了,但是JMS在当今的J2EE应用中间仍然扮演着特殊的地位。
* 主要特点：
	* 1、多种语言和协议编写客户端。语言: Java, C, C++, C#, Ruby, Perl, Python, PHP。应用协议: OpenWire,Stomp REST,WS Notification,XMPP,AMQP
	* 2、完全支持JMS1.1和J2EE 1.4规范 (持久化,XA消息,事务)
	* 3、对Spring的支持,ActiveMQ可以很容易内嵌到使用Spring的系统里面去,而且也支持Spring2.0的特性
	* 4、通过了常见J2EE服务器(如 Geronimo,JBoss 4, GlassFish,WebLogic)的测试,其中通过JCA 1.5 resource adaptors的配置,可以让ActiveMQ可以自动的部署到任何兼容J2EE 1.4 商业服务器上
	* 5、支持多种传送协议:in-VM,TCP,SSL,NIO,UDP,JGroups,JXTA
	* 6、支持通过JDBC和journal提供高速的消息持久化
	* 7、从设计上保证了高性能的集群,客户端-服务器,点对点
	* 8、支持Ajax
	* 9、支持与Axis的整合
	* 10、可以很容易得调用内嵌JMS provider,进行测试
	
### <font color=orange> 什么情况下使用ActiveMQ? </font>
* 多个项目之间集成 
	* 跨平台 
	* 多语言 
	* 多项目
* 降低系统间模块的耦合度，解耦 
	* 软件扩展性
* 系统前后端隔离 
	* 前后端隔离，屏蔽高安全区
	
### <font color=orange> ActiveMQ的消息形式 </font>
* 消息的传递有两种类型：
	* 一种是点对点的，即一个生产者和一个消费者一一对应；(一对一)
	* 另一种是发布/订阅模式，即一个生产者产生消息并进行发送后，可以由多个消费者进行接收。(广播)

* JMS定义了五种不同的消息正文格式，以及调用的消息类型，允许你发送并接收以一些不同形式的数据，提供现有消息格式的一些级别的兼容性。
	* StreamMessage -- Java原始值的数据流
	* MapMessage--一套名称-值对
	* TextMessage--一个字符串对象<font color=red>(使用的最多)</font>
	* ObjectMessage--一个序列化的 Java对象
	* BytesMessage--一个字节的数据流

## <font color=orange> ActiveMQ的安装 </font>
* [官网下载地址](http://activemq.apache.org)
* [其他镜像站下载](/2016/08/08/资料整理/国内镜像站mirror整理)

解压压缩文件, 进入`/bin`目录, 
* 运行: `./activemq start`
* 关闭: `./activemq stop`
* 查询状态: `./activemq status`
如果服务器安装了防火墙请开放`8161`端口, 该端口是ActiveMQ默认的管理端口.
然后浏览器打开`http://服务器ip地址:8161/admin`, 用户名和密码默认是`admin`
<center>
<img src="http://47.96.147.179/images/java/activeMQ.png" alt="hello" style="width: 100%; text-align: center; display: block; margin-top:0px; margin-bottom:0px"/>
</center>

## <font color=orange> ActiveMQ的使用 </font>
ActiveMQ的Maven依赖: 
```xml
<dependency>
    <groupId>org.apache.activemq</groupId>
    <artifactId>activemq-all</artifactId>
    <version>5.11.2</version>
</dependency>
```
### <font color=orange> Queue </font>
默认会持久化到服务端
#### <font color=orange> Producer: 消息发送者 </font>
* 1、创建ConnectionFactory对象，需要指定服务端ip及端口号, 默认消息服务端口`61616`, 有防火墙请开放此端口。
* 2、使用ConnectionFactory对象创建一个Connection对象。
* 3、开启连接，调用Connection对象的start方法。
* 4、使用Connection对象创建一个Session对象。
* 5、使用Session对象创建一个Destination对象（topic、queue），此处创建一个Queue对象。
* 6、使用Session对象创建一个Producer对象。
* 7、创建一个Message对象，创建一个TextMessage对象。
* 8、使用Producer对象发送消息。
* 9、关闭资源。

```java
package com.test;

import org.apache.activemq.ActiveMQConnectionFactory;
import org.junit.Test;

import javax.jms.*;

public class TestActiveMQ {
    /**
     * 测试activeMQ消息发送者
     * @throws Exception
     */
    @Test
    public void testQueueProducer() throws Exception {
        // 第一步：创建ConnectionFactory对象，需要指定服务端ip及端口号。
        //brokerURL服务器的ip及端口号
        ConnectionFactory connectionFactory = new ActiveMQConnectionFactory("tcp://192.168.1.184:61616");
        // 第二步：使用ConnectionFactory对象创建一个Connection对象。
        Connection connection = connectionFactory.createConnection();
        // 第三步：开启连接，调用Connection对象的start方法。
        connection.start();
        // 第四步：使用Connection对象创建一个Session对象。
        //第一个参数：是否开启事务。true：开启事务，第二个参数忽略。一般不是事务, 可以使用消息队列保证数据的一致性.
        //第二个参数：当第一个参数为false时，才有意义。消息的应答模式。1、自动应答2、手动应答。一般是自动应答。
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        // 第五步：使用Session对象创建一个Destination对象（topic、queue），此处创建一个Queue对象。
        //参数：队列的名称。
        Queue queue = session.createQueue("test-queue");
        // 第六步：使用Session对象创建一个Producer对象。
        MessageProducer producer = session.createProducer(queue);
        // 第七步：创建一个Message对象，创建一个TextMessage对象。
        /*TextMessage message = new ActiveMQTextMessage();
		message.setText("hello activeMq,this is my first test.");*/
        TextMessage textMessage = session.createTextMessage("hello activeMq,this is my first test.");
        // 第八步：使用Producer对象发送消息。
        producer.send(textMessage);
        // 第九步：关闭资源。
        producer.close();
        session.close();
        connection.close();
    }
}
```

#### <font color=orange> Consumer: 消息接受者 </font>
* 1、创建一个ConnectionFactory对象。
* 2、从ConnectionFactory对象中获得一个Connection对象。
* 3、开启连接。调用Connection对象的start方法。
* 4、使用Connection对象创建一个Session对象。
* 5、使用Session对象创建一个Destination对象。和发送端保持一致queue，并且队列的名称一致。
* 6、使用Session对象创建一个Consumer对象。
* 7、接收消息。
* 8、打印消息。
* 9、关闭资源

```java
package com.test;

import org.apache.activemq.ActiveMQConnectionFactory;
import org.junit.Test;

import javax.jms.*;

public class TestActiveMQ {

    /**
     * 测试activeMQ消息接受者
     * @throws Exception
     */
    @Test
    public void testQueueConsumer() throws Exception {
        // 第一步：创建一个ConnectionFactory对象。
        ConnectionFactory connectionFactory = new ActiveMQConnectionFactory("tcp://192.168.1.184:61616");
        // 第二步：从ConnectionFactory对象中获得一个Connection对象。
        Connection connection = connectionFactory.createConnection();
        // 第三步：开启连接。调用Connection对象的start方法。
        connection.start();
        // 第四步：使用Connection对象创建一个Session对象。
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        // 第五步：使用Session对象创建一个Destination对象。和发送端保持一致queue，并且队列的名称一致。
        Queue queue = session.createQueue("test-queue");
        // 第六步：使用Session对象创建一个Consumer对象。
        MessageConsumer consumer = session.createConsumer(queue);
        // 第七步：接收消息。
        consumer.setMessageListener(new MessageListener() {

            @Override
            public void onMessage(Message message) {
                try {
                    TextMessage textMessage = (TextMessage) message;
                    String text = null;
                    //取消息的内容
                    text = textMessage.getText();
                    // 第八步：打印消息。
                    System.out.println(text);
                } catch (JMSException e) {
                    e.printStackTrace();
                }
            }
        });
        //等待键盘输入
        System.in.read();
        // 第九步：关闭资源
        consumer.close();
        session.close();
        connection.close();
    }
}
```

### <font color=orange> Topic </font>
默认不会持久化到服务端,但是可以配置持久化到服务器, 需要先运行消息接受者.
#### <font color=orange> Producer: 消息发送者 </font>
* 1、创建ConnectionFactory对象，需要指定服务端ip及端口号。
* 2、使用ConnectionFactory对象创建一个Connection对象。
* 3、开启连接，调用Connection对象的start方法。
* 4、使用Connection对象创建一个Session对象。
* 5、使用Session对象创建一个Destination对象（topic、queue），此处创建一个Topic对象。
* 6、使用Session对象创建一个Producer对象。
* 7、创建一个Message对象，创建一个TextMessage对象。
* 8、使用Producer对象发送消息。
* 9、关闭资源。

```java
package com.test;

import org.apache.activemq.ActiveMQConnectionFactory;
import org.junit.Test;

import javax.jms.*;

public class TestActiveMQ {
    /**
     * 测试activeMQ topic消息发送者
     * @throws Exception
     */
    @Test
    public void testTopicProducer() throws Exception {
        // 第一步：创建ConnectionFactory对象，需要指定服务端ip及端口号。
        // brokerURL服务器的ip及端口号
        ConnectionFactory connectionFactory = new ActiveMQConnectionFactory("tcp://192.168.1.184:61616");
        // 第二步：使用ConnectionFactory对象创建一个Connection对象。
        Connection connection = connectionFactory.createConnection();
        // 第三步：开启连接，调用Connection对象的start方法。
        connection.start();
        // 第四步：使用Connection对象创建一个Session对象。
        // 第一个参数：是否开启事务。true：开启事务，第二个参数忽略。
        // 第二个参数：当第一个参数为false时，才有意义。消息的应答模式。1、自动应答2、手动应答。一般是自动应答。
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        // 第五步：使用Session对象创建一个Destination对象（topic、queue），此处创建一个topic对象。
        // 参数：话题的名称。
        Topic topic = session.createTopic("test-topic");
        // 第六步：使用Session对象创建一个Producer对象。
        MessageProducer producer = session.createProducer(topic);
        // 第七步：创建一个Message对象，创建一个TextMessage对象。
		/*
		 * TextMessage message = new ActiveMQTextMessage(); message.setText(
		 * "hello activeMq,this is my first test.");
		 */
        TextMessage textMessage = session.createTextMessage("hello activeMq,this is my topic test");
        // 第八步：使用Producer对象发送消息。
        producer.send(textMessage);
        // 第九步：关闭资源。
        producer.close();
        session.close();
        connection.close();
    }
}
```
#### <font color=orange> Consumer: 消息接受者 </font>
* 1、创建一个ConnectionFactory对象。
* 2、从ConnectionFactory对象中获得一个Connection对象。
* 3、开启连接。调用Connection对象的start方法。
* 4、使用Connection对象创建一个Session对象。
* 5、使用Session对象创建一个Destination对象。和发送端保持一致topic，并且话题的名称一致。
* 6、使用Session对象创建一个Consumer对象。
* 7、接收消息。
* 8、打印消息。
* 9、关闭资源

```java
package com.test;

import org.apache.activemq.ActiveMQConnectionFactory;
import org.junit.Test;

import javax.jms.*;

public class TestActiveMQ {
    /**
     * 测试activeMQ 消息接受者
     * @throws Exception
     */
    @Test
    public void testTopicConsumer() throws Exception {
        // 第一步：创建一个ConnectionFactory对象。
        ConnectionFactory connectionFactory = new ActiveMQConnectionFactory("tcp://192.168.1.184:61616");
        // 第二步：从ConnectionFactory对象中获得一个Connection对象。
        Connection connection = connectionFactory.createConnection();
        // 第三步：开启连接。调用Connection对象的start方法。
        connection.start();
        // 第四步：使用Connection对象创建一个Session对象。
        Session session = connection.createSession(false, Session.AUTO_ACKNOWLEDGE);
        // 第五步：使用Session对象创建一个Destination对象。和发送端保持一致topic，并且话题的名称一致。
        Topic topic = session.createTopic("test-topic");
        // 第六步：使用Session对象创建一个Consumer对象。
        MessageConsumer consumer = session.createConsumer(topic);
        // 第七步：接收消息。
        consumer.setMessageListener(new MessageListener() {

            @Override
            public void onMessage(Message message) {
                try {
                    TextMessage textMessage = (TextMessage) message;
                    String text = null;
                    // 取消息的内容
                    text = textMessage.getText();
                    // 第八步：打印消息。
                    System.out.println(text);
                } catch (JMSException e) {
                    e.printStackTrace();
                }
            }
        });
        System.out.println("topic的消费端03。。。。。");
        // 等待键盘输入
        System.in.read();
        // 第九步：关闭资源
        consumer.close();
        session.close();
        connection.close();
    }
}
```

## <font color=orange> Spring整合ActiveMQ </font>
### <font color=orange>发送消息</font>
#### <font color=orange>导入相关jar包</font>
```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jms</artifactId>
    <version>4.2.4.RELEASE<</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context-support</artifactId>
    <version>4.2.4.RELEASE<</version>
</dependency>
```
#### <font color=orange>spring配置文件`applicationContext-activemq.xml`</font>
* 1、配置ConnectionFactory
* 2、配置生产者, 使用JMSTemplate对象
* 3、配置Destination
	* 注意构造函数的参数是自定义queue/topic的名称.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd">

    <!-- 真正可以产生Connection的ConnectionFactory，由对应的 JMS服务厂商提供 -->
    <bean id="targetConnectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory">
        <property name="brokerURL" value="tcp://192.168.1.184:61616"/>
    </bean>
    <!-- Spring用于管理真正的ConnectionFactory的ConnectionFactory -->
    <bean id="connectionFactory"
          class="org.springframework.jms.connection.SingleConnectionFactory">
        <!-- 目标ConnectionFactory对应真实的可以产生JMS Connection的ConnectionFactory -->
        <property name="targetConnectionFactory" ref="targetConnectionFactory"/>
    </bean>
    <!-- 配置生产者 -->
    <!-- Spring提供的JMS工具类，它可以进行消息发送、接收等 -->
    <bean id="jmsTemplate" class="org.springframework.jms.core.JmsTemplate">
        <!-- 这个connectionFactory对应的是我们定义的Spring提供的那个ConnectionFactory对象 -->
        <property name="connectionFactory" ref="connectionFactory"/>
    </bean>
    <!--这个是队列目的地，点对点的 -->
    <bean id="queueDestination" class="org.apache.activemq.command.ActiveMQQueue">
        <constructor-arg>
            <value>test-queue</value>
        </constructor-arg>
    </bean>
    <!--这个是主题目的地，一对多的 -->
    <bean id="topicDestination" class="org.apache.activemq.command.ActiveMQTopic">
        <constructor-arg value="topic"/>
    </bean>
</beans>
```
#### <font color=orange>测试代码</font>
```java
package com.test;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jms.core.JmsTemplate;
import org.springframework.jms.core.MessageCreator;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.jms.*;

@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration("classpath:applicationContext-activemq.xml")
public class TestActiveMQSpring {

    @Autowired
    private JmsTemplate jmsTemplate;

    @Autowired
    private Queue queue;

    @Test
    public void testQueueProducer() throws Exception {
        jmsTemplate.send(queue, new MessageCreator() {

            @Override
            public Message createMessage(Session session) throws JMSException {
                TextMessage textMessage = session.createTextMessage("spring activemq test");
                return textMessage;
            }
        });
    }
}
```

### <font color=orange>接受消息</font>
#### <font color=orange>导入相关jar包</font>
```xml
<!--导入ActiveMQ的jar包-->
<dependency>
    <groupId>org.apache.activemq</groupId>
    <artifactId>activemq-all</artifactId>
</dependency>
```
#### <font color=orange>创建MessageListener实现类</font>
```java
package com.coppco.search.listener;

import javax.jms.JMSException;
import javax.jms.Message;
import javax.jms.MessageListener;
import javax.jms.TextMessage;

public class HJMessageListener implements MessageListener {
    @Override
    public void onMessage(Message message) {
        try {
            TextMessage textMessage = (TextMessage) message;
            //取消息内容
            String text = textMessage.getText();
            System.out.println(text);
        } catch (JMSException e) {
            e.printStackTrace();
        }
    }
}
```

#### <font color=orange>spring配置文件`applicationContext-activemq.xml`</font>
* 1、配置ConnectionFactory
* 2、配置Destination
	* 注意构造函数的参数是自定义queue/topic的名称要和前面设置的一样.
* 3、配置消息监听者

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd">

    <!-- 真正可以产生Connection的ConnectionFactory，由对应的 JMS服务厂商提供 -->
    <bean id="targetConnectionFactory" class="org.apache.activemq.ActiveMQConnectionFactory">
        <property name="brokerURL" value="tcp://192.168.1.184:61616"/>
    </bean>
    <!-- Spring用于管理真正的ConnectionFactory的ConnectionFactory -->
    <bean id="connectionFactory"
          class="org.springframework.jms.connection.SingleConnectionFactory">
        <!-- 目标ConnectionFactory对应真实的可以产生JMS Connection的ConnectionFactory -->
        <property name="targetConnectionFactory" ref="targetConnectionFactory"/>
    </bean>
    <!--这个是队列目的地，点对点的 -->
    <bean id="queueDestination" class="org.apache.activemq.command.ActiveMQQueue">
        <constructor-arg>
            <value>test-queue</value>
        </constructor-arg>
    </bean>
    <!--这个是主题目的地，一对多的 -->
    <bean id="topicDestination" class="org.apache.activemq.command.ActiveMQTopic">
        <constructor-arg value="topic"/>
    </bean>
    <!-- 接收消息 -->
    <!-- 配置监听器 -->
    <bean id="myMessageListener" class="com.coppco.search.listener.HJMessageListener"/>
    <!-- 消息监听容器 -->
    <bean class="org.springframework.jms.listener.DefaultMessageListenerContainer">
        <property name="connectionFactory" ref="connectionFactory"/>
        <property name="destination" ref="queueDestination"/>
        <property name="messageListener" ref="myMessageListener"/>
    </bean>
</beans>
```
#### <font color=orange>测试代码</font>
```java

```