---
layout: post
title: Java Web学习17---Oracle
comments: true
date: 2017-03-10 11:14:06
tags:
	- Java
	- Oracle
	- 数据库
---

ORACLE数据库系统是美国ORACLE公司（甲骨文）提供的以分布式数据库为核心的一组软件产品，是目前最流行的客户/服务器(CLIENT/SERVER)或B/S体系结构的数据库之一。

<!--more-->

ORACLE数据库是目前世界上使用最为广泛的数据库管理系统，作为一个通用的数据库系统，它具有完整的数据管理功能；作为一个关系数据库，它是一个完备关系的产品；作为分布式数据库它实现了分布式处理功能。但它的所有知识，只要在一种机型上学习了ORACLE知识，便能在各种类型的机器上使用它。

## <font color=orange> Oracle Database的基本概念 </font>

* Oracle服务器
	* 是一个关系型数据管理系统(RDBMS), 它提供开放的, 全面的, 近乎完整的信息管理.
	* 由一个<font color=red>Oracle数据库</font>和多个<font color=red>Oracle实例</font>组成.
* Oracle体系结构
	* <font color=red>Oracle数据库</font>: Oracle数据库是数据的物理存储。这就包括（数据文件ORA或者DBF、控制文件、联机日志、参数文件）。其实Oracle数据库的概念和其它数据库不一样，这里的数据库是一个操作系统只有一个库。可以看作是Oracle就只有一个大数据库。
	* <font color=red>Oracle实例</font>: 一个Oracle实例（Oracle Instance）有一系列的后台进程（Backguound Processes)和内存结构（Memory Structures)组成。一个数据库可以有n个实例。
	* <font color=red>用户</font>: 用户是在实例下建立的。不同实例可以建相同名字的用户。
	* <font color=red>表空间</font>: 表空间是Oracle对物理数据库上相关数据文件（ORA或者DBF文件）的逻辑映射。一个数据库在逻辑上被划分成一到若干个表空间，每个表空间包含了在逻辑上相关联的一组结构。每个数据库至少有一个表空间(称之为system表空间)。每个表空间由同一磁盘上的一个或多个文件组成，这些文件叫数据文件(datafile)。一个数据文件只能属于一个表空间。
	* <font color=red>数据文件（dbf、ora）</font>: 数据文件是数据库的物理存储单位。数据库的数据是存储在表空间中的，真正是在某一个或者多个数据文件中。而一个表空间可以由一个或多个数据文件组成，一个数据文件只能属于一个表空间。一旦数据文件被加入到某个表空间后，就不能删除这个文件，如果要删除某个数据文件，只能删除其所属于的表空间才行。

## <font color=orange> 安装Oracle </font>

* Windows
&emsp;&emsp;Windows下安装比较简单, 运行`setup.exe`默认安装路径, 路径中不要出现中文.Oracle默认会创建一个名为`orcl`的数据库, 你可以为它设置一个管理密码.后面Oracle会检查系统配置, 如果都符合就可以安装Oracle了.
当安装快完成的时候, 有一个界面提示`数据库创建完成, 有关详细信息, 请检查......`时, 下面有一个`口令管理`按钮, 点击打开`口令管理`界面, `是否锁定账户?`去掉勾选`SCOTT`和`HR`两个用户名, 并设置它们的密码即可. 
* CentOS安装Oracle
	* 1、创建运行oracle数据库的系统用户和用户组
		* 创建用户组oinstall: `groupadd oinstall`
		* 创建用户组dba: `groupadd dba`
		* 创建oracle用户，并加入到oinstall和dba用户组: `useradd -g oinstall -G dba oracle`
		* 设置用户oracle的登陆密码: `passwd oracle`
		* 查看新建的oracle用户: `id oracle`
	* 2、安装依赖包, 不同版本依赖包不同, [具体参考](https://docs.oracle.com/cd/E11882_01/install.112/e47689/pre_install.htm#BABCFJFG)
		* `yum -y install gcc make binutils gcc-c++ compat-libstdc++-33 elfutils-libelf-devel elfutils-libelf-devel-static ksh libaio libaio-devel numactl-devel sysstat unixODBC unixODBC-devel pcre-devel`
	* 3、更改hosts文件, 要把主机名添加到hosts文件中
		* 首先运行`hostname`命令, 查看主机名
		* 然后运行`vi /etc/hosts`, 把主机名添加在127.0.0.1后面
		* 按`:wq`, 保存退出
	* 4、配置内核参数
		* 运行: `vi /etc/sysctl.conf`
		>	#添加以下参数
		fs.aio-max-nr = 1048576
		fs.file-max = 6815744
		kernel.shmall = 2097152
		kernel.shmmax = 536870912
		kernel.shmmni = 4096
		kernel.sem = 250 32000 100 128
		net.ipv4.ip_local_port_range = 9000 65500
		net.core.rmem_default = 262144
		net.core.rmem_max = 4194304
		net.core.wmem_default = 262144
		net.core.wmem_max = 1048576
		vm.hugetlb_shm_group = 1001	
		
		* 让内核参数生效: `/sbin/sysctl -p`
	* 5、修改内核限制
		* 运行: `vi /etc/security/limits.conf`
		>	#在末尾添加
		oracle soft nproc 2047
		oracle hard nproc 16384
		oracle soft nofile 1024
		oracle hard nofile 65536
		oracle soft stack 10240
		
		* `:wq`, 保存退出
	* 6、修改/etc/pam.d/login
		* 运行: `vi /etc/pam.d/login`
		>	#在末尾加入, 如果是32位系统使用/lib/security/pam_limits.so
		session required /lib64/security/pam_limits.so
		session required pam_limits.so
		
		* `:wq`, 保存退出
	* 7、修改系统配置文件
		* `vi /etc/profile`
		>	#加入如下配置
		if [[ $USER = "oracle" ]]; then
		if [[ $SHELL = "/bin/ksh" ]]; then
		ulimit -p16384
		ulimit -n65536
		else
		ulimit -u16384 -n 65536
		fi
		fi
		
		* `:wq`, 保存退出
		* 使其生效: `source /etc/profile`
	* 8、修改oracle环境变量
		* 运行: `vi /home/oracle/.bash_profile`
		>	#加入如下配置文件
		#oracle
		export ORACLE_BASE=/data/oracle #oracle数据库安装目录
		export ORACLE_HOME=$ORACLE_BASE/product/11.2.0/db_1 #oracle数据库路径
		export ORACLE_SID=orcl #oracle启动数据库实例名
		export ORACLE_TERM=xterm #xterm窗口模式安装
		export PATH=$ORACLE_HOME/bin:/usr/sbin:$PATH #添加系统环境变量
		export LD_LIBRARY_PATH=$ORACLE_HOME/lib:/lib:/usr/lib #添加系统环境变量
		export LANG=C #防止安装过程出现乱码
		export NLS_LANG=AMERICAN_AMERICA.ZHS16GBK  #设置Oracle客户端字符集，必须与Oracle安装时设置的字符集保持一致，如：ZHS16GBK，否则出现数据导入导出中文乱码问题
		
		* `:wq`, 保存退出
		* 使其生效: `source /home/oracle/.bash_profile`
	* 9、将下载好的linux.x64_11gR2_database_1of2.zip和linux.x64_11gR2_database_2of2.zip传递到Linux服务器
		* `scp linux.x64_11gR2_database_1of2.zip linux.x64_11gR2_database_2of2.zip root@192.168.1.115:/usr/local/src`
	* 10、创建oracle数据库安装目录
		* oracle数据库安装目录, 可以更改其他目录: `mkdir -p /data/oracle`
		* oracle数据库配置文件目录: `mkdir -p /data/oraInventory`
		* oracle数据库软件包解压目录: `mkdir -p /data/database`
		* 设置目录所有者为oinstall用户组的oracle用户: `chown -R oracle:oinstall /data/oracle`、`chown -R oracle:oinstall /data/oraInventory`、`chown -R oracle:oinstall /data/database`
	* 11、解压
		* 如果没有uzip命令, 运行: `yum install -y unzip zip`
		* 进入压缩包所在的目录: `cd /usr/local/src`
		* 运行: `unzip linux.x64_11g/R2_database_1of2.zip -d /data/database/`
	* 12、创建/etc/oraInst.loc文件
		* 运行: `vi /etc/oraInst.loc`
		>	#添加以下参数
		inventory_loc=oracle数据库配置文件目录
		inst_group=oinstall
	* 13、安装
		* 图形化界面安装, 使用图像化界面登录oracle用户
			* 进入到`cd /data/database/database/`
			* 运行: `./runInstaller`
		* 静默安装, 没有图形化界面时使用这个
			* 编辑、安装数据库响应文件db_install.rsp
			* 最终的安装主要在与三个响应文件的配置：dbca.rsp,netca.rsp,db_install.rsp，分别对应于创建数据库，创建监听，安装数据库。三个文件的路径在database/response 文件夹下。
			* 复制三个配置文件到`/home/oracle`
				* 运行: `mkdir -p /home/oracle`
				* 拷贝配置文件: `cp /data/database/database/response/* /home/oracle`
				* 设置权限: `chmod 770 /home/oracle`
			* 开始安装
				* 登录oracle用户: `su - oracle`
				* 修改配置文件: `vi /home/oracle/db_install.rsp`
				>	#修改下面的内容	
				oracle.install.option=INSTALL_DB_SWONLY // 安装类型
				ORACLE_HOSTNAME=xxx // 主机名称（hostname查询，这里要注意，主机名要在 /etc/hosts 文件中配置好ip对应关系，否则安装会报错）
				UNIX_GROUP_NAME=oinstall // 安装组
INVENTORY_LOCATION=/data/oracle/oraInventory //INVENTORY目录（不填就是默认值）
				SELECTED_LANGUAGES=en,zh_CN,zh_TW // 选择语言
				ORACLE_HOME=/data/oracle/product/11.2.0/db_1 // oracle_home
				ORACLE_BASE=/data/oracle // oracle_base
				oracle.install.db.InstallEdition=EE // oracle版本
				oracle.install.db.isCustomInstall=false //自定义安装，否，使用默认组件
				oracle.install.db.DBA_GROUP=dba // dba用户组
				oracle.install.db.OPER_GROUP=oinstall // oper用户组				oracle.install.db.config.starterdb.type=GENERAL_PURPOSE //数据库类型
				oracle.install.db.config.starterdb.globalDBName=orcl //globalDBName（这里要和第8步配置的sid一致）
				oracle.install.db.config.starterdb.SID=orcl //SID（这里要和第8步配置的sid一致）
				oracle.install.db.config.starterdb.memoryLimit=81920 //自动管理内存的内存(M)				oracle.install.db.config.starterdb.password.ALL=****** //设定所有数据库用户使用同一个密码
				SECURITY_UPDATES_VIA_MYORACLESUPPORT=false（手动写了false）
				DECLINE_SECURITY_UPDATES=true //设置安全更新（貌似是有bug，这个一定要选true，否则会无限提醒邮件地址有问题，终止安装。PS：不管地址对不对）
				
				* `:wq`: 保存退出
				* 运行安装
					* 进入runInstaller文件夹: `cd /data/oracle/database/database`
					* 运行: `./runInstaller -silent -responseFile /home/oracle/db_install.rsp`
					* <font color=red>对于一些不符合安装条件的电脑, 可以通过添加`- ignorePrereq`</font>
						* `./runInstaller -silent -ignorePrereq -responseFile /home/oracle/db_install.rsp`
						* 出现下面文字即表示安装成功
						>	#Root scripts to run
						/data/oracle/product/11.2.0/db_1/root.sh
						To execute the configuration scripts:
		     			1. Open a terminal window 
		     			2. Log in as "root" 
		     			3. Run the scripts 
		     			4. Return to this window and hit "Enter" key to continue 
		     			Successfully Setup Software.
	* 14、切换root用户, 这个脚本
		* 切换用户: `su - root`
		* 进入安装目录: `cd /data/oracle/product/11.2.0/db_1`
		* 运行: `./root.sh`					
	* 15、配置、添加监听
		* 进入: `cd /data/database/response`
		* 查看: `cat netca.rsp | grep -Ev "^#|^$" `
		* 运行: `netca -silent -responsefile /data/database/response/netca.rsp`
			* 如果提示: `DISPLAY environment variable not set` 
			* 运行: `export DISPLAY=127.0.0.1:0.0`
			* 如果提示: `Exception in thread "main" java.lang.UnsatisfiedLinkError: /data/oracle/product/11.2.0/db_1/jdk/jre/lib/amd64/xawt/libmawt.so: libXext.so.6: cannot open shared object file: No such file or directory`
			* 查找: `yum search libXext`
			* 运行: `yum -y install libXext.x86_64`
		* 查询状态: `lsnrctl status`
		* 查询: `netstat -tlnp`, 如果出现`tcp6       0      0 :::1521                 :::*                    LISTEN      21723/tnslsnr`就说明已经完成了监听.
	* 16、创建数据库, 修改相应文件, 有些参数有默认值就不需要
		* 可以修改`/home/oracle/dbca.rsp`文件, 也可以使用参数来运行
		* 参数运行: `dbca -silent -createDatabase -templateName /data/oracle/product/11.2.0/db_1/assistants/dbca/templates/General_Purpose.dbc -gdbName orcl -sid orcl -sysPassword oracle -systemPassword oracle  -emConfiguration LOCAL -dbsnmpPassword oracle -sysmanPassword oracle -characterSet al32utf8`
			* `-silent`: 指以静默方式执行dbca命令
			* `-createDatabase`: 指使用dbca
			* `-templateName`: 指定用来创建数据库的模板名称，这里指定为/data/oracle/product/11.2.0/db_1/assistants/dbca/templates/General_Purpose.dbc，即一般用途的数据库模板
			* `-gdbName`: 指定创建的全局数据库名称，这里指定名称为orcl
			* `-sid`: 指定数据库系统标识符，这里指定为orcl，与数据库同名
			* `-sysPassword`: SYS 用户口令设置为oracle
			* `-systemPassword`:   SYSTEM 用户口令设置为oracle
			* `-emConfiguration`:  指定Enterprise Management的管理选项。LOCAL表示数据库由Enterprise Manager本地管理
			* `-dbsnmpPassword`:   DBSNMP 用户口令设置为oracle
			* `-sysmanPassword`:   SYSMAN 用户口令设置为oracle
			* `-characterSet`: 指定数据库使用的字符集，这里指定为al32utf8
		* 检查安装: `lsnrctl status`
	* 17、设置oracle开机启动
		* 切换root: `su - root`
		* 运行: `vi /etc/oratab`
		* 修改: `orcl:/u01/app/oracle/product/11.2.0/db_1:Y`
		* 设置`Y`后可以通过`dbstart $ORACLE_HOME`来启动实例和监听器
		* 可以通过`dbshut $ORACLE_HOME`关闭实例, 监听器也停止了
		* 查看监听器状态: `lsnrctl status`
		* 开启监听: `lsnrctl start`
		* 停止监听: `lsnrctl stop`
	* 18、将1521端口开放
		* 永久开启1521端口: `firewall-cmd --add-port=1521/tcp --zone=public --permanent`
		* 查询所有已经开启的端口: `firewall-cmd --zone=public --list-ports`
		* 刷新防火墙使设置生效: `firewall-cmd --reload`
		* 重启防火墙: `sudo systemctl restart firewalld`
	* 19、连接Oracle
		* 19.1、使用1158端口, 网页连接
			* 首先需要开启1158端口, 并添加进防火墙
				* `firewall-cmd --add-port=1158/tcp --zone=public --permanent`
				* `firewall-cmd --reload`
				* `sudo systemctl restart firewalld`
			* 然后查看oracledbconsole服务是否启动
				* Linux中使用: `emctl  status  dbconsole`
			* 开启服务: `emctl start dbconsole`
			* 开启监听器: `lsnrctl start`
			* 然后客户机使用: `http://oracle服务器ip或hostname:1158/em/console/logon/logon`来进行连接
		* 19.2、<font color=red>ORACLE 9i之后, ORACLE 11g之前</font>, 可以使用这个5560端口的web形式
			* 开启: `$ORACLE_HOME/bin/isqlplusctl start`
			* 使用: `http://oracle服务器ip或hostname:5560/isqlplus/`
	* 20、修改ORACLE默认字符集, 可以修改为ZHS16GBK
		* 修改ORACLE的字符集
			* 首先使用管理员登录: `sqlplus / as sysdba`
			* 查询当前字符集: `select * from v$nls_parameters where parameter='NLS_CHARACTERSET';`
			* 关闭数据库: `shutdown immediate;`
			* 重新装载数据库: `startup mount`
			* 修改
				* `ALTER SYSTEM ENABLE RESTRICTED SESSION;`
				* `ALTER SYSTEM SET JOB_QUEUE_PROCESSES=0;`
				* `ALTER SYSTEM SET AQ_TM_PROCESSES=0;`
				* `alter database open;`
				* `ALTER DATABASE CHARACTER SET ZHS16GBK;`
					* 如果出现错误: `new character set must be a superset of old character set`
					* 可以跳过超集的检查做更改: `ALTER DATABASE character set INTERNAL_USE ZHS16GBK;`
				* `shutdown immediate;`
				* `startup`
			* 再次查看字符集: `select * from v$nls_parameters where parameter='NLS_CHARACTERSET';`
		* 设置系统环境变量
			* 切换oracle用户: `su - oracle`
			* 设置环境变量
				* `vi ~/.bash_profile` 或者 `cd /home/oracle`, 然后 `vi .bash_profile`
				* 添加: `export NLS_LANG=AMERICAN_AMERICA.ZHS16GBK`
## <font color=orange> Oracle的使用 </font>
* 登录
	* 普通登录: `sqlplus 用户名/密码@ip地址:1521/orcl [as sysdba]`
		* `as sysdba`: 是以管理员的身份登录
	* Linux上面的超级管理员可以通过: `sqlplus / as sysdba`以系统管理员的身份来登录.
* 启动和关闭数据库
	* `startup nomount`: 非安装启动，这种方式下启动可执行：重建控制文件、重建数据库，读取init.ora文件，启动instance，即启动SGA和后台进程，这种启动只需要init.ora文件。
	* `startup mount`: 安装启动，这种方式启动下可执行：数据库日志归档、数据库介质恢复、使数据文件联机或脱机、重新定位数据文件、重做日志文件。执行“nomount”，然后打开控制文件，确认数据文件和联机日志文件的位置，但此时不对数据文件和日志文件进行校验检查。
	* `startup open`: 先执行“nomount”，然后执行“mount”，再打开包括Redo log文件在内的所有数据库文件，这种方式下可访问数据库中的数据。
	* `startup`: 相当于下面三条命令`startup nomount`、`alter database mount`、`alter database open`
	* `shutdown normal`: 正常方式关闭数据库
	* `shutdown immediate`: 立即方式关闭数据库，在SVRMGRL中执行shutdown immediate，数据库并不立即关闭，而是在Oracle执行某些清除工作后才关闭（终止会话、释放会话资源），当使用shutdown不能关闭数据库时，shutdown immediate可以完成数据库关闭的操作。
	* `shutdown abort`: 直接关闭数据库，正在访问数据库的会话会被突然终止，如果数据库中有大量操作正在执行，这时执行shutdown abort后，重新启动数据库需要很长时间。
* 查看当前连接数据库的用户
	* `show user`: USER为`SYS`
* 查看所有用户
	* `select * from dba_users;`   
	* `select * from all_users;`  
	* `select * from user_users;`
* 用户的切换	
	* 在登录的状态下输入: `conn 用户名/密码 [as sysdba]`
	* 加上`as sysdba`, 就会切换为管理员模式
* 查看用户下的表
	* `select * from tab;`
* 美化查询结果
	* `show linesize`: 查询行宽
	* `set linesize 300;`: 设置行宽
	* `set pagesize 20`: 设置一页显示20行记录
	* `column 列名 format a数字`: 设置列宽
		* column可以简写为`col`
		* format可以简写为`for`
* 超级管理员可以查询某个用户下的表
	* `select * from 用户名.表名`
* 查看表的结构
	* `desc 表名`
* 保存后面的操作到文本
	* `spool 文件名`
	* 关闭输出:`spool off`
* 注释
	* 使用`--`表示单行注释: `--这里是注释`
	* 使用`/*`和`*/`表示块注释
* 执行上一条SQL语句
	* 使用`/`
* 清屏
	* Windows
		* `host cls`
	* Linux
		* `host clear`
* 如果SQL语句出现错误, 可以修改sql语句
	* 首先输入错误的行数: `行数* sql语句`
	* 然后通过: `c /以前的错误字段/正确的字段`
	* 最后通过运行: `/`执行上一条语句

* <font color=red>SQL中的null值</font>
	* 包含null值的表达式都为`null`
	* sql语句中null永远!=null
		* 查看某个字段是否为null
			* 判断字段为空: `where 字段名 is null`
			* 判断字段不为空: `where 字段名 is not null`
	* MySQL中如果字段为空
		* 使用`ifnull(字段,0)`来控制
	* Oracle中如果字段为null, 可以使用下面的函数
		* 使用nvl, 如`nvl(字段名, 0)`
		* 使用nvl2
	* 如果集合中有null值, 不能使用`not in`, 可以使用`in`
	* <font color=red>排序中的空值问题</font>
		* 当排序时存在null时就会产生问题  
		* `nulls first` : 把空值放在前面
		* `nulls last`: 把空值放在后面
		* 例如: `select * from emp order by sal nulls first;`
	* 分组函数又叫多行函数(MySQL中叫聚合函数)会自动过滤null值
* 使用别名	
	* 方式1: `字段名 as "别名"`
	* 方式2: `字段名 "别名"`
	* 方式3: `字段名 别名`
	* 方式1和方式2没有区别
	* 方式3中不能出现空格、关键字等, 而且不能为纯数字
* 连接符
	* Oracle数据库中提供了`dual`的伪表, 它只是为了满足SQL99标准.
	* 可以使用`||`也可以使用`concat`
	* `select 'Hello' || ' World!' from dual;`
* Oracle对字符串大小写和日期敏感, 字符串和日期都需要使用单引号括起来
	* 日期默认格式是: `DD-MON-YY`
	* 查询默认日期格式: `select * from v$nls_parameters;`
	* 修改默认日期格式
		* 修改本次查询: `alter session set NLS_DATE_FORMAT = 'yyyy-MM-dd';`
		* 修改所有的: `alter system set NLS_DATE_FORMAT = 'yyyy-MM-dd';`
* 查看SQL执行时间
	* 打开SQL执行时间显示: `set timing on`
	* 关闭SQL执行时间显示: `set timing off`
* 查看SQL回写信息
	* 打开SQL回写信息: `set feedback on`
	* 关闭SQL回写信息: `set feedback off`
* ORACLE中执行SQL脚本
	* `@sql脚本路径`
* ORACLE中没有limit, 使用`rownum`的伪列
	* rownum永远使用默认的顺序生成
		* 如果需要更改行号, 需要使用`select`排序生成临时表, 然后再根据行号控制
		* 例如: `select rownum, empno, ename, sal from (select * from scott.emp e order by sal desc) where rownum <=3;`
	* rownum只能使用`<`、`=`和`<=`, 不能使用`>`和`>=`
	* <font color=red>ORACLE中的分页</font>
		* `select * from (select rownum r , el.* from (select * from scott.emp order by sal desc) el where rownum <= 8) where r >=5;`
* ORACLE中的`level`伪列, 是层级的伪劣

	
## <font color=orange> Oracle中的函数 </font>
* 单行函数
	* 字符函数
		* 变为大写: `select UPPER(列名|'字符串') from 表|伪表;`
		* 变为小写: `select LOWER(列名|'字符串') from 表|伪表;`
		* 首字符大写: `select INITCAP(列名|'字符串') from 表|伪表;`
		* 字符串连接: 可以使用`CONCAT`也可以使用`||`
		* 字符串截取: `select SUBSTR(列名|'字符串', fromIndex, length) from 表|伪表;`
			* fromIndex: 开始索引, 使用1和0效果相同
			* length: 长度
		* 字符串的字符数: `select LENGTH(列名|'字符串') from 表|伪表;`
		* 字符串的字节数: `select LENGTHB(列名|'字符串') from 表|伪表;`
		* 在字符串中查找另外一个字符串的位置: `select INSTR(列名|'字符串', 字符串1) from 表|伪表;`
			* 在列或者字符串中查找字符串1的位置
		* 左填充LPAD和右填充RPAD: `select LPAD(列名|'字符串', length, '字符串') from 表|伪表;`
			* length: 是表示填充后总长度
			* 字符串: 使用该字符串进行填充
		* 去掉前后指定的字符: `select TRIM('字符串1' from 列名|'字符串') from 表|伪表;`
			* 从后面的列或者字符串中, 前后去掉字符串1
		* 字符串替换: `select REPLACE(列名|'字符串', '字符串1', '字符串2') from 表|伪表;`
			* 字符串1: 是被替换的字符串
			* 字符串2: 是替换的字符串
	* 数值函数
		* 四舍五入函数: `select ROUND(12.534) from 表|伪表;`, 默认是保留整数
			* 指定保留的小数位: `select ROUND(12.534, 2) from 表|伪表;`, 保留2位小数
		* 取整: `select TRUNC(12.534) from 表|伪表;`, 默认去掉全部小数位
			* 指定保留的小数位: `select TRUNC(12.534, 2) from 表|伪表;`
		* 取余数: `select MOD(10, 3) from 表|伪表;`, 求10/3的余数
	* 日期函数, Oracle中提供了很多和日期相关的函数，包括日期的加减，在日期加减时有一些规律
		* 日期 – 数字 = 日期
		* 日期 + 数字 = 日期
		* 日期 – 日期 = 数字
		* 查看日期: `select to_char(sysdate, 'yyyy-MM-dd HH:mm:ss') from dual;`
		* 两个日期相差的月数: `select MONTHS_BETWEEN(列名1|'日期1', 列名2|'日期2') from 表|伪表;`
		* 向指定日期中加上若干月: `select ADD_MONTHS(列名|'日期', 月数) from 表|伪表;`
		* 下一个日期: `sleect NEXT_DAY(列名|'日期', '星期一') from 表|伪表;`, 下一个星期一
		* 本月最后一天: `select LAST_DAY(列名|'日期') from 表|伪表;`
		* 日期四舍五入: `select ROUND(列名|'日期', 'YEAR|MONTH') from 表|伪表;`
		* 日期截断: `select TRUNC(列名|'日期', 'YEAR|MONTH') from 表|伪表;`
	* 转换函数
		* 显示类型转换
			* TO_CHAR: 将其他类型转换为字符串
				* 将日期转为字符串: `TO_CHAR(列名|'日期', 'format')`
					* 例如: `select TO_CHAR(sysdate, 'yyyy-MM-dd HH:mm:ss') from dual;`
					* 自定义元素需要使用`""`括起来, 例如: `select TO_CHAR(sysdate, 'yyyy-MM-dd HH:mm:ss" Today is "day') from dual;`
				* 将数字转为字符串: `TO_CHAR(列名|数字, 'format')`
					* format通常有: 9---一个数字, 0---零, $---美元符, L---本地货币符号, .---小数点, ,---千分符
					* 例如: `select to_char(sal, 'L9,999.99') from scott.emp;`
			* TO_DATE: 将其他类型转为日期
			* TO_NUMBER: 将其他类型转为数字
		* 隐式数据转换, 以下数据类型ORACLE会自动转换
<center>

|源数据类型|目标数据类型|
|:--:|:--:|
|VARCHAR2 or CHAR|NUMBER|
|VARCHAR2 or CHAR|DATE|
| NUMBER |VARCHAR2|
| DATE |VARCHAR2|

</center>

* &emsp;
	* 通用函数, <font color=red>适用于任何类型, 也适合于空值</font>
		* NVL(expr1, expr2): expr1为空值, 返回expr2
		* NVL2(expr1, expr2, expr3): expr1为空时返回expr3, 否则返回expr2
		* NULLIF(expr1, expr2): 如果expr1和expr2相等, 返回空值, 否则返回expr1
		* COALESCE(expr1, expr2,...,exprn): 从左到右找出第一个不为null的值
	* 条件表达式
		* CASE表达式: SQL99标准
			* `select empno, ename, job, sal 涨前, case job when 'PRESIDENT' then sal+1000  when 'MANAGER' then sal+800 else sal+400 end 涨后 from scott.emp;`
			* `select empno, ename, job, sal 涨前, case when sal<1000 then sal+1000  when sal>1000 then sal+800 else sal+400 end 涨后 from scott.emp;`
		* DECODE函数: ORACLE自己的语法, 类似Java
			* `decode(列名|expr, search1, result1, search2, result2,...,searchn,resultn, default)`
			* 例如: `select empno, ename, job, sal 涨前, decode(job, 'PRESIDENT',sal+1000, 'MANAGER', sal+800, sal+400) 涨后 from scott.emp;`
		
* 多行函数, 分组函数(MySQL中叫聚合函数)
	* AVG: 求平均值
	* COUNT: 计数
	* MAX: 求最大值
	* MIN: 求最小值
	* SUM: 求和
	* WM_CONCAT: 行转列
	* `group by`: ORACLE中包含在select中的列而未在分组函数中使用的列必须包含在`group by`中, 而group by中列, 可以不必包含在select中
	* `having`中可以使用多行函数, `where中不能使用多行函数`
	* `group by`的增强: `select deptno, job, sum(sal) from scott.emp group by rollup(deptno,job);`
		* `break on deptno`: deptno值相同的只显示一次
		* `skip 2`: 不同的分组, 跳过两格
		* `break on null`: 取消设置
	* 多表查询
		* ORACLE中特有
			* 右连接: `select d.deptno  bumenghao, d.dname bumengminchen, count(e.empno) renshu from scott.emp e, scott.dept d where e.deptno(+)=d.deptno group by d.deptno, d.dname;`
			* 左连接: `select d.deptno  bumenghao, d.dname bumengminchen, count(e.empno) renshu from scott.emp e, scott.dept d where e.deptno=d.deptno(+) group by d.deptno, d.dname;`
			* 自连接: 通过同一张表的不同别名, 将一张表视为多张表
			* 层次查询: `select level, empno, ename, mgr from scott.emp connect by prior empno=mgr start with mgr is null;`
				* level: 是一个伪列
				* connect by 是连接条件
				* start wiht 是起始条件
		* SQL99语法, 不仅仅在ORACLE中使用, 也可以在其他使用该标准的数据库中使用
			* 1、交叉连接（CROSS JOIN）: 用于产生笛卡尔积
				* `SELECT * FROM scott.emp CROSS JOIN scott.dept;`
			* 2、自然连接（NATURAL JOIN）：自动找到匹配的关联字段，消除掉笛卡尔积
				* `SELECT * FROM scott.emp NATURAL JOIN scott.dept;`
			* 3、JOIN…USING子句：用户自己指定一个消除笛卡尔积的关联字段
				* `SELECT * FROM scott.emp JOIN scott.dept USING(deptno);`
			* 4、JOIN…ON子句：用户自己指定一个可以消除笛卡尔积的关联条件
				* `SELECT * FROM scott.emp e JOIN scott.dept d ON(e.deptno=d.deptno);`
			* 5、连接方向的改变：
				* 左（外）连接：LEFT OUTER JOIN…ON;
					* `SELECT * FROM scott.emp e LEFT OUTER JOIN scott.dept d ON(e.deptno=d.deptno);`
				* 右（外）连接：RIGHT OUTER JOIN…ON;
					* `SELECT * FROM scott.emp e RIGHT OUTER JOIN scott.dept d ON(e.deptno=d.deptno);`
				* 全（外）连接：FULL OUTER JOIN…ON; --> 把两张表中没有的数据都显示
					* `SELECT * FROM scott.emp e FULL OUTER JOIN scott.dept d ON(e.deptno=d.deptno);`
	* 子查询
		* 合理的书写风格
		* 括号
		* 可以在主查询的where、select、having、from后面使用子查询
		* 不可以在group by后面使用子查询
		* 强调from 后面的子查询
		* 主查询和子查询可以是不同表, 只要子查询返回的结果主查询可以使用
		* 一般不在子查询中排序, 但top-n分析问题中, 必须对子查询排序
		* 一般先执行子查询, 再执行主查询
		* 单行子查询只能使用单行操作符, 多行子查询只能使用多行操作符
		* 注意子查询中的null问题
	* 相关子查询
		* 将主查询的参数传递给子查询
		* 例如这里的`e.deptno`都是将主查询结果传递给子查询: `select empno , ename, sal, (select avg(sal) from scott.emp where deptno=e.deptno) avgsal from scott.emp e where sal > (select avg(sal) from scott.emp where deptno=e.deptno);`
	* 集合运算
		* 注意事项
			* 参与运算的各个集合必须列数相同, 并且类型一致
			* 采用第一个集合作为最后的表头
			* order by永远在最后面
			* 可以使用()
		* 并集
			* UNION: 公共部分只要一份
			* UNION ALL: 公共部分全部都要
		* 交集
			* INTERSECT
		* 差集
			* MINUS
	* <font color=red>使用地址符&</font>
		* 在ORACLE中可以使用地址符, 快速执行SQL语句
		* 例如: `INSERT into scott.emp(empno, ename, sal, deptno) values(&empno, &ename, &sal, &deptno);`
			* 然后会提示输入, empno、ename、sal和deptno的值
			* 如果再次需要插入, 只需执行`/`, 再次输入empno、ename、sal和deptno的值即可
		* 例如: `SELECT * from &table;`
			* 会提示输入表
			* 如果希望再次查询其他表, 只需执行`/`, 再次输入表名即可
	* SQL的一些技巧
		* 快速创建一个新表Account, 结构和user一样: `create table Account as select * from user where 1=2;`
		* 使用子查询快速插入多条数据: `insert into account(username, password,email, name, sex, birthday ,valid,registTime) select username, password,email, name, sex, birthday ,valid,registTime from user;`
			* 可以不必写`VALUES`
			* 子查询中的值应该和INSERT中的值对应
			* 如果字段完全都是表中字段 可以后面直接使用`*`, 前面不用写字段名
		* 海量插入数据时
			* 数据泵(PLSQL程序, 需要使用dbms_datapump)
			* SQL*Loader工具
			* 外部表
	
	
## <font color=orange> Oracle中的事务 </font>
* ORACLE中事务起始标志: 第一条DML语句(update、insert和update)
* ORACLE中事务结束标志
	* 提交
		* 显示: commit
		* 隐式: exit退出、DDL语句和DCL语句
	* 回滚
		* 显示: rollback
		* 隐式: 非正常退出(掉电、宕机、命令行退出)
* ORACLE只支持`Read committed`和`Serializable `, 这两种事务, 默认是`Read committed`.
## <font color=orange> Oracle中的数据类型 </font>
	
|No|数据类型|	描述|
|:--:|:--:|:--:|
|1|Varchar， varchar2 |	可变长字符数据|
|2|CHAR(size) |	定长字符数据|
|3|NUMBER|	NUMBER(n)表示一个整数，长度是n, NUMBER(m,n):表示一个小数，总长度是m，小数是n，整数是m-n, Number(10,2):整数部分占 8 位，小数部分占2份|
|4|DATE|	表示日期类型   |
|5|CLOB	|大对象，表示大文本数据类型，可存4G|
|6|BLOB|	大对象，表示二进制数据，可存4G|
|7|LONG|	可变长字符数据，最大2G|
|8|RAW and LONG RAW| 原始二进制数据|
|9|BFILE|存储外部文件的二进制文件, 最大4G|
|10|ROWID|行地址, 它也是一个伪列|
	
## <font color=orange> SQL的优化 </font>
* <font color=red>SQL尽量使用列名, 而不要直接使用*来进行查询</font>
* oracle中会先把小写字母转成大写字母, 所以<font color=red>SQL语句用大写</font>
* <font color=red>oracle中where解析的顺序, 从右往左, 尽量把过滤多的条件放在右边</font>
* 尽量使用where语句, 减少使用having语句
	* having是先分组后过滤
	* where是先过滤再分组
* 尽量不要使用集合运算
	* UNION ALL 比UNION 高效