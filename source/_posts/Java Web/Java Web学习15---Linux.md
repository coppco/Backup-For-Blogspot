---
layout: post
title: Java Web学习15---Linux
comments: true
toc: true
date: 2016-11-05 09:04:52
tags:
	- Java
	- Linux
---

Linux是基于Unix的一种自由和开放源码的操作系统，存在着许多不同的Linux版本，但它们都使用了Linux内核。Linux可安装在各种计算机硬件设备中，比如手机、平板电脑、路由器、台式计算机.

<!--more-->

## <font color=orange> Linux </font>
 
今天各种场合都有使用各种Linux发行版，从嵌入式设备到超级计算机，并且在服务器领域确定了地位，通常服务器使用LAMP（Linux + Apache + MySQL + PHP）或LNMP（Linux + Nginx + MySQL + PHP）组合。

* Linux系统的应用
	* 服务器系统
		* Web应用服务器
		* 数据库服务器
		* 接口服务器
		* DNS
		* FTP等
	* 嵌入式系统
		* 路由器
		* 防火墙
		* 手机
		* PDA
		* IP分享器
		* 交换器
		* 家电用品的微电脑控制器等
	* 高性能运算、计算密集型应用
		* 桌面应用系统
		* 移动手持系统
* Linux的版本
	* 内核版本
		* 内核版本是指在Linus领导下的内核小组开发维护的系统内核的版本号
	* 发行版本
		* 发行版本是一些组织和公司根据自己发行版的不同而自定的
		* 目前市面上较知名的发行版有：Ubuntu、RedHat、CentOS、Debian、Fedora、SuSE、OpenSUSE、TurboLinux、BluePoint、RedFlag、Xterm、SlackWare等。

### <font color=orange>安装 Linux </font>
可以安装在电脑上面, 也可以使用虚拟机安装.

* 虚拟机软件
	* VmWare	   : 收费的
	* VirtualBox : 免费的, Oracle公司的
* CentOS: [下载地址](https://www.centos.org/download/)
	* CentOS-x.x-x86_64-DVD-1503-01.iso : 标准安装版，一般下载这个就可以了（推荐）
	* CentOS-x.x-x86_64-NetInstall-1503-01.iso : 网络安装镜像（从网络安装或者救援系统）
	* CentOS-x.x-x86_64-Everything-1503-01.iso: 对完整版安装盘的软件进行补充，集成所有软件。（包含centos7的一套完整的软件包，可以用来安装系统或者填充本地镜像）
	* CentOS-x.x-x86_64-GnomeLive-1503-01.iso: GNOME桌面版
	* CentOS-x.x-x86_64-KdeLive-1503-01.iso: KDE桌面版
	* CentOS-x.x-x86_64-livecd-1503-01.iso : 光盘上运行的系统，类拟于winpe
	* CentOS-x.x-x86_64-minimal-1503-01.iso : 精简版，自带的软件最少
* 安装 CentOS
	* <font color=orange>选择`Install CentOS 7`</font>
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centOS_1.png" alt="选择Install CentOS 7" style="width: 70%; text-align: center; display: block;"/>
</center>
	* <font color=orange>语言选择</font>`中文`----`简体中文(中国)`----`继续`, <font color=red>在此选择的语言会称为操作系统的默认语言</font>
<center>	
<img src="http://oak4eha4y.bkt.clouddn.com/centos_2.png" alt="语言选择" style="width: 70%; text-align: center; display: block;"/>
</center>
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_5.png" alt="主设置界面" style="width: 70%; text-align: center; display: block;"/>
</center>

	* <font color=orange>日期和时间</font>
		* 选择一个计算机物理位置最近的时区, 即使你使用NTP(网络时间协议)也需要选择正确的时区.
		* 可以使用上面的列表来选择, 也可以鼠标选择图中区域来选择
		* 使用网络时间, 首先需要当前系统已经联网, 并且点击后面的设置按钮`添加并标记为使用NTP服务器`
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_3.png" alt="日期和时间" style="width: 70%; text-align: center; display: block;"/>
</center>
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_4.png" alt="设置NTP" style="width: 70%; text-align: center; display: block;"/>
</center>
	* <font color=orange>键盘</font>(K), 添加一个或多个键盘类型作为系统键盘, 第一个位置将作为默认键盘.
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_6.png" alt="键盘设置" style="width: 70%; text-align: center; display: block;"/>
</center>	
	* <font color=orange>语言支持</font>(L), 选择需要安装的额外支持语言	
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_7.png" alt="语言支持" style="width: 70%; text-align: center; display: block;"/>
</center>	
	* <font color=orange>安装源</font>, 选择安装源安装系统的位置
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_8.png" alt="安装源" style="width: 70%; text-align: center; display: block;"/>
</center>	

		* 自动检测安装介质: 使用完整DVD或者USB驱动器启动安装时, 安装程序会检查到它, 并显示在此选项下的基本信息. 点击`验证`以确保适合于安装.
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_9.png" alt="安装源" style="width: 70%; text-align: center; display: block;"/>
</center>	
		* iSO文件: 如果安装程序检测到一个分区的硬盘驱动器挂载的文件系统, 此选项会出现.选择ISO文件,  点击`验证`以确保适合于安装.
		* 在网络上: 从网络上选择下载地址, 可以选择`http://`、`https://`、`ftp://`和`nfs`
	* <font color=orange>软件选择</font>, 选择一个软件包, 默认是`最小安装`
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_10.png" alt="软件选择" style="width: 70%; text-align: center; display: block;"/>
</center>	
	
		* 最小安装: 这个选项只提供运行CentOS 的基本软件包。最小安装为单一目的服务器提供基本需要，并可在这样的安装中最大化性能和安全性
		* 基础设施服务器: 这个选项提供在服务器中使用的CentOS 基本安装，不包含桌面。
		* 文件及打印服务器: 用于企业的文件、打印及存储服务器。
		* 基本网页服务器: 基本系统平台，加上PHP，Web server，还有MySQL和PostgreSQL数据库的客户端，无桌面。
		* 虚拟化主机: 这个选项提供 KVM 和 Virtual Machine Manager 工具以创建用于虚拟机器的主机。
		* 带GUI的服务器: 带有用于操作网络基础设施服务GUI的服务器。
		* GNOME桌面: GNOME是一个非常直观且用户友好的桌面环境。
		* KDE Pasma Workspaces(KDE桌面): 一个高度可配置图形用户界面，其中包括面板、桌面、系统图标以及桌面向导和很多功能强大的KDE应用程序。
		* 开发及生成工作站(Development and Creative Workstation): 这个选项提供在您的CentOS 编译软件所需的工具。
	* <font color=orange>网络和主机名</font>, 本地访问接口由安装程序会自动检测到的接口都列在左侧窗格中。单击列表中的界面显示在右侧的详细信息。要启动或关闭网络接口，将开关在屏幕的右上角为ON或OFF。
		* 主机名可以是完全限定域名（FQDN）格式hostname.domainname或主机名格式 的短主机名.
		* 许多网络有动态主机配置协议（DHCP）服务，它提供自动连接系统与域名。要允许DHCP服务的域名分配给这台机器，只有指定的短主机名.
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_11.png" alt="网络和主机名" style="width: 70%; text-align: center; display: block;"/>
</center>	
		
		* 如果需要手动配置网络
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_12.png" alt="手动配置网络" style="width: 70%; text-align: center; display: block;"/>
</center>				
	* <font color=orange>安装位置</font>
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_13.png" alt="安装位置" style="width: 70%; text-align: center; display: block;"/>
</center>	
	
		* 本地标准磁盘: 本地的硬盘
		* 专用磁盘 & 网络磁盘: 添加额外的专用或网络设备.
		* <font color=red>分区</font>
			* 自动配置分区: 系统自动分区
			* 我想让额外空间可用: 只能在自动配置分区中使用, 但是空间不足, 可以使用这个来释放空间.
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_14.png" alt="我想让额外空间可用" style="width: 70%; text-align: center; display: block;"/>
</center>	

			* <font color=red>我要配置分区</font>: 手动分区
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_17.png" alt="手动分区" style="width: 70%; text-align: center; display: block;"/>
</center>	
				* `/root`: 挂载在/boot中的分区, 包含操作系统内核以及自我引导过程中的文件, 一般是xfs格式的分区, 500M足够大多数用户使用.
				* `/`: 根目录, 默认情况下所有文件都会写入该分区
				* `/var`: 包含了大量应用程序, 同时还保存了临时保存下载的更新软件包
				* `swap`: 被用来支持虚拟内存, 当内存不足时, 数据会写入swap分区, 如果需要安装Oracle, 至少需要3GB
				* `/home`: 用于用户保存系统中的数据,可以在不擦除用户数据的情况下安装或升级CentOS系统.
		* <font color=red>加密</font>, 对`/root`以外的分区进行加密
	* 开始安装
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_15.png" alt="开始安装" style="width: 70%; text-align: center; display: block;"/>
</center>	

		* 安装过程中可以设置root用户密码和创建新用户, 安装的时间取决于选择的软件安装包多少
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_16.png" alt="安装过程中" style="width: 70%; text-align: center; display: block;"/>
</center>	
	* 安装完成后重启, 需要LICENSING
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_18.png" alt="LICENSING" style="width: 70%; text-align: center; display: block;"/>
</center>		
* `ssh 用户名@host`, 例如: `ssh root@1.2.3.4`
* 使用VirtualBox安装虚拟机后, 网络需要选择`桥接网卡`方式, 才能正确获取虚拟机的IP地址
	
### <font color=orange> Linux目录结构 </font>
* bin: 存放二进制可执行文件
* sbin: 存放二进制可执行文件, 只有root才能访问
* <font color=red>etc</font>: 存放系统配置文件
* <font color=red>usr</font>: 用于存放系统共享的系统资源
* <font color=red>home</font>: 存放用户文件的根目录(普通用户)
* <font color=red>root</font>: 超级用户目录(root管理员的home目录在root中)
* dev: 用于存放设备文件
* lib: 存放跟文件系统中的程序运行所需要的共享库以及内科模块
* mnt: 系统管理员安装临时文件系统的安装点
* boot: 存放用于系统引导时使用的各种文件
* tmp: 用于存放各种临时文件
* var: 用于存放运行时需要改变数据的文件

### <font color=orange> Linux常用命令 </font>
* <font color=red>命令使用帮助, 对于不会使用的命令使用下面</font>
	* 方式1: 使用`命令 --help`查看, 例如: `ls --help`
	* 方式2: 使用`man 命令`查看, 例如: `man ls`
		* 退出帮助目录: `q`
* <font color=red>切换目录命令: cd</font>
	* `cd ..`: 切换到上一层目录
	* `cd /`: 切换到系统根目录
	* `cd ~`: 切换到用户主目录
	* `cd -`: 切换到上一个所在目录
* <font color=red>创建目录命令: mkdir</font>
	* `mkdir iOS`: 在当前目录下创建iOS目录
	* `mkdir -p iOS/Apps`: 级联创建iOS以及Apps目录
* <font color=red>删除目录命令: rmdir</font>
	* `rmdir iOS`: 删除iOS目录
	* `rmdir -p iOS/Apps`: 可以迭代删除, 不为空的目录会先删除里面的目录
* <font color=red>列出文件列表命令: ls</font>
	* `ls`: 显示文件夹或文件
	* `ls -a`: Linux系统以`.`开头的文件都是隐藏文件, 该命令会显示隐藏文件
	* `ls -l`: 也可以简写为`ll`, 以列表的形式展示文件或文件夹(显示较多信息) 
		* `ls -lh`或`ll -h`: 友好的方式显示文件(文件大小会使用k、m、g方式显示)
* <font color=red>浏览文件</font>
	* cat: 显示文件的所有内容, 文件太大不推荐使用
	* more: 分页显示文件内容
		* enter: 下一行
		* 空格: 下一页(到底了会自动退出)
	* less: 分页显示文件内容
		* 上下键: 可以查看内容
		* q: 退出
	* <font color=red>tail</font>: 查看文件的后面内容
		* `tail -20 文件路径`: 查看该文件最后20行内容
		* `tail -f 文件路径`: 动态查看内容
			* 通过`ctrl + c`结束
* <font color=red>查看ip地址</font>
	* `ifconfig`: 可以查看ip地址等 
	* `ip addr`: 可以查看ip地址等 
* <font color=red>下载</font>
	* `wget 下载地址`: 下载文件
* <font color=red>显示当前所在目录</font>
	* `pwd`: 显示当前所在目录
* <font color=red>文件操作</font>
	* `touch 文件名`: 创建一个空白的文件
	* `cp 文件 目录/文件名`: 复制文件
	* `mv 文件 目录/文件名`: 移动文件(重命名)
	* `rm 文件名/文件夹`: 删除文件或文件夹
		* 默认会有提示框, 是否删除
		* `rm -f 文件名/文件夹`: 不会出现确认框
		* `rm -r 文件/目录`: 带询问的递归删除
		* `rm -rf 文件/目录`(危险): 不带询问的递归删除
* <font color=red>打包或解压</font>
tar命令位于/bin目录下，它能够将用户所指定的文件或目录打包成一个文件，但不做压缩。一般Linux上常用的压缩方式是选用tar将许多文件打包成一个文件，再以gzip压缩命令压缩成xxx.tar.gz(或称为xxx.tgz)的文件。
	* 常用参数：
		* -c：创建一个新tar文件
		* -v：显示运行过程的信息
		* -f：指定文件名
		* -z：调用gzip压缩命令进行压缩
		* -t：查看压缩文件的内容
		* -x：解开tar文件
	* 常用组合
		* -cvf: 打包一个文件, 并指定tar包名
		* -zcvf: 打包并压缩tar包, 格式是gzip
			* `tar -zcvf App.tar.gz 文件/文件路径`: 路径可以有多个, 中间使用空格隔开
		* -xvf: 解开tar文件
			* `tar -xvf App.tar.gz [-C 路径]`: 如果没有路径, 默认是当前文件夹
* <font color=red>查找符合条件的字符串</font>
	* `grep 字符串 文件名`: 在文件中查找符合条件的字符串
	* 常见参数
		* `--color`: 高亮显示
		* `-A行数`: 后面显示多少行
		* `-B行数`: 前面显示多少行
	* `grep url yum.conf --color -A4`: 在文件中查找符合条件的字符串, 高亮显示, 后面显示4行
* 防火墙
	* 永久开启8080端口: firewall-cmd --add-port=8080/tcp --zone=public --permanent
	* 查询所有已经开启的端口: firewall-cmd --zone=public --list-ports
	* 移除8080端口:firewall-cmd --permanent --zone=public --remove-port=8080/tcp
	* 刷新防火墙使设置生效: firewall-cmd --reload
	* 重启防火墙: sudo systemctl restart firewalld
	
### <font color=orange> Vi和Vim编辑器 </font>
在Linux下一般使用vi编辑器来编辑文件。vi既可以查看文件也可以编辑文件。默认系统没有安装 VIM ，你可以自己安装,<font color=red>在终端以root身份输入`yum install -y vim`等待安装完成即可</font>.
* 三种模式：
	* 命令行
		* 切换到命令行模式: `ESC`键
	* 插入模式
		* 切换到命令行模式: `i`、`o`、`a`键
		* `i`:  在当前位置生前插入
    	* `I`:  在当前行首插入
    	* `a`:  在当前位置后插入
    	* `A`:  在当前行尾插入
    	* `o`:  在当前行之后插入一行
    	* `O`:  在当前行之前插入一行
	* 底行模式, 需要先切换到命令行模式
		* 切换到底行模式: `:`键
* vim常用命令
	* 打开文件：`vim file名`
	* 退出：`esc`,然后 `:q`
	* 修改文件：输入`i`进入插入模式
	* 保存并退出：`esc`, 然后 `:wq`
	* 不保存退出：`esc`, 然后 `:q!`
	* 强制退出: `:q!`或`:wq!`
* 快捷键
	* `dd`: 快速删除一行
	* `R`: 替换
	* `yy`: 复制一行
	* `p`: 粘贴
	
### <font color=orange> 重定向输出`>`和`>>` </font>
* `>`: 重定向输出, 覆盖原有功能
* `>>`: 重定向输出, 追加在内容后面
* 例如: `ifconfig > 3.txt`, 把内容输出到3.txt中

### <font color=orange> 命令执行控制&& </font>
命令之间使用 && 连接，实现逻辑与的功能。
 
* 只有在 && 左边的命令返回真（命令返回值 $? == 0），&& 右边的命令才会被执行。 
* 只要有一个命令返回假（命令返回值 $? == 1），后面的命令就不会被执行。
* 例如: `mkdir test && cd test`

### <font color=orange> 管道 | </font>
管道是Linux命令中重要的一个概念，其作用是<font color=red>将一个命令的输出用作另一个命令的输入</font>。
* 例如: `ifconfig | grep 192.168 -color`, 在ifconfig结果中查找`192.168`
* 查找java相关的进程: `ps -ef | grep java`
* 查找和3306相关的信息: `ps -ef | grep 3306`

### <font color=orange> 网络通讯命令 </font>
* `ifconfig`: 显示或设置网络设备。如果该命令不存在: 运行`yum -y install net-tools`
	* `ifconfig`: 显示网络设备
	* `ifconfig eth0 up`: 启用eth0网卡
	* `ifconfig eth0 down`:  停用eth0网卡
* `ping ip地址或网址`:  探测网络是否通畅。
	* 参数
		* `-c数量`: ping多少次
	* `ping 192.168.1.1 -c10`: ping地址192.168.1.1, 10次
* `netstat`:  查看网络端口。
	* `netstat -an | grep 3306`: 查询3306端口占用情况

### <font color=orange> 加密相关命令 </font>
* md5sum: 查看文件的md5值
	* `md5sum 文件名`: 文件的md5值
	* `md5sum 文件名 > md5.txt`: 获取md5值并写到md5.txt中
* sha1sum: 查看文件的sha1值
	* `sha1sum 文件名`: 文件的sha1值
	* `sha1sum 文件名 > sha1.txt`: 获取sha1值并写到sha1.txt中

### <font color=orange> 系统管理命令 </font>
* date: 显示或设置系统时间
	* `date`:  显示当前系统时间
	* `date -s “2014-01-01 10:10:10“`:  设置系统时间
* `df`: 显示磁盘信息
	* `df –h`:  友好显示大小
* `free`:  显示内存状态
	* `free –m`:  以mb单位显示内存组昂头
* `fdisk -l`: 硬盘使用情况
* `top`:  显示/管理执行中的程序
* `clear`:  清屏幕
* `ps`:  正在运行的某个进程的状态
	* `ps –ef`:  查看所有进程
	* `ps –ef | grep ssh`:  查找某一进程
* `kill`: 杀掉某一进程
	* `kill 2868`:  杀掉pid为2868的进程
	* `kill -9 2868`:  强制杀死进程
* `du`:  显示目录或文件的大小。
	* `du –h`: 显示当前目录的大小
* `who`:  显示目前登入系统的用户信息。
* `hostname`:  查看当前主机名
	* 修改主机名: `vi /etc/sysconfig/network`
* `uname`:  显示系统信息。
	* `uname -a`:  显示本机详细信息。
		* 依次为：内核名称(类别)，主机名，内核版本号，内核版本，内核编译日期，硬件名，处理器类型，硬件平台类型，操作系统名称

### <font color=orange> 用户管理 </font>
* 添加用户
	* `useradd test`:  添加test用户
	* `useradd test -d /home/t1`:  指定用户home目录
* 设置密码
	* `passwd test`: 为test用户设置密码
* 删除用户
	* `userdel test`: 只删除用户, 不删除home里面的目录
	* `userdel -r test`:  删除用户, home目录也会删除
* 切换用户
	* `su - 用户名`: 常用方法, 用户环境也会切换到该用户
	* `ssh -l 用户名 -p 端口 主机ip地址`
* 查看用户信息
	* `id 用户名`: 查看用户信息等
* 将现有用户添加到另外一个组里面, 不改变之前的组, -a是append, 就是将用户添加到新用户组中而不必离开原有的其他用户组
	* `usermod -a -g 组名 用户名`
* 更改用户的主要用户组
	* `usermod -g 组名 用户名`
* 将用户从组里面删除, 前提是该组不上用户的主要组
	* `gpasswd -d user名 group名`
### <font color=orange> 用户组管理 </font>
* 创建组
	* `groupadd 组名`: 添加组
* 用户添加组
	* `useradd 用户名 -g 组名`: 添加用户并设置组
	* 当在创建一个新用户user时，若没有指定他所属于的组，就建立一个和该用户同名的私有组
* 删除组
	* `groupdel 组名`: 如果组里面有用户, 无法删除

## <font color=orange> 文件权限 </font>
Linux系统文件的权限由文件类型(第一个字符)、属主权限(第二个到第四个)、属组权限(第五个到第七个)和其他用户权限(第八个到第十个)组成.如`-rwxr--r--`.
* 权限rwx-
	* `r`: 读权限, 值对应4
	* `w`: 写权限, 值对应2
	* `x`: 可执行权限, 值对应1
	* `-`: 没有相关权限
* 文件类型
	* <font color=red>`-`: 普通文件</font>
	* <font color=red> `d`: 目录</font>
	* `l`: 符号链接
* 文件权限管理
	* `chmod`:  变更文件或目录的权限, 前提是该文件/文件夹属于当前用户
		* `chmod 755 a.txt`: 更改权限 
		* `chmod u=rwx,g=rx,o=rx a.txt`: 更改当前用户、组和其他用户的权限
		* `chmod 000 a.txt`: 所有用户都没权限
		* `chmod 777 a.txt`: 所有用户读写执行权限
	* `chown`:  变更文件或目录改文件所属用户和组
		* `chown 用户名:组名 a.txt`: 变更当前的目录或文件的所属用户和组
		* `chown -R 用户名:组名 dir`: 变更目录中的所有的子目录及文件的所属用户和组
