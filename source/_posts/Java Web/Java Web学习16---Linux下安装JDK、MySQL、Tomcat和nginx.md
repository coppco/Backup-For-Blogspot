---
layout: post
title: Java Web学习16---Linux下安装JDK、MySQL、Tomcat和nginx
comments: true
toc: true
date: 2016-11-13 10:24:53
tags:
	- RPM
	- Java
	- nginx
	- JDK
	- MySQL
	- Tomcat
---

## <font color=orange> RPM </font>

RPM是RPM Package Manager（RPM软件包管理器）的缩写，这一文件格式名称虽然打上了RedHat的标志，但是其原始设计理念是开放式的，现在包括OpenLinux、S.u.S.E.以及Turbo Linux等Linux的分发版本都有采用，可以算是公认的行业标准了。Linux可以通过brew安装: `brew install rpm`.

* RPM主要功能
	* 安装、卸载、升级和管理软件
	* 组件查询功能
	* 验证功能
	* 软件包GPG和MD5数字签名的导入、验证和发布
	* 软件包依赖处理
	* 选择安装
	* 网络远程安装功能
* RPM 命令：遵循GPL协议且功能强大的包管理，它可以建立、安装、请求、确认、和卸载软件包。间接的提升了Linux 的易用性
	* `-e`: 卸载rpm包
	* `-q`: 查询已安装的软件信息
	* `-i`: 安装rpm包
	* `-u`: 升级rpm包
	* `--replacepkgs`: 重新安装rpm包
	* `--justdb`: 升级数据库，不修改文件系统
	* `--percent`: 在软件包安装时输出百分比
	* `--help`: 帮助
	* `--version`: 显示版本信息
	* `-c`: 显示所有配置文件
	* `-d`: 显示所有文档文件
	* `-h`: 显示安装进度
	* `-l`: 列出软件包中的文件
	* `-a`: 显示出文件状态
	* `-p`: 查询/校验一个软件包文件
	* `-v`: 显示详细的处理信息
	* `--dump`: 显示基本文件信息
	* `--nomd5`: 不验证文件的md5支持
	* `--nofiles`: 不验证软件包中的文件
	* `--nodeps`: 不验证软件包的依赖关系
	* `--whatrequires`: 查询/验证需要一个依赖性的软件包
	* `--whatprovides`: 查询/验证提供一个依赖性的软件包
* RPM的常用参数还包括：
	* `－vh`: 显示安装进度；
	* `－U`: 升级软件包；
	* `－qpl`: 列出RPM软件包内的文件信息；
	* `－qpi`: 列出RPM软件包的描述信息；
	* `－qf`: 查找指定文件属于哪个RPM软件包；
	* `－Va`: 校验所有的RPM软件包，查找丢失的文件；
	* `－qa`: 查找相应文件，如`rpm -qa mysql`


## <font color=orange> yum </font>

Yum（全称为 Yellow dog Updater, Modified）是一个在Fedora和RedHat以及CentOS中的Shell前端软件包管理器。基于RPM包管理，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，无须繁琐地一次次下载、安装.

* 常用命令行命令
	* 安装软件(以foo-x.x.x.rpm为例）：yum install foo-x.x.x.rpm
	* 删除软件：yum remove foo-x.x.x.rpm或者yum erase foo-x.x.x.rpm
	* 升级软件：yum upgrade foo或者yum update foo
	* 查询信息：yum info foo
	* 搜索软件（以包含foo字段为例）：yum search foo
	* 显示软件包依赖关系：yum deplist foo
* 常用参数
	* `-e`: 静默执行 
	* `-t`: 忽略错误
	* `-R[分钟]`: 设置等待时间
	* `-y`: 自动应答yes
	* `--skip-broken`: 忽略依赖问题
	* `--nogpgcheck`: 忽略GPG验证
　

## <font color=orange> Linux安装JDK </font>

* 检查是否安装了JDK
	* `java -version`: 如果未找到命令, 那么就没有安装, 有版本信息那么久已经安装了
* 查看安装了哪些JDK版本
	* `rpm -qa | grep java`
* 卸载JDK
	* `rpm -e --nodeps 卸载的包`: `-e`是卸载, `--nodeps`是不验证软件包的依赖关系
* 安装JDK(tar.gz压缩包), 如果是rpm包直接运行`rpm -ivh rpm包`即可, rpm包默认安装在`/usr/java`目录下
	* 1、通过scp命令把文件传递到Linux服务器: `scp 文件路径 用户名@服务器ip地址:路径`, 输入密码后即可开始传递
	* 2、先把文件拷贝到`/usr/local/java`目录下
	* 3、解压tar压缩包: `tar -xvf tar包名`
	* 4、安装插件: `yum install glibc.i686`
	* 5、配置环境变量
		* 5.1、编辑`/etc/profile`文件, 运行`vi /etc/profile`命令
		* 5.2、在末尾添加一下内容
		>	#set java environment
		#JAVA_HOME后面是你的JDK安装路径, 路径和版本不一样可能不同
		JAVA_HOME=/usr/local/java/jdk1.7.0_72
		CLASSPATH=.:$JAVA_HOME/lib.tools.jar
		PATH=$JAVA_HOME/bin:$PATH
		export JAVA_HOME CLASSPATH PATH
		* 5.3、使更改的配置立即生效: `source /etc/profile`
	* 6、运行: `java -version`如果可以看到版本信息即可
		* `echo ＄JAVA_HOME`: 查看JAVA_HOME环境变量
		* `echo ＄CLASSPATH`: 查看CLASSPATH环境变量
		* `echo ＄PATH`: 查看PATH环境变量

## <font color=orange> Linux安装MySQL </font>

* 查看是否安装了MySQL
	* `rpm -qa | grep mysql`
* 卸载MySQL
	* `rpm -e --nodeps 卸载的包`: `-e`是卸载, `--nodeps`是不验证软件包的依赖关系
	* CentOS 7新版还需要卸载mariadb: `rpm -e --nodeps mariadb-libs-5.5.52-1.el7.x86_64`
* CentOS7上安装MySQL(tar.gz压缩包)
	* 1、下载MySQL: [MySQL-5.7]( https://cdn.mysql.com//Downloads/MySQL-5.7/mysql-5.7.18-1.el7.x86_64.rpm-bundle.tar
)
	* 2、通过scp命令把文件传递到Linux服务器: `scp 文件路径 用户名@服务器ip地址:路径`, 输入密码后即可开始传递.可以通过windows下载, 然后通过scp命令传输到服务器, 也可以通过wget命令直接在Linux服务器上面下载(如果提示没有wget命令, 需要运行`yum -y install wget`).
	* 3、创建目录: `/usr/local/mysql`, 并把MySQL的tar包拷贝到该目录
	* 4、解压tar压缩包: `tar -xvf tar包名`
	* 5、安装相关插件(如果需要): `yum -y install libaio.so.1`、`yum -y install libgcc_s.so.1`和`yum -y install libncurses.so.5`
	* 6、安装 对应的rpm包, 注意顺序
		* 6.1、`rpm -ivh mysql-community-common-5.7.18-1.el7.x86_64.rpm`
		* 6.2、`rpm -ivh mysql-community-libs-5.7.18-1.el7.x86_64.rpm`
		* 6.3、`rpm -ivh mysql-community-client-5.7.18-1.el7.x86_64.rpm`
		* 6.4、`rpm -ivh mysql-community-server-5.7.18-1.el7.x86_64.rpm`
			* CentOS7需要依赖安装perl:  `yum -y install perl`
	* 7、数据库初始化
		* 7.1、为了保证数据库目录为与文件的所有者为 mysql 登陆用户，如果你的linux系统是以 root 身份运行 mysql 服务，需要执行下面的命令初始化: `mysqld --initialize --user=用户名`, 用户名一般是root
		* 7.2、上面的命令运行了之后, 系统会生成一个随机密码, 会在`/var/log/mysqld.log`保存
		* 7.3、可以通过`grep password /var/log/mysqld.log`命令查看, 例如: `A temporary password is generated for root@localhost: K-hd5nRr/1Il`, 那么<font color=red>`K-hd5nRr/1Il`</font>就是密码.
		* 7.4、登录MySQL后可以修改密码`mysql -uroot -pK-hd5nRr/1Il`
			* <font color=red>7.4.1、如果该命令运行之后出现</font>`ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/var/run/mysqld/mysqld.sock' (2)`, 那是因为MySQL没有正常启动导致
			* 7.4.2、老版本运行`service mysqld start`, 新版本运行`systemctl start mysqld.service`开启服务
			* 7.4.3、如果不能运行, 查看报错日志, `grep [ERROR] | /var/log/mysqld.log`, 如果出现`The innodb_system data file 'ibdata1' must be writable`
			* 7.4.4、运行`mkdir -p /usr/local/mysql/data`
			* 7.4.5、更改权限: `chmod -R 777 /usr/local/mysql/data/`
			* 7.4.6、修改MySQL的配置文件: `vi /etc/my.cnf`, 更改`datadir=/var/lib/mysql/data`为刚刚的文件夹.
		* 7.5、修改密码: `set password=password('新密码');`
			* 7.5.1、新版本MySQL, 由于密码策略, 过于简单的密码可能无法修改成功
			* 7.5.2、MySQL查看密码策略等级: `select @@validate_password_policy;`
				* 0/LOW：只检查长度。
				* 1/MEDIUM：检查长度、数字、大小写、特殊字符。
				* 2/STRONG：检查长度、数字、大小写、特殊字符字典文件。
			* 7.5.3、查看相关密码策略: `SHOW VARIABLES LIKE 'validate_password%';`
				* validate_password_check_user_name: 检查用户名, on或者off
				* validate_password_dictionary_file: 插件用于验证密码强度的字典文件路径
				* validate_password_length: 密码最小长度，参数默认为8，它有最小值的限制，最小值为:validate_password_number_count + validate_password_special_char_count + (2 * validate_password_mixed_case_count)
				* validate_password_mixed_case_count 密码至少要包含的小写字母个数和大写字母个数。
				* validate_password_number_count 密码至少要包含的数字个数。
				* validate_password_policy 密码强度检查等级，0/LOW、1/MEDIUM、2/STRONG
				* validate_password_special_char_count 密码至少要包含的特殊字符数。
			* 7.5.4、修改密码策略
				*  `set global validate_password_mixed_case_count=0;`
				*  `set global validate_password_policy=0;`
				*  `set global validate_password_number_count=3;`
				*  `set global validate_password_special_char_count=0;`
				*  `set global validate_password_length=3;`
	* <font color=red>8、无法远程连接到Linux服务器上面的数据库</font>
		* 方式1: 
			* 8.1.1、首先登录到MySQL中: `mysql -uroot -p` 
			* 8.1.2、然后运行 `grant all privileges on *.* to root@'%' identified by '123456';`
				* `*.*`表示所有数据库的所有表
				* `root`表示用户名
				* `123456`: 表示使用该密码登录
			* 8.1.3、刷新: `flush privileges;`
			* 8.1.4、退出MySQL: `exit;`
			* 8.1.5、重启MySQL: `systemctl restart mysqld.service`
		* 方式2:
			* 8.2.1、首先登录到MySQL中: `mysql -uroot -p` 
			* 8.2.2、选择数据库: `use mysql`
			* 8.2.3、查看mysql库中的user表的host值: `select 'host' from user where user='root';`
			* 8.2.4、修改host值（以通配符%的内容增加主机/IP地址），当然也可以直接增加IP地址 : `update user set host = '%' where user ='root';`
			* 8.2.5、刷新MySQL的系统权限相关表: `flush privileges;`
			* 8.2.6、重起mysql服务即可
		* 另外, 如果mysql装在阿里云服务器上, 请检查ECS的`安全组配置`
			* 添加一个`出方向`的`Tcp`、端口`3306`授权对象为`0.0.0.0/0`的策略
		* 8.3、如果服务器是 CentOS7，将 MySQL 服务加入防火墙: `sudo firewall-cmd --zone=public --permanent --add-service=mysql`
		* 8.4、重启防火墙: `sudo systemctl restart firewalld`
	* 9、字符集问题, 需要修改为utf8, 如果数据库要存储emoji表情, 那么需要使用`utf8mb4`, 它向下兼容utf8.
		* 9.1.1、修改现有数据库和表
```
ALTER DATABASE `数据库名` CHARACTER SET = utf8mb4 COLLATE = utf8mb4_unicode_ci;

ALTER TABLE `表名` CONVERT TO CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
```
		* 9.1.2、运行`vi /etc/my.cnf`
		* 9.1.3、在配置文件中添加一个字符集的配置
```
[client]
# 客户端来源数据的默认字符集
default-character-set= utf8mb4
[mysqld]
# 服务端默认字符集
character-set-server=utf8mb4
# 连接层默认字符集
collation-server=utf8mb4_unicode_ci
[mysql]
# 数据库默认字符集
default-character-set= utf8mb4
```
		* 9.1.4、重启mysql服务
	* 10、查看MySQL服务状态: 老版本使用`service mysqld status`, 新版本使用`systemctl status mysqld.service`
	* 11、启动MySQL: 老版本使用`service mysqld start`, 新版本使用`systemctl start mysqld.service`
	* 12、停止MySQL: 老版本使用`service mysqld stop`, 新版本使用`systemctl stop mysqld.service`
	* 13、重启MySQL: 老版本使用`service mysqld restart `, 新版本使用`systemctl restart mysqld.service`
	* 14、设置mysql服务开机自启动: `systemctl enable mysqld.service`
	* 15、停止mysql服务开机自启动: `systemctl disable mysqld.service`

## <font color=orange> Linux安装Tomcat </font>

* 1、下载Tomcat的Linux版本: [Tomcat-7.0.79.tar.gz](https://www.apache.org/dist/tomcat/tomcat-7/v7.0.79/bin/apache-tomcat-7.0.79.tar.gz)
* 2、新建tomcat文件夹: `mkdir -p /usr/local/tomcat`
* 3、把tomcat.tar.gz拷贝到该文件夹下: `cp tomcat.tar.gz /usr/local/tomcat`
* 4、解压tar: `tar -xvf tomcat.tar.gz`
* 5、运行Tomcat
	* 进入`/usr/local/tomcat/apache-tomcat-x.x.x/bin`目录,不同版本后面的路径可能不一样
		* 方式1: 运行 `sudo sh startup.sh`
		* 方式2: 运行 `./startup.sh` 
* 6、<font color=red>Linux默认情况下8080端口对外是是关闭的, 需要开启这个端口或者修改Tomcat端口为80</font>
	* 永久开启8080端口: `firewall-cmd --add-port=8080/tcp --zone=public --permanent`
	* 查询所有已经开启的端口: `firewall-cmd --zone=public --list-ports`
	* 移除8080端口:`firewall-cmd --permanent --zone=public --remove-port=8080/tcp`
	* 刷新防火墙使设置生效: `firewall-cmd  --reload`
	* 重启防火墙: `sudo systemctl restart firewalld`

### <font color=orange> war包 </font>
war包在tomcat/webapps目录中, tomcat启动时,会自动解压


## <font color=orange> nginx </font>
Nginx (engine x) 是一个高性能的HTTP和反向代理服务器，也是一个IMAP/POP3/SMTP服务器。其特点是占有内存少，并发能力强，负载均衡,动静分离.事实上nginx的并发能力确实在同类型的网页服务器中表现较好.

* 反向代理（Reverse Proxy）: 是指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。
* 虚拟主机: 可以在一台服务器虚拟出多个网站
* 负载均衡服务器（load-balancing server）: 是进行负载分配的服务器。通过负载均衡服务器，将服务请求均衡分配到实际执行的服务中，从而保证整个系统的响应速度。
* 集群: 集群通信系统是一种用于集团调度指挥通信的移动通信系统，主要应用在专业移动通信领域。该系统具有的可用信道可为系统的全体用户共用，具有自动选择信道功能，它是共享资源、分担费用、共用信道设备及服务的多用途、高效能的无线调度通信系统。

* Linux安装 Nginx
	* nginx官网: [官网地址](http://nginx.org/)
	* 下载nginx源码到服务器
		* `wget http://nginx.org/download/nginx-1.8.1.tar.gz`
	* 解压压缩包
	* 安装依赖包: 
		* gcc: `yum -y install gcc-c++`
		* pcre: `yum install -y pcre pcre-devel`
		* zlib: `yum install -y zlib zlib-devel`
		* openssl: `yum install -y openssl openssl-devel`
	* 编译运行
		* 进入nginx文件夹
		* 使用configure命令创建一makeFile文件, prefix是安装路径, 临时文件目录是`/var/temp/nginx`
```
./configure \
--prefix=/usr/local/nginx \
--pid-path=/var/run/nginx/nginx.pid \
--lock-path=/var/lock/nginx.lock \
--error-log-path=/var/log/nginx/error.log \
--http-log-path=/var/log/nginx/access.log \
--with-http_gzip_static_module \
--http-client-body-temp-path=/var/temp/nginx/client \
--http-proxy-temp-path=/var/temp/nginx/proxy \
--http-fastcgi-temp-path=/var/temp/nginx/fastcgi \
--http-uwsgi-temp-path=/var/temp/nginx/uwsgi \
--http-scgi-temp-path=/var/temp/nginx/scgi
```
		* 确认运行前 `/var/temp/nginx/client`目录已经创建
			* 创建文件夹: `mkdir -p /var/temp/nginx/client`
	* 编译、安装
		* 编译运行: `make`
		* 安装运行: `make install`
	* 运行
		* 前面confingure中`prifix`就是nginx的安装目录, 它里面的`sbin`下有一个nginx可执行程序
		* 进入该文件夹
		* 运行: `./nginx`
		* 打开该服务器ip地址, 出现 `Welcome to nginx!`表示已经成功开启, 如果不能访问, 还需要把80端口添加到防火墙中.
	* 关闭
		* 关闭: `./nginx -s stop`
		* 退出: `./nginx -s quit`
	* 不关闭nginx的情况下更新配置文件
		* `./nginx -s reload`

### <font color=orange> 配置虚拟主机</font>
#### <font color=orange> 通过端口区分不同虚拟机 </font>
在nginx安装目录下有一个`conf`文件夹里面的`nginx.conf`就是配置文件
```xml
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
    
    server {
        #监听的端口
        listen       80; 
        #监听的域名
        server_name  localhost;
   
        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        #访问路径
        location / {
            #路径  相对于nginx安装目录下的html路径
            root   html;
            #欢迎语
            index  index.html index.htm;
        }

        #error_page  404              /404.html;
    
        # redirect server error pages to the static page /50x.html
        #错误页面
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
         }
    }
}
```
* 通过端口区分就是添加一个server标签, 然后修改不同的端口
```xml
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
    
    server {
        #监听的端口
        listen       80; 
        #监听的域名
        server_name  localhost;
   
        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        #访问路径
        location / {
            #路径  相对于nginx安装目录下的html路径
            root   html;
            #欢迎语
            index  index.html index.htm;
        }

        #error_page  404              /404.html;
    
        # redirect server error pages to the static page /50x.html
        #错误页面
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
         }
    }
    
    server {
        #监听的端口
        listen       81; 
        #监听的域名
        server_name  localhost;
   
        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        #访问路径
        location / {
            #路径  相对于nginx安装目录下的html路径
            root   html81;
            #欢迎语
            index  index.html index.htm;
        }

        #error_page  404              /404.html;
    
        # redirect server error pages to the static page /50x.html
        #错误页面
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html81;
         }
    }
}
```
#### <font color=orange> 通过域名区分不同虚拟机 </font>
通过域名区分就是添加一个server标签, 然后修改不同的hostname
```xml
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
    
    server {
        #监听的端口
        listen       80; 
        #监听的域名
        server_name  localhost;
   
        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        #访问路径
        location / {
            #路径  相对于nginx安装目录下的html路径
            root   html;
            #欢迎语
            index  index.html index.htm;
        }

        #error_page  404              /404.html;
    
        # redirect server error pages to the static page /50x.html
        #错误页面
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html;
         }
    }
    
    server {
        #监听的端口
        listen       80; 
        #监听的域名
        server_name  www.coppco.com;
   
        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        #访问路径
        location / {
            #路径  相对于nginx安装目录下的html路径
            root   html-coppco;
            #欢迎语
            index  index.html index.htm;
        }

        #error_page  404              /404.html;
    
        # redirect server error pages to the static page /50x.html
        #错误页面
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            root   html-coppco;
         }
    }
}
```
### <font color=orange>反向代理</font>
反向代理（Reverse Proxy）方式是指以代理服务器来接受internet上的连接请求，然后将请求转发给内部网络上的服务器，并将从服务器上得到的结果返回给internet上请求连接的客户端，此时代理服务器对外就表现为一个反向代理服务器。

```xml
#user  nobody;
worker_processes  1;

#error_log  logs/error.log;
#error_log  logs/error.log  notice;
#error_log  logs/error.log  info;

#pid        logs/nginx.pid;


events {
    worker_connections  1024;
}


http {
    include       mime.types;
    default_type  application/octet-stream;

    #log_format  main  '$remote_addr - $remote_user [$time_local] "$request" '
    #                  '$status $body_bytes_sent "$http_referer" '
    #                  '"$http_user_agent" "$http_x_forwarded_for"';

    #access_log  logs/access.log  main;

    sendfile        on;
    #tcp_nopush     on;

    #keepalive_timeout  0;
    keepalive_timeout  65;

    #gzip  on;
    
    #服务器列表, server_xxxx名称可以自己定义, 里面的ip和端口设置为自己的服务器的ip, weight是配置权重的, 权重却高请求越多
    upstream server_xxxx{
    	server 192.168.1.111:8080;
    	server 192.168.1.222:8080 weight=2;
    	#添加ip_hash;可以保证同一个ip进入同一台服务器, 解决session共享问题
    	ip_hash;
	}

    
    server {
        #监听的端口
        listen       80; 
        #监听的域名
        server_name  localhost;
   
        #charset koi8-r;

        #access_log  logs/host.access.log  main;
        #访问路径
        location / {
            #通过代理访问, server_xxxx
            proxy_pass http://server_xxxx;
            #欢迎语
            index  index.html index.htm;
        }

        #error_page  404              /404.html;
    
        # redirect server error pages to the static page /50x.html
        #错误页面
        error_page   500 502 503 504  /50x.html;
        location = /50x.html {
            proxy_pass http://server_xxxx;
         }
    } 
}
```
### <font color=orange>负载均衡</font>
可以根据服务器的实际情况调整服务器权重。权重越高分配的请求越多，权重越低，请求越少。默认是都是1

```
upstream server_xxxx{
    server 192.168.1.111:8080;
    server 192.168.1.222:8080 weight=2;
    #添加ip_hash;可以保证同一个ip进入同一台服务器, 解决session共享问题
    ip_hash;
 }
```