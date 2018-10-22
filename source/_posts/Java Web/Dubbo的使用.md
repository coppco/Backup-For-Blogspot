---
layout: post
title: Dubbo的使用
comments: true
toc: true
date: 2017-05-06 14:19:31
tags:
	- Java
	- Dubbo
	- 分布式服务架构
---

Dubbo是阿里巴巴公司开源的一个高性能优秀的服务框架，使得应用可通过高性能的RPC实现服务的输出和输入功能，可以和Spring框架无缝集成。

Dubbo目前只适合Java使用,Dubbo经历过停止更新之后, 目前又重新维护了!!!

<!--more-->

## <font color=orange> 什么是Dubbo </font>

随着互联网的发展, 网站应用的规模不断扩大, 常规的垂直应用框架已经无法应对, 分布式服务框架以及流动计算框架势在必行.

* 单一应用架构
	* 当网站流量很小时, 只需一个应用, 将所有功能都部署在一起, 以减少部署节点和成本.
	* 此时, 用于简化增删改查的工作量的数据访问框架(ORM)是关键.
	* Cluster数一般: 1 ~ 10个
* 垂直应用架构
	* 当访问量逐渐增大, 单一应用增加机器带来的加速度越来越小, 将应用拆成互不相干的几个应用, 以提升效率.
	* 此时, 用于加速前端网页开发的Web框架(MVC)是关键.
	* Cluster数一般: 10 ~ 1000个
* 分布式服务架构
	* 当垂直应用越来越多, 应用之间交互不可避免, 将核心业务取出来, 作为独立的服务, 逐渐形成稳定的服务中心, 使前端应用能快速的响应多变的市场需求.
	* 此时, 用于提高业务复用及整合的分布式服务框架(RPC)是关键.
	* Cluster数一般: 1000 ~ 10000个
* 流动计算架构
	* 当服务越来越多, 容量的评估, 小服务资源的浪费等问题逐渐呈现, 此时需增加一个调度中心基于访问压力实时管理集群容量, 提高集群利用率.
	* 此时, 用于提高机器利用率的资源调度和治理中心(SOA)是关键.
	* Cluster数一般: 10000+个

Dubbo就是资源调度和治理中心的管理工具.

## <font color=orange> Dubbo的架构 </font>
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/dubbo1.png" alt="dubbo" style="width: 70%; text-align: center; display: block;"/>
</center>

* 节点角色说明
	* Provider: 暴露服务的服务提供方.
	* Consumer: 调用远程服务的服务消费方.
	* Registry: 服务注册与发现的注册中心.
	* Monitor: 统计服务的调用次调和调用时间的监控中心.
	* Container: 服务运行容器.
* 调用关系说明
	* 0、服务容器负责启动, 加载, 运行服务提供者
	* 1、服务提供者在启动时, 向注册中心注册自己提供的服务.
	* 2、服务消费者在启动时, 向注册中心订阅自己所需的服务.
	* 3、注册中心返回服务提供者地址列表给消费者, 如果有变更, 注册中心将基于长链接推送变更数据给消费者.
	* 4、服务消费者, 从提供者地址列表汇中, 基于软负载均衡算法, 选一台提供者进行调用, 如果调用失败, 再选另一台调用.
	* 5、服务消费者和提供者, 在内存中累计调用次数和调用时间, 定时每分钟发送一次统计数据到监控中心.

## <font color=orange> 注册中心 </font>
注册中心负责服务地址的注册和查找, 相当于目录服务, 服务提供者和消费者只在启动时与注册中心交互, 注册中心不转发请求, 压力较小. 建议使用Zookeeper注册中心.

Zookeeper是Apache Hadoop的子项目, 是一个树形的目录服务, 支持变更服务, 支持变更推送, 适合作为Dubbo服务的注册中心, 工业强度较高, 可用于生成环境.

### <font color=orange> 安装Zookeeper </font>
* 1、创建安装目录
	* `mkdir -p /usr/local/services/zookeeper`
	* `cd /usr/local/services/zookeeper`
* 2、使用[清华大学开源软件镜像站](https://mirrors.tuna.tsinghua.edu.cn/)下载Zookeeper.
	* `wget https://mirrors.tuna.tsinghua.edu.cn/apache/zookeeper/zookeeper-3.4.11/zookeeper-3.4.11.tar.gz`
* 3、解压缩
	* `tar -zxvf zookeeper-3.4.11.tar.gz`
* 4、修改配置
	* `cd zookeeper-3.4.11`
	* `mkdir data`
	* `cd conf`
	* `cp zoo_sample.cfg zoo.cfg`
	* `vi zoo.cfg`
	* 修改`dataDir`为刚刚新建的data文件夹路径
	* 保存`zoo.cfg`文件
* 5、启动Zookeeper服务
	* `cd ../bin/`
	* `./zkServer.sh start`
* 6、其他相关命令
	* 状态: `./zkServer.sh status`
	* 关闭: `./zkServer.sh stop`
	* 重启: `./zkServer.sh restart`
* 7、Linux系统还要注意防火墙端口问题
	* 开放2181端口: `firewall-cmd --add-port=2181/tcp --zone=public --permanent`, --permanent是永久有效参数
	* 查询开放的端口: `firewall-cmd --zone=public --list-ports`
	* 刷新防火墙使设置生效: `firewall-cmd --reload`
* 8、设置开机启动
	* 在`/etc/init.d`目录下新建一个zookeeper文件
```
vi /etc/init.d/zookeeper
```
	* 在zookeeper文件中写入, 注意里面的JAVA_HOME是必须的,写上你的安装路径, 还有Zookeeper的安装路径也要改成你的安装路径
```
#!/bin/bash  
#chkconfig:2345 20 90  
#description:zookeeper  
#processname:zookeeper  
export JAVA_HOME=/usr/java/jdk1.8.0_171
case $1 in  
        start) su root /usr/local/zookeeper/bin/zkServer.sh start;;  
        stop) su root /usr/local/zookeeper/bin/zkServer.sh stop;;  
        status) su root /usr/local/zookeeper/bin/zkServer.sh status;;  
        restart) su /usr/local/zookeeper/bin/zkServer.sh restart;;  
        *) echo "require start|stop|status|restart" ;  
esac 
```
	* 修改权限: `chmod +x /etc/init.d/zookeeper`
	* 开机启动设置
		* 开机启动: `chkconfig --add zookeeper`
		* 启动zookeeper: `service zookeeper start` 
		* 停止zookeeper: `service zookeeper stop` 
		* 重启zookeeper: `service zookeeper restart` 
		* zookeeper状态: `service zookeeper status`  

## <font color=orange> 使用Dubbo </font>
### <font color=orange> 导入相关Dubbo和Zookeeper相关jar包 </font>
```xml
<properties>
    <dubbo.version>2.5.3</dubbo.version>
    <zookeeper.version>3.4.7</zookeeper.version>
    <zkclient.version>0.1</zkclient.version>
</properties>
<!-- dubbo相关 -->
<dependencys>
    <dependency>
        <groupId>com.alibaba</groupId>
        <artifactId>dubbo</artifactId>
        <version>${dubbo.version}</version>
        <exclusions>
            <exclusion>
                <artifactId>spring</artifactId>
                <groupId>org.springframework</groupId>
            </exclusion>
        </exclusions>
    </dependency>
    <dependency>
        <groupId>org.apache.zookeeper</groupId>
        <artifactId>zookeeper</artifactId>
        <version>${zookeeper.version}</version>
    </dependency>
    <dependency>
        <groupId>com.github.sgroschupf</groupId>
        <artifactId>zkclient</artifactId>
        <version>${zkclient.version}</version>
    </dependency>
</dependencys>
```
### <font color=orange> Service接口和实现类 </font>
#### <font color=orange> Service接口</font>

```java
package com.coppco.service;

import com.coppco.pojo.TbItem;

public interface ItemService {

    TbItem getItemById(Long id);
}
```
#### <font color=orange> Service实现类 </font>
```java
package com.coppco.service.impl;

import com.coppco.mapper.TbItemMapper;
import com.coppco.pojo.TbItem;
import com.coppco.service.ItemService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Service;


@Service
public class ItemServiceImpl implements ItemService {

    @Autowired
    private TbItemMapper tbItemMapper;

    public TbItem getItemById(Long id) {
        return tbItemMapper.selectByPrimaryKey(id);
    }
}
```
### <font color=orange> 服务层发布Dubbo服务 </font>
在Service工程中的Spring配置文件`applicationContext-service.xml`中添加: 
```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:context="http://www.springframework.org/schema/context" xmlns:p="http://www.springframework.org/schema/p"
       xmlns:aop="http://www.springframework.org/schema/aop" xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xmlns:dubbo="http://code.alibabatech.com/schema/dubbo"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.2.xsd
	http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.2.xsd
	http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.2.xsd http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.2.xsd
	http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-4.2.xsd http://code.alibabatech.com/schema/dubbo http://code.alibabatech.com/schema/dubbo/dubbo.xsd">

    <!--注解扫描器-->
    <context:component-scan base-package="com.coppco.service"/>

    <!--发布dubbo服务-->
    <!--提供方信息, 用于计算依赖关系-->
    <dubbo:application name="taotao-manager"/>
    <!--注册中心地址-->
    <dubbo:registry protocol="zookeeper" address="192.168.1.184:2181"/>
    <!--用dubbo协议在20880端口暴露服务-->
    <dubbo:protocol name="dubbo" port="20880"/>
    <!--声明需要暴露的服务接口-->
    <dubbo:service interface="com.coppco.service.ItemService" ref="itemServiceImpl" timeout="300000"></dubbo:service>

</beans>
```
### <font color=orange> 表现层中使用Dubbo </font>
* 在表现层的SpringMVC的配置文件`springmvc.xml`中添加: 
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


    <!--注解驱动-->
    <mvc:annotation-driven/>

    <!--配置视图解析器: 可以配置也可以不配置-->
    <bean class="org.springframework.web.servlet.view.InternalResourceViewResolver">
        <!--前缀-->
        <property name="prefix" value="/WEB-INF/jsp/"/>
        <!--后缀-->
        <property name="suffix" value=".jsp"/>
    </bean>

    <!-- 消费方应用名，用于计算依赖关系，不是匹配条件，不要与提供方一样 -->
    <dubbo:application name="taotao-web"/>
    <!--注册中心地址-->
    <dubbo:registry protocol="zookeeper" address="192.168.1.184:2181"/>
    <!--引用dubbo服务-->
    <dubbo:reference interface="com.coppco.service.ItemService" id="itemServiceImpl" timeout="300000"/>

    <!--注解扫描-->
    <context:component-scan base-package="com.coppco.controller"/>
</beans>

```
* 使用暴露的接口
```java
package com.coppco.controller;

import com.coppco.pojo.TbItem;
import com.coppco.service.ItemService;
import org.springframework.stereotype.Controller;
import org.springframework.web.bind.annotation.PathVariable;
import org.springframework.web.bind.annotation.RequestMapping;
import org.springframework.web.bind.annotation.ResponseBody;

import javax.annotation.Resource;


/*
 * 商品管理Controller
 */

@Controller
public class ItemController {

    @Resource
    private ItemService itemService;

    @RequestMapping("/item/{itemId}")
    @ResponseBody
    public TbItem getItemById(@PathVariable Long itemId) {
        return itemService.getItemById(itemId);
    }

}

```
## <font color=orange> Dubbo监控中心 </font>
Dubbo提供了监控中, 推荐部署在安装注册中心的电脑上, 首先需要安装Tomcat, 然后通过Tomcat部署`dubbo-admin-x.x.x.war`.部署之后访问地址是`http://x.x.x.x:8080/dubbo-admin/`, 登录名和密码默认是`root`.
* 如果监控中心和注册中心不在一台电脑上, 需要修改`dubbo-admin.x.x.x.war`包解压中的`/dubbo-admin/WEB-INF/dubbo.properties`中的注册中心地址、用户名和密码.