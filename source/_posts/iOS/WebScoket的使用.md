---
layout: post
title: WebScoket的使用
comments: true
toc: true
date: 2018-03-15 09:49:12
tags:
	- iOS
	- Java
	- WebSocket
---

<img src="http://47.96.147.179/images/java/moxie_WebSocket.jpg" alt="hello" style="width: 70%; text-align: center; display: block; margin-top:30px; margin-bottom:30px"/>


以往在Web项目中, 如果要实现和服务器实时通讯可以通过轮询、长轮询来实现, 此时即浪费带宽又消耗服务器资源.但是使用WebSocket可以很好的解决该问题, 如果你担心不同浏览器不支持WebSocket, 那么[socketio](https://socket.io)是一个不错的选择, 它封装了WebSocket、轮询和其他一些实时通讯方式.
<!--more-->

## <font color=orange>OSI参考模型</font>
七层模型，亦称OSI（Open System Interconnection）参考模型，是国际标准化组织（ISO）制定的一个用于计算机或通信系统间互联的标准体系。
它是一个七层的、抽象的模型体，不仅包括一系列抽象的术语或概念，也包括具体的协议。从上到下分别是应用层、表示层、会话层、传输层、网络层、数据链路层和物理层.
具体的网络通讯协议图, 请参考[科来](http://www.colasoft.com.cn/download/protocols_map.php)

### <font color=orange>TCP/IP协议簇常见协议</font>
* 应用层
	* DHCP、FTP、HTTP、POP3、SMTP、TELNET、XMPP、SOAP、MSN、WebSocket
* 表示层
* 会话层
	* TLS、SSL、RPC
* 传输层
	* TCP、UDP
* 网络层
	* DNS、IP
* 数据链路层
	* ARP
* 物理层

### <font color=orange>WebSocket、HTTP和TCP</font>
HTTP和WebSocket是应用层协议, 都是基于TCP协议来传输数据的, 只不过WebSocket必须依赖HTTP协议进行一次握手, 成功之后数据直接从TCP通道传输了.
HTTP是单向通讯, 而WebSocket是双向通讯.

### <font color=orange>WebSocket和Socket</font>
Socket(套接字)并不是一个协议, 它工作在OSI模型中的会话层. Socket实际上是一个编程接口(API), 是对TCP/IP的封装, 方便我们通过网络层传输数据, 通过Socket可以实现双向通讯.

WebSocket是一个应用层协议, 也可以实现双向通讯.
### <font color=orange>WebSocket和HTML5</font>
WebSocket API 是 HTML5 标准的一部分， 但这并不代表 WebSocket 一定要用在 HTML 中，或者只能在基于浏览器的应用程序中使用。

* 基于C的[libwebsocket.org](https://libwebsockets.org/git/libwebsockets)
* 基于Node.js的[Socket.io](http://socket.io/)
* Java中Nett实现的[netty-socketio](https://github.com/mrniko/netty-socketio)
* 基于Python的[ws4py](https://github.com/Lawouach/WebSocket-for-Python)
* 基于C++的[WebSocket++](http://www.zaphoyd.com/websocketpp)
* iOS实现的[SocketRocket](https://github.com/facebook/SocketRocket), [SwiftWebSocket](https://link.jianshu.com/?t=https://github.com/tidwall/SwiftWebSocket)
* Tomcat7.0.47以上版本支持WebSocket
* Spring4.x提供了以`STOMP`协议为基础的websocket通信实现, 对于不支持的浏览器, 使用[sockjs](https://github.com/sockjs/sockjs-client)模拟websocket对象的办法来实现兼容.
* Apache 对 WebSocket 的支持： [Apache Module mod_proxy_wstunnel](http://httpd.apache.org/docs/2.4/mod/mod_proxy_wstunnel.html)
* Nginx 对 WebSockets 的支持： [NGINX as a WebSockets Proxy](http://nginx.com/blog/websocket-nginx/) 、 [NGINX Announces Support for WebSocket Protocol](NGINX Announces Support for WebSocket Protocol) 、[WebSocket proxying](http://nginx.org/en/docs/http/websocket.html)

## <font color=orange>Java实现WebSocket</font>
### <font color=orange>使用Tomcat实现WebSocket</font>
#### <font color=orange>Maven项目使用Tomcat实现WebSocket</font>
* 新建Maven项目, 需要添加Maven依赖
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.coppco</groupId>
    <artifactId>Maven_Tomcat_WebSocket</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>war</packaging>
    <dependencies>
        <dependency>
            <groupId>javax</groupId>
            <artifactId>javaee-api</artifactId>
            <version>7.0</version>
            <!--只在编译和测试时使用, 部署到Tomcat时, Tomcat已经包含该包-->
            <scope>provided</scope>
        </dependency>
    </dependencies>

    <build>
        <finalName>Maven_Tomcat_WebSocket</finalName>
        <plugins>
            <!-- 资源文件拷贝插件 -->
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-resources-plugin</artifactId>
                <version>2.7</version>
                <configuration>
                    <encoding>UTF-8</encoding>
                </configuration>
            </plugin>
        </plugins>
        <pluginManagement>
            <plugins>
                <!-- 配置Tomcat插件 -->
                <plugin>
                    <groupId>org.apache.tomcat.maven</groupId>
                    <artifactId>tomcat7-maven-plugin</artifactId>
                    <version>2.2</version>
                    <configuration>
                        <path>/</path>
                        <port>8080</port>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
</project>
```
* 编写消息处理类
```java
package com.coppco.websocket;

import javax.websocket.*;
import javax.websocket.server.PathParam;
import javax.websocket.server.ServerEndpoint;
import java.io.IOException;
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;


/**
 * 该注解用来指定一个URI，客户端可以通过这个URI来连接到WebSocket。类似Servlet的注解mapping。无需在web.xml中配置
 */
@ServerEndpoint("/websocket/{username}")
public class WebSocket {

    /**
     * 静态变量，用来记录当前在线连接数。应该把它设计成线程安全的
     */
    private static int onlineCount = 0;

    /**
     * 实现服务端与单一客户端通信的话，可以使用Map来存放，其中Key可以为用户标识
     */
    private static Map<String, WebSocket> clients = new ConcurrentHashMap<String, WebSocket>();
    /**
     * 与某个客户端的连接会话，需要通过它来给客户端发送数据
     */
    private Session session;

    /**
     * 用户名
     */
    private String username;

    /**
     * 连接建立成功调用的方法
     * @param username 用户名
     * @param session session为与某个客户端的连接会话，需要通过它来给客户端发送数据
     * @throws IOException
     */
    @OnOpen
    public void onOpen(@PathParam("username") String username, Session session) throws IOException {

        this.username = username;
        this.session = session;

        addOnlineCount();
        clients.put(username, this);
        System.out.println("已连接" + username);
    }

    /**
     * 连接关闭调用的方法
     * @throws IOException
     */
    @OnClose
    public void onClose() throws IOException {
        clients.remove(username);
        System.out.println("已断开" + username);
        subOnlineCount();
    }

    /**
     * 收到客户端消息后调用的方法
     * @param message 消息
     * @throws IOException
     */
    @OnMessage
    public void onMessage(String message) throws IOException {
        //这里可以根据业务逻辑决定群发或者单发
        sendMessageAll(message);

    }

    /**
     * 发生错误时调用
     * @param session
     * @param error
     */
    @OnError
    public void onError(Session session, Throwable error) {
        error.printStackTrace();
    }

    /**
     * 单发消息
     * @param message
     * @param To
     * @throws IOException
     */
    public void sendMessageTo(String message, String To) throws IOException {
        // session.getBasicRemote().sendText(message);
        //session.getAsyncRemote().sendText(message);
        for (WebSocket item : clients.values()) {
            if (item.username.equals(To) )
                item.session.getAsyncRemote().sendText(message);
        }
    }

    /**
     * 群发消息
     * @param message
     * @throws IOException
     */
    public void sendMessageAll(String message) throws IOException {
        for (WebSocket item : clients.values()) {
            item.session.getAsyncRemote().sendText(message);
        }
    }


    public static synchronized int getOnlineCount() {
        return onlineCount;
    }

    public static synchronized void addOnlineCount() {
        WebSocket.onlineCount++;
    }

    public static synchronized void subOnlineCount() {
        WebSocket.onlineCount--;
    }

    public static synchronized Map<String, WebSocket> getClients() {
        return clients;
    }
}

```
* Web客户端
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8"/>
    <title>Java后端WebSocket的Tomcat实现</title>
</head>
<body>
    Welcome<br/><input id="text" type="text"/>
    <button onclick="send()">发送消息</button>
    <hr/>
    <button onclick="closeWebSocket()">关闭WebSocket连接</button>
    <hr/>
    <div id="message"></div>
</body>

<script type="text/javascript">
    var websocket = null;
    //判断当前浏览器是否支持WebSocket
    if ('WebSocket' in window) {
        websocket = new WebSocket("ws://localhost:8080/websocket/" + Math.floor(Math.random()*10+1));
    }
    else {
        alert('当前浏览器 Not support websocket')
    }

    //连接发生错误的回调方法
    websocket.onerror = function () {
        setMessageInnerHTML("WebSocket连接发生错误");
    };

    //连接成功建立的回调方法
    websocket.onopen = function () {
        setMessageInnerHTML("WebSocket连接成功");
    }

    //接收到消息的回调方法
    websocket.onmessage = function (event) {
        setMessageInnerHTML(event.data);
    }

    //连接关闭的回调方法
    websocket.onclose = function () {
        setMessageInnerHTML("WebSocket连接关闭");
    }

    //监听窗口关闭事件，当窗口关闭时，主动去关闭websocket连接，防止连接还没断开就关闭窗口，server端会抛异常。
    window.onbeforeunload = function () {
        closeWebSocket();
    }

    //将消息显示在网页上
    function setMessageInnerHTML(innerHTML) {
        document.getElementById('message').innerHTML += innerHTML + '<br/>';
    }

    //关闭WebSocket连接
    function closeWebSocket() {
        websocket.close();
    }

    //发送消息
    function send() {
        var message = document.getElementById('text').value;
        websocket.send(message);
    }
</script>
</html>
```

#### <font color=orange>Spring Boot使用Tomcat实现WebSocket</font>
使用Spring Boot项目配置稍微有点不一样, 
* 首先需要添加依赖`spring-boot-starter-websocket`, 如果是打成war部署在外部Tomcat时需要添加`javaee-api`的依赖
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.coppco</groupId>
    <artifactId>tomcatwebsocket</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>tomcatwebsocket</name>
    <description>Demo project for Spring Boot</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.2.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-websocket</artifactId>
        </dependency>
        <!--注意如果打成war部署在外部Tomcat时需要添加这个-->
        <!--<dependency>-->
            <!--<groupId>javax</groupId>-->
            <!--<artifactId>javaee-api</artifactId>-->
            <!--<version>7.0</version>-->
            <!--<scope>provided</scope>-->
        <!--</dependency>-->
        <!-- 热部署 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
    </dependencies>

    <build>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
                <configuration>
                    <fork>true</fork>
                </configuration>
            </plugin>
        </plugins>
    </build>
    
</project>
```
* 如果使用内置Tomcat时, 需要在`xxApplication中注入Bean`, 使用外部Tomcat时不需要添加.
```java
package com.coppco;

import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.web.socket.server.standard.ServerEndpointExporter;

@SpringBootApplication
public class TomcatwebsocketApplication {
    public static void main(String[] args) {
        SpringApplication.run(TomcatwebsocketApplication.class, args);
	}

    /**
     * 使用内置Tomcat时需要注入ServerEndpointExporter的Bean, 它会自动注册使用了@ServerEndpoint注解声明的Websocket endpoint
     * @return
     */
    @Bean
    public ServerEndpointExporter serverEndpointExporter() {
        return new ServerEndpointExporter();
    }
}
```
* 编写消息处理类, 需要在处理类上添加`@Component`注解, 其他代码同上
* Web客户端, 代码同上

### <font color=orange>Spring4.x实现WebSocket</font>
* 首先需要添加依赖`spring-boot-starter-websocket`
```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
	xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>com.coppco</groupId>
    <artifactId>springbootwebsocket</artifactId>
    <version>0.0.1-SNAPSHOT</version>
    <packaging>jar</packaging>

    <name>springbootwebsocket</name>
    <description>Demo project for Spring Boot</description>

    <parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.0.2.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
    </parent>

    <properties>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
        <java.version>1.8</java.version>
    </properties>

    <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-websocket</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>

        <!-- 热部署 -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>

    </dependencies>

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
</project>
```
* 在`xxxApplication`类上添加注解、实现接口、注入Bean
```java
package com.coppco;

import com.coppco.interceptor.UsernameInterceptor;
import com.coppco.messageHandle.WebSocketMessageHandle;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.annotation.Bean;
import org.springframework.web.socket.config.annotation.EnableWebSocket;
import org.springframework.web.socket.config.annotation.WebSocketConfigurer;
import org.springframework.web.socket.config.annotation.WebSocketHandlerRegistry;

/**
 * 实现WebSocketConfigurer
 */
@SpringBootApplication
@EnableWebSocket //允许WebSocket
public class SpringbootwebsocketApplication implements WebSocketConfigurer{

    public static void main(String[] args) {
        SpringApplication.run(SpringbootwebsocketApplication.class, args);
    }

    /**
     * 第一个addHandler是对正常连接的配置，第二个是如果浏览器不支持websocket，使用socketjs模拟websocket的连接
     * @param webSocketHandlerRegistry
     */
    @Override
    public void registerWebSocketHandlers(WebSocketHandlerRegistry webSocketHandlerRegistry) {
        webSocketHandlerRegistry.addHandler(webSocketMessageHandle(), "/websocket").addInterceptors(new UsernameInterceptor());
        webSocketHandlerRegistry.addHandler(webSocketMessageHandle(), "/sockjs/webSocket").addInterceptors(new UsernameInterceptor());
    }
    
    /**
     * 消息处理器
     * @return
     */
    @Bean
    public WebSocketMessageHandle webSocketMessageHandle() {
        return new WebSocketMessageHandle();
    };
}
```

* 消息处理器
```java
package com.coppco.messageHandle;

import org.springframework.stereotype.Component;
import org.springframework.web.socket.*;
import org.springframework.web.socket.handler.TextWebSocketHandler;
import java.io.IOException;
import java.util.Map;
import java.util.concurrent.ConcurrentHashMap;

@Component
public class WebSocketMessageHandle extends TextWebSocketHandler {

    /**
     * 静态变量，用来记录当前在线连接数。应该把它设计成线程安全的
     */
    private static int onlineCount = 0;

    /**
     * 实现服务端与单一客户端通信的话，可以使用Map来存放，其中Key可以为用户标识
     */
    private static Map<String, WebSocketSession> clients = new ConcurrentHashMap<String, WebSocketSession>();

    /**
     * 连接成功时候，会触发UI上onopen方法
     */
    @Override
    public void afterConnectionEstablished(WebSocketSession session) throws Exception {
        String username = (String) session.getAttributes().get("username");
        addOnlineCount();
        System.out.println("已连接:  " + username + "session: " + this);
        clients.put((String) session.getAttributes().get("username"), session);
    }


    /**
     * 收到文本消息时，会调用该方法
     */
    @Override
    protected void handleTextMessage(WebSocketSession session, TextMessage message) throws Exception {
        String send = (String) session.getAttributes().get("username");
        sendMessageAll(send, message);
    }

    /**
     * 出现错误时，会调用该方法
     */
    @Override
    public void handleTransportError(WebSocketSession session, Throwable exception) throws Exception {
        super.handleTransportError(session, exception);
    }

    /**
     * 关闭连接时，会调用该方法
     */
    @Override
    public void afterConnectionClosed(WebSocketSession session, CloseStatus status) throws Exception {
        String username = (String) session.getAttributes().get("username");
        clients.remove(username);
        System.out.println("已断开: " + username);
        subOnlineCount();
    }


    /**
     * 单发消息
     * @param message
     * @param To
     * @throws IOException
     */
    public void sendMessageTo(TextMessage message, String To) throws IOException {
        for (Map.Entry<String, WebSocketSession> entry : clients.entrySet()) {
            if (entry.getKey().equals(To)) {
                entry.getValue().sendMessage(message);
            }
        }
    }

    /**
     * 群发消息
     * @param message
     * @throws IOException
     */
    public void sendMessageAll(String send, TextMessage message) throws IOException {
        for (String key: clients.keySet()) {
            if (!key.equals(send)) {
                clients.get(key).sendMessage(message);
                System.out.println("" + send + "发送消息给------>" + key + " : 内容  " + message.getPayload());
            }
        };
    }

    @Override
    public boolean supportsPartialMessages() {
        return super.supportsPartialMessages();
    }

    public static synchronized int getOnlineCount() {
        return onlineCount;
    }

    public static synchronized void addOnlineCount() {
        WebSocketMessageHandle.onlineCount++;
    }

    public static synchronized void subOnlineCount() {
        WebSocketMessageHandle.onlineCount--;
    }

    public static synchronized Map<String, WebSocketSession> getClients() {
        return clients;
    }
}

```

* 拦截器, 添加用户名
```java
package com.coppco.interceptor;

import org.springframework.context.annotation.Scope;
import org.springframework.http.server.ServerHttpRequest;
import org.springframework.http.server.ServerHttpResponse;
import org.springframework.lang.Nullable;
import org.springframework.stereotype.Component;
import org.springframework.web.socket.WebSocketHandler;
import org.springframework.web.socket.server.HandshakeInterceptor;

import javax.rmi.CORBA.Util;
import java.util.Map;
import java.util.Random;


/**
 * 自定义拦截器
 */
@Component
public class UsernameInterceptor implements HandshakeInterceptor {

    @Override
    public boolean beforeHandshake(ServerHttpRequest serverHttpRequest, ServerHttpResponse serverHttpResponse, WebSocketHandler webSocketHandler, Map<String, Object> map) throws Exception {
        String username = (String) map.get("username");
        if (username == null) {
            map.put("username", new Random().nextInt(10) + "");
        }
        return true;
    }

    @Override
    public void afterHandshake(ServerHttpRequest serverHttpRequest, ServerHttpResponse serverHttpResponse, WebSocketHandler webSocketHandler, @Nullable Exception e) {

    }

}
```

* Web客户端, 注意使用SockJS时地址是`http`
```html
<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8"/>
    <title>Java后端WebSocket的Tomcat实现</title>
</head>
<body>
    Welcome<br/><input id="text" type="text"/>
    <button onclick="send()">发送消息</button>
    <hr/>
    <button onclick="closeWebSocket()">关闭WebSocket连接</button>
    <hr/>
    <div id="message"></div>
</body>
<script src="/js/sockjs-0.3.min.js" type="text/javascript"></script>
<script type="text/javascript">
    var websocket = null;
    //判断当前浏览器是否支持WebSocket
    if ('WebSocket' in window) {
        websocket = new WebSocket("ws://localhost:8080/websocket");
    } else if ('MozWebSocket' in window) {
        websocket = new WebSocket("ws://localhost:8080/websocket");
    } else {
        websocket = new SockJS("http://localhost:8080/sockjs/websocket");
    }

    //连接发生错误的回调方法
    websocket.onerror = function () {
        setMessageInnerHTML("WebSocket连接发生错误");
    };

    //连接成功建立的回调方法
    websocket.onopen = function () {
        setMessageInnerHTML("WebSocket连接成功");
    }

    //接收到消息的回调方法
    websocket.onmessage = function (event) {
        setMessageInnerHTML(event.data);
    }

    //连接关闭的回调方法
    websocket.onclose = function () {
        setMessageInnerHTML("WebSocket连接关闭");
    }

    //监听窗口关闭事件，当窗口关闭时，主动去关闭websocket连接，防止连接还没断开就关闭窗口，server端会抛异常。
    window.onbeforeunload = function () {
        closeWebSocket();
    }

    //将消息显示在网页上
    function setMessageInnerHTML(innerHTML) {
        document.getElementById('message').innerHTML += innerHTML + '<br/>';
    }

    //关闭WebSocket连接
    function closeWebSocket() {
        websocket.close();
    }

    //发送消息
    function send() {
        var message = document.getElementById('text').value;
        websocket.send(message);
    }
</script>
</html>
```
## <font color=orange>iOS使用SocketRocket实现WebSocket连接</font>
这里我使用的是[facebook/SocketRocket](https://github.com/facebook/SocketRocket)
* 首先使用Cocoapods导入相关库
```
pod 'SocketRocket'
```
* 简单的示例
```objc
//
//  ViewController.m
//  iOS-WebScoket
//
//  Created by apple on 2018/6/16.
//  Copyright © 2018年 apple. All rights reserved.
//

#import "ViewController.h"
#import <SocketRocket.h>

@interface ViewController ()<SRWebSocketDelegate, UITableViewDelegate, UITableViewDataSource>
/**
 webSocket
 */
@property(nonatomic, strong)SRWebSocket *webSocket;
/**
 显示消息
 */
@property(nonatomic, strong)UITableView *tableView;
/**
 消息
 */
@property(nonatomic, strong)NSMutableArray *messages;
/**
 发送消息
 */
@property(nonatomic, strong)UITextField *messageTF;
@end

@implementation ViewController

- (void)viewDidLoad {
    self.navigationItem.title = @"使用SocketRocket";
    [super viewDidLoad];
    [self.view addSubview:self.tableView];
    [self.webSocket open];
    
}

- (SRWebSocket *)webSocket {
    if (!_webSocket) {
        _webSocket = ({
            SRWebSocket *object = [[SRWebSocket alloc] initWithURLRequest:[NSURLRequest requestWithURL:[NSURL URLWithString:@"ws://localhost:8080/websocket"]]];
            object.delegate = self;
            object;
        });
    }
    return _webSocket;
}

- (UITableView *)tableView {
    if (!_tableView) {
        _tableView = ({
            UITableView *object = [[UITableView alloc] initWithFrame:self.view.bounds style:(UITableViewStylePlain)];
            [object registerClass:[UITableViewCell class] forCellReuseIdentifier:@"cell"];
            object.tableFooterView = [UIView new];
            object.delegate = self;
            object.dataSource = self;
            object;
        });
    }
    return _tableView;
}
- (NSMutableArray *)messages {
    if (!_messages) {
        _messages = ({
            NSMutableArray *object = [[NSMutableArray alloc] init];
            object;
        });
    }
    return _messages;
}

- (UITextField *)messageTF {
    if (!_messageTF) {
        _messageTF = ({
            UITextField *object = [[UITextField alloc] init];
            object.tintColor = [UIColor blueColor];
            object.borderStyle = UITextBorderStyleRoundedRect;
            object.placeholder = @"请输入发送内容";
            object.textColor = [UIColor blackColor];
            object.font = [UIFont systemFontOfSize:16];
            UIButton *rightView = [UIButton buttonWithType:(UIButtonTypeCustom)];
            [rightView addTarget:self action:@selector(sendMessage) forControlEvents:(UIControlEventTouchUpInside)];
            [rightView setTitle:@"   发送   " forState:(UIControlStateNormal)];
            [rightView setTitleColor:[UIColor orangeColor] forState:(UIControlStateNormal)];
            [rightView sizeToFit];
            object.rightView = rightView;
            object.rightViewMode = UITextFieldViewModeAlways;
            object;
        });
    }
    return _messageTF;
}
- (void)sendMessage {
    [self.webSocket send:self.messageTF.text ? : @"iOS"];
}

#pragma - mark UITableViewDelegate, UITableViewDataSource
- (CGFloat)tableView:(UITableView *)tableView heightForHeaderInSection:(NSInteger)section {
    return 40;
}
- (UIView *)tableView:(UITableView *)tableView viewForHeaderInSection:(NSInteger)section {
    return self.messageTF;
}
- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:@"cell" forIndexPath:indexPath];
    cell.textLabel.text = self.messages[indexPath.row];
    return cell;
}

- (NSInteger)numberOfSectionsInTableView:(UITableView *)tableView {
    return 1;
}

- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
    return self.messages.count;
}

#pragma - mark SRWebSocketDelegate

/**
 接收到消息
 */
- (void)webSocket:(SRWebSocket *)webSocket didReceiveMessage:(NSString *)message {
    [self.messages addObject:message];
    [self reload];
}

/**
 连接时
 */
- (void)webSocketDidOpen:(SRWebSocket *)webSocket {
    [self.messages addObject:@"连接WebSocket成功"];
    [self reload];
}

/**
 连接失败时
 */
- (void)webSocket:(SRWebSocket *)webSocket didFailWithError:(NSError *)error {
    [self.messages addObject:@"连接WebSocket失败"];
    [self reload];
}

/**
 关闭时
 */
- (void)webSocket:(SRWebSocket *)webSocket didCloseWithCode:(NSInteger)code reason:(NSString *)reason wasClean:(BOOL)wasClean {
    [self.messages addObject:@"关闭WebSocket连接"];
    [self reload];
}

/**
 接收到服务器的Pong时调用, 一般用作心跳
 */
- (void)webSocket:(SRWebSocket *)webSocket didReceivePong:(NSData *)pongPayload {
    [self.messages addObject:@"收到心跳包"];
    [self reload];
}
/**
 是否把 NSData 转成 NSString, 默认YES
 */
- (BOOL)webSocketShouldConvertTextFrameToString:(SRWebSocket *)webSocket {
    return YES;
}

- (void)reload {
    [self.tableView reloadData];
}

@end

```

## <font color=orange>Spring Boot中使用netty-socketio</font>
[netty-socketio](https://github.com/mrniko/netty-socketio)是基于netty的[socket.io](https://github.com/socketio/socket.io)服务实现，相对于javaee的原生websocket支持（@serverEndpoint）和spring-boot的MessageBroker(@messageMapping)，netty-socketio完整的实现了socket.io提供的监听前台事件、向指定客户端发送事件、将指定客户端加入指定房间、向指定房间广播事件、客户端从指定房间退出等操作。

### <font color=orange>netty-socketio依赖</font>
```xml
<dependency>
    <groupId>com.corundumstudio.socketio</groupId>
    <artifactId>netty-socketio</artifactId>
    <version>1.7.14</version>
</dependency>
```
### <font color=orange> 简单的例子 </font>
```java
package com.coppco.chat.runner;


import com.corundumstudio.socketio.*;
import com.corundumstudio.socketio.listener.ConnectListener;
import com.corundumstudio.socketio.listener.DataListener;
import com.corundumstudio.socketio.listener.DisconnectListener;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;


/**
 * 使用CommandLineRunner, 在Application启动之后回调启动Socket.io
 */
@Component
@org.springframework.context.annotation.Configuration
public class ChatRunner implements CommandLineRunner {

    private final static Logger logger = LoggerFactory.getLogger(ChatRunner.class);


    private static SocketIOServer server;

    @Override
    public void run(String... args) throws Exception {

        Configuration configuration = new Configuration();

        /**
         * ip
         */
        configuration.setHostname("127.0.0.1");
        /**
         * 端口
         */
        configuration.setPort(9090);

        configuration.setPingInterval(5000);

        configuration.setPingTimeout(3000);

        //设置最大每帧处理数据的长度，防止他人利用大数据来攻击服务器
        //configuration.setMaxFramePayloadLength(1024 * 1024);
        //设置http交互最大内容长度
        //configuration.setMaxHttpContentLength(1024 * 1024);


        /**
         * 身份验证
         */
        configuration.setAuthorizationListener(new AuthorizationListener() {
            @Override
            public boolean isAuthorized(HandshakeData handshakeData) {
                logger.info("授权成功");
                return true;
            }
        });

        server = new SocketIOServer(configuration);

        //事件监听, ChatObject是前端和后端发送的数据类型
        server.addEventListener("chatevent", ChatObject.class, new DataListener<ChatObject>() {
            @Override
            public void onData(SocketIOClient socketIOClient, ChatObject object, AckRequest ackRequest) throws Exception {
                //广播
                server.getBroadcastOperations().sendEvent("chatevent", object);
                logger.info(JSONUtils.toJSON(object));
            }
        });

        //连接
        server.addConnectListener(new ConnectListener() {
            @Override
            public void onConnect(SocketIOClient socketIOClient) {
                HandshakeData data = socketIOClient.getHandshakeData();
                logger.info(JSONUtils.toJSON(data) + "上线了");
            }
        });

        //失去连接
        server.addDisconnectListener(new DisconnectListener() {
            @Override
            public void onDisconnect(SocketIOClient socketIOClient) {
                HandshakeData data = socketIOClient.getHandshakeData();
                logger.info(JSONUtils.toJSON(data) + "下线了");
            }
        });

        server.start();
    }
}

```

### <font color=orange>实际使用</font>
#### <font color=orange>创建一个Configuration类</font>
```java
package com.coppco.socketio;

import com.alibaba.dubbo.config.annotation.Reference;
import com.coppco.service.UserService;
import com.corundumstudio.socketio.*;
import com.corundumstudio.socketio.annotation.SpringAnnotationScanner;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Value;
import org.springframework.context.annotation.Bean;

/**
 * netty-socket.io的相关配置类
 */
@org.springframework.context.annotation.Configuration
public class JCConfig {

    @Value("${justChat.service.hostname}")
    private String hostname;

    @Value("${justChat.service.port}")
    private int port;


    @Reference(version = "${consumer.version}",
            application = "${dubbo.application.id}"
    )
    private UserService userService;

    private static final Logger logger = LoggerFactory.getLogger(JCConfig.class);

    @Bean
    public SocketIOServer socketIOServer() {
        Configuration config = new Configuration();

        //设置服务器ip以及端口
        config.setHostname(hostname);
        config.setPort(port);

        //设置ping间隔以及超时时间
        config.setPingInterval(5000);
        config.setPingTimeout(3000);
        config.setWorkerThreads(100);

        //设置
        SocketConfig socketConfig = new SocketConfig();
        socketConfig.setReuseAddress(true);
        config.setSocketConfig(socketConfig);

        //设置最大每帧处理数据的长度，防止他人利用大数据来攻击服务器
        config.setMaxFramePayloadLength(1024 * 1024);
        //设置http交互最大内容长度
        config.setMaxHttpContentLength(1024 * 1024);

        //授权
        config.setAuthorizationListener(new AuthorizationListener() {
            @Override
            public boolean isAuthorized(HandshakeData data) {
                //获取userId和token
                //String userIdString = data.getSingleUrlParam("userId");
                //String token = data.getSingleUrlParam("token");
                
                return true;
            }
        });

        final SocketIOServer server = new SocketIOServer(config);

        return server;
    }

    /**
     * 使用netty-socket.io中的注解扫描
     * @param socketServer
     * @return
     */
    @Bean
    public SpringAnnotationScanner springAnnotationScanner(SocketIOServer socketServer) {
        return new SpringAnnotationScanner(socketServer);
    }
}
```
#### <font color=orange>创建一个Runner在Application启动时开启Server</font>
```java
package com.coppco.socketio;

import com.corundumstudio.socketio.SocketIOServer;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.boot.CommandLineRunner;
import org.springframework.stereotype.Component;

/**
 * Socket.io执行类, 利用CommandLineRunner接口在Application启动之后的回调
 */

@Component
public class JCRunner implements CommandLineRunner {

    @Autowired
    private SocketIOServer server;


    @Override
    public void run(String... args) throws Exception {
        this.server.start();
    }
}
```
#### <font color=orange>消息处理类</font>
```java
package com.coppco.socketio;

import com.alibaba.dubbo.config.annotation.Reference;
import com.coppco.common.pojo.Message;
import com.coppco.common.pojo.User;
import com.coppco.service.MessageService;
import com.coppco.service.UserService;
import com.coppco.socketio.utils.SocketClientUtils;
import com.corundumstudio.socketio.AckCallback;
import com.corundumstudio.socketio.AckRequest;
import com.corundumstudio.socketio.SocketIOClient;
import com.corundumstudio.socketio.SocketIOServer;
import com.corundumstudio.socketio.annotation.OnConnect;
import com.corundumstudio.socketio.annotation.OnDisconnect;
import com.corundumstudio.socketio.annotation.OnEvent;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.util.Date;

/**
 * 消息和事件处理中心
 */

@Component
public class JCMessageEventHandler {

    private static final Logger logger = LoggerFactory.getLogger(JCMessageEventHandler.class);

    @Autowired
    private SocketIOServer server;

    @Reference(version = "${consumer.version}",
            application = "${dubbo.application.id}"
    )
    private UserService userService;

    @Reference(version = "${consumer.version}",
            application = "${dubbo.application.id}"
    )
    private MessageService messageService;

    private String sessionKey = "user";

    /**
     * 连接成功时
     * @param client
     */
    @OnConnect
    public void onConnect(SocketIOClient client) {
        //获取用户id和token
        String userId = client.getHandshakeData().getSingleUrlParam("userId");

        String token = client.getHandshakeData().getSingleUrlParam("token");

        User user = userService.findUserById(Long.parseLong(userId));

        if (user != null) {
            //保存已连接的用户
            SocketClientUtils.put(userId, client.getSessionId());

            client.set(sessionKey, user);

            logger.info(user.getMobile() + ": 连接成功");
        } else {
            client.disconnect();

            logger.error(user.getMobile() + ": 获取信息失败");
        }

    }

    /**
     * 断开连接时
     * @param client
     */
    @OnDisconnect
    public void disConnect(SocketIOClient client) {
        User user = client.get(sessionKey);
        if (null != user) {
            SocketClientUtils.remove(user.getId() + "");

            logger.info(user.getMobile() + ": 断开连接");
        }
    }

    /**
     * 聊天
     * @param client
     * @param ackRequest
     * @param message
     */
    @OnEvent(value = "chat")
    public void eventChat(SocketIOClient client, AckRequest ackRequest, Message message) {

        //ack数据: 在TCP/IP协议中，如果接收方成功的接收到数据，那么会回复一个ACK数据。通常ACK信号有自己固定的格式,长度大小,由接收方回复给发送方。
        if (ackRequest.isAckRequested()) {
            ackRequest.sendAckData("服务器已经收到消息");
        }
        messageService.saveMessage(message);

        Boolean isGroupChat = message.getChatType().equalsIgnoreCase("group");

        Long to_userId = message.getRecipientUserId();

        if (isGroupChat) {

        } else {
            //在线
            if (SocketClientUtils.containsKey(to_userId + "")) {
                SocketIOClient targetClient = server.getClient(SocketClientUtils.get(to_userId + ""));

                targetClient.sendEvent("chat", new AckCallback<String>(String.class) {
                    @Override
                    public void onSuccess(String result) {
                        logger.info(to_userId + "已收到消息 ， ack 回复 ： " + result);
                    }

                    @Override
                    public void onTimeout() {
                        logger.info(to_userId + "接受超时");
                    }
                }, message);
            }
        }
    }
}
```


## <font color=orange>后记</font>

这里只是很简单的使用了一下WebSocket, 实际开发中不可能这么简单, 复杂的逻辑, 心跳机制、断线重连等问题, 以及图片、语音和视频的传输.