---
layout: post
title: CentOS 7搭建GitLab
comments: true
toc: true
date: 2017-04-07 17:06:02
tags:
	- Java
	- Linux
	- CentOS
	- GitLab
	- git
	- 资料整理
	- 持续集成
---

&emsp;&emsp;CentOS 是一个基于Red Hat Linux 提供的可自由使用源代码的企业级Linux发行版本。CentOS是开源的, 完全免费的, 最新版本是CentOS 7。CentOS是Community Enterprise Operating System的缩写。

&emsp;&emsp;GitLab 是一个用于仓库管理系统的开源项目，使用Git作为代码管理工具，并在此基础上搭建起来的web服务。

<!--more-->

## <font color=orange>GitLab</font>
&emsp;&emsp;与SVN相比, GitLab同样提供完美的用户权限管理, 与Git相比, 除了涵盖Git所有功能, 同时又提供方便的后台管理, 非常适合企业使用.和Github相比, 因为有墙的关系国内访问速度很慢, github的公开项目免费，私有项目收费,最低`$7/month`可以拥有5个私有项目。

* 国内提供代码托管服务的网站
	* [阿里云持续交付平台](https://crp.aliyun.com)
	* [码云](https://git.oschina.net)

## <font color=orange>GitLab的安装</font>
* 支持的[Linux系统](https://about.gitlab.com/installation/)
	* Omnibus package installation (recommended)(综合包安装(推荐))
		* Ubuntu
		* Debian
		* CentOS 6、7
		* OpenSUSE
		* Raspberry Pi 2 
	* Other official installation methods(其他官方安装方法)
		* Docker 
		* Kubernetes
		* RedHat OpenShift等 
		
## <font color=orange>CentOS 7安装GitLab</font>

* 1、系统防火墙中打开HTTP、SSH访问和邮件系统(如果不使用postfix, 可以不安装postfix)
```
#安装ssh
sudo yum install curl openssh-server openssh-clients -y
#ssh开机启动
sudo systemctl enable sshd 
#开启ssh
sudo systemctl start sshd 
#安装policycoreutils-python(10.x以后版本需要)
sudo yum install policycoreutils policycoreutils-python -y
#安装邮件系统
sudo yum install postfix -y
#开机启动 
sudo systemctl enable postfix 
#开启邮件系统
sudo systemctl start postfix 
#开启http访问
sudo firewall-cmd --permanent --add-service=http
#重新加载防火墙 
sudo systemctl reload firewalld
```
* 2、添加GitLab软件包服务器并安装软件包
	* 方式1: 由于墙的问题, 下载速度可能很慢, 推荐第二种方式
```
curl -sS https://packages.gitlab.com/install/repositories/gitlab/gitlab-ce/script.rpm.sh | sudo bash
 
sudo yum -y install gitlab-ce
```
	* 方式2: 使用清华大学开源软件镜像站(默认安装最新版), 新建`vi /etc/yum.repos.d/gitlab-ce.repo`,内容如下:
```
[gitlab-ce] 
name=gitlab-ce 
baseurl=http://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7 
repo_gpgcheck=0 
gpgcheck=0 
enabled=1 
gpgkey=https://packages.gitlab.com/gpg.key
```
		* 然后运行
			* `sudo yum makecache`
			* `sudo yum install gitlab-ce`
	* 方式3: 直接下载rpm包, 进行安装

```
wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-9.3.0-ce.0.el7.x86_64.rpm

rpm -ivh gitlab-ce-9.3.0-ce.0.el7.x86_64.rpm
```

* 4、读取配置文件并安装
	* 4.1、`sudo gitlab-ctl reconfigure`
	* 4.2、在我使用CentOS 7.2实际安装时会出现<font color=red>`Error executing action run on resource execute[semodule -i /opt/gitlab/embedded/selinux/rhel/7/gitlab-7.2.0-ssh-keygen.pp]`</font>的错误
		* 4.2.1、解决方法: 运行: `yum install libsemanage-static libsemanage-devel`
		* 4.2.2、再次运行: `sudo gitlab-ctl reconfigure`
* 5、其他配置, GitLab默认会有一个配置包括host、端口、默认git存放路径等
	* GitLab的配置都是在`/etc/gitlab/gitlab.rb`中进行配置
	* 5.1、配置访问host和端口
```
external_url '192.168.1.188:8888'
```
	* 5.2、修改unicorn端口, 默认使用`80`和`8080`, 如果被占用可以修改, 也不能修改为9090, 该端口是普罗米修斯监控的默认端口.
``` 
unicorn['listen'] = '127.0.0.1'
unicorn['port'] = 18080
#和上面的端口一样
gitlab_workhorse['auth_backend'] = "http://localhost: 18080"
```
	* 5.3、修改默认仓库位置
```
#简单的设置
git_data_dir "/data/git"
#复杂的设置
git_data_dirs({
    "default" => {
        "path" => "/data/git",
        "failure_count_threshold" => 10,
        "failure_wait_time" => 30,
        "failure_reset_time" => 1800,
        "storage_timeout" => 5
    }
})
```
	* 5.4、配置邮箱提醒, `vi /etc/gitlab/gitlab.rb`
```
#阿里云邮箱, 具体的后缀名根据各个申请可能不一样
gitlab_rails['smtp_enable'] = true
gitlab_rails['smtp_address'] = "smtp.mxhichina.com"
gitlab_rails['smtp_port'] = 25
gitlab_rails['smtp_user_name'] = "gitlab@ilanni.com"
gitlab_rails['smtp_password'] = "ilanni6543"
gitlab_rails['smtp_domain'] = "ilanni.com"
gitlab_rails['smtp_authentication'] = "login"
gitlab_rails['smtp_enable_starttls_auto'] = true
gitlab_rails['smtp_tls'] = false
gitlab_rails['gitlab_email_from'] = "gitlab@ilanni.com"

#163邮箱设置, 对于设置了安全码的, 把安全码放在密码的位置
gitlab_rails['smtp_enable'] = true 
gitlab_rails['smtp_address'] = "smtp.163.com" 
gitlab_rails['smtp_port'] = 25 
gitlab_rails['smtp_user_name'] = "xxx@163.com" 
gitlab_rails['smtp_password'] = "xxx" 
gitlab_rails['smtp_domain'] = "163.com" 
gitlab_rails['smtp_authentication'] = :login 
gitlab_rails['smtp_enable_starttls_auto'] = true
gitlab_rails['smtp_tls'] = false
gitlab_rails['gitlab_email_from'] = "xxx@163.com" 
```
	* 5.5、设置不能注册(需要管理员账户登录)
		* 设置不能注册后, 可以配置好邮箱设置, 创建用户不设置密码, 那么会自动发送注册邮件到设置的邮箱, 让用户设置密码.
		* 对于人员想对较多的可以打开注册, 设置注册域的白名单
<center>
<img src="http://47.96.147.179/images/java/centos_gitlab_setting.png" alt="进入设置" style="width: 70%; height:100px; text-align: center; display: block;"/>
</center>
<center>
<img src="http://47.96.147.179/images/java/centos_gitlab_disableSignUp.png" alt="去掉Sign-up enabled前面的勾" style="width: 70%; text-align: center; display: block;"/>
</center>
	* 5.6、修改配置文件后需要重新加载配置文件和重启
```
sudo gitlab-ctl reconfigure
sudo gitlab-ctl restart
```

## <font color=orange>GitLab的汉化, CentOS 6.x</font>
* 首先查看GitLab版本

```
cat /opt/gitlab/embedded/service/gitlab-rails/VERSION
```

* 查看是否有该版本的汉化补丁
	* 查看`https://gitlab.com/larryli/gitlab.git`, 看是否有该版本的分支(截止到今天, 最新的汉化包是8.8.5版本)
	* 查看`https://gitlab.com/xhang/gitlab.git`, 延续Larry Li项目的8-8-zh中文版本进行更新，目前最新版本是9.3.0 
	* 如果没有就不能汉化, 有的话即可汉化

* 新建一个文件夹并进入

```
mkdir -p /root/gitlab_cn
cd /root/gitlab_cn
```

* 克隆GitLab仓库, 根据版本选择对应的仓库

```
git clone https://gitlab.com/larryli/gitlab.git
#或者gitcafe镜像, 速度快点
git clone https://gitcafe.com/larryli/gitlab.git
```
* 获取汉化补丁(注意版本)

```
#根据分支
git diff origin/8-8-stable origin/8-8-zh > /tmp/8.8.diff

#根据tag
# 查看tag版本，选择合适的汉化版本
git tag
# 对比不同，这里比较的是tag，v9.3.0为英文原版，v9.3.0-zh为汉化版本。diff结果是汉化补丁。
git diff v9.3.0..v9.3.0-zh > /tmp/9.3.0.diff
```

* 停止GitLab并执行汉化
```
gitlab-ctl stop
cd /opt/gitlab/embedded/service/gitlab-rails
git apply --whitespace=warn /tmp/8.8.diff
```
* 启动gitlab

```
sudo gitlab-ctl start
```

## <font color=orange>GitLab的汉化, CentOS 7.x</font>
* 下载补丁
```
git clone https://git.oschina.net/qiai365/gitlab-L-zh.git
```
* 切换版本
```
cd gitlab-L-zh
git checkout -b 8-5-zh origin/8-5-zh
cp -r /opt/gitlab/embedded/service/gitlab-rails{,.ori}
```
* 汉化操作
```
gitlab-ctl stop

yes|cp -rf ../gitlab-L-zh/* /opt/gitlab/embedded/service/gitlab-rails/

gitlab-ctl start
```

## <font color=orange>GitLab的备份、恢复、卸载和一些问题</font>
待续

### <font color=orange>验证码邮箱配置是否正确</font>
* 进入控制台: 运行`gitlab-rails console`
* 出现`Loading production environment (Rails 4.2.8)
irb(main):001:0>`之后执行`Notify.test_email('收件人邮箱', '邮件标题', '邮件正文').deliver_now`

### <font color=orange>备份</font>
* 配置文件
	* 运行: `sh -c 'umask 0077; gitlab_version=$(sudo cat /opt/gitlab/embedded/service/gitlab-rails/VERSION); tar Pcf /var/opt/gitlab/backups/$(date "+etc-gitlab${gitlab_version}-%s.tar") /etc/gitlab'`
	* 上面命令会把`/etc/gitlab`中文件、版本号和时间戳备份在: `/var/opt/gitlab/backups/etc-gitlabx.x.x-xxxxxx.tar`中
* 数据文件
	* 默认备份地址是: `/var/opt/gitlab/backups`
	* 手动创建备份: `sudo gitlab-rake gitlab:backup:create`
	* 日常备份: 添加`crontab`
		* 1、运行`crontab -e`
		* 2、如下配置:
```
# 每天5点执行备份
0 5 * * * /opt/gitlab/bin/gitlab-rake gitlab:backup:create CRON=1
```
	* 修改备份周期和备份目录
		* 修改配置文件: `vi /etc/gitlab/gitlab.rb`
		* 修改下面配置文件: 
```
# 设置备份周期为7天 - 604800秒
gitlab_rails['backup_keep_time'] = 604800
# 备份目录
gitlab_rails['backup_path'] = '/var/opt/gitlab/backups'
```
		* 修改完配置文件后还需要重新加载文件,重启

### <font color=orange>恢复</font>
确保备份的Gitlab版本和需要恢复的Gitlab版本一致才能恢复.

* 恢复配置文件
	* 运行: `sudo tar -xvf /var/opt/gitlab/backups/etc-gitlab-1511768198.tar -C  /etc/gitlab`
	* 该命令会把tar压缩文件解压到`/etc/gitlab`中, 注意替换你的备份文件地址

* 恢复数据文件
	* 停止连接数据库的进程
```
sudo gitlab-ctl stop unicorn
sudo gitlab-ctl stop sidekiq
```
	* 恢复备份数据文件, 将覆盖GitLab数据库！
```
#1512314152是1512314152_gitlab_backup.tar中的前面时间戳
sudo gitlab-rake gitlab:backup:restore BACKUP= 1512314152
```
	* 启动GitLab
```
sudo gitlab-ctl start
```
	* 检查 GitLab
```
sudo gitlab-rake gitlab:check SANITIZE=true
```

### <font color=orange>卸载</font>

```
sudo gitlab-ctl uninstall
sudo rpm -e gitlab-ce
find / -name gitlab|xargs rm -rf
```
* 卸载后再次安装会卡在`ruby_block[supervise_redis_sleep] action run`
	* 解决方法
		* 执行:`systemctl restart gitlab-runsvdir`
		* 运行: `gitlab-ctl reconfigure`
	
### <font color=orange>项目的git地址不对问题</font>
* 运行: `vi /var/opt/gitlab/gitlab-rails/etc/gitlab.yml`
* 修改里面的host地址为服务器ip地址

## <font color=orange>配置SSH</font>

### <font color=orange>Mac新建SSH</font>
1、在终端运行下面的命令: 注意邮箱换成你自己的邮箱
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
2、会让你输入SSH的存储地方以及安全密钥, 我们使用默认好了, 直接回车下一步
3、最后会在`/Users/你的用户名/.ssh`下生成`id_rsa`、`id_rsa.public`
4、如果你之前运行过上面的命令, 该目录中会存在上诉文件, 可以直接使用`id_rsa.public`中的内容

### <font color=orange>Window新建SSH</font>
1、安装git, Open Git Bash, 运行下面的命令: 注意邮箱换成你自己的邮箱
```
ssh-keygen -t rsa -b 4096 -C "your_email@example.com"
```
2、会让你输入SSH的存储地方以及安全密钥, 我们使用默认好了, 直接回车下一步
3、最后会在`c:/Users/你的用户名/.ssh`下生成`id_rsa`、`id_rsa.public`
4、如果你之前运行过上面的命令, 该目录中会存在上诉文件, 可以直接使用`id_rsa.public`中的内容

## <font color=orange>git用户名和邮箱配置</font>
### <font color=orange>全局配置</font>
```
git config --global user.name "xxxxx"
git config --global user.email "xxxxx@xx.com"
```
Mac会在`/Users/你的用户名`下生成`.gitconfig`的文件.
### <font color=orange>单个项目配置</font>
```
git config user.name "xxxxx"
git config user.email "xxxxx@xx.com"
```
### <font color=orange>查看配置</font>
```
#查看当前配置, 在当前项目下面查看的配置是全局配置+当前项目的配置, 使用的时候会优先使用当前项目的配置
git config --list
```

# <font color=orange>通过SSH连接gitlab时报如下错误</font>

```
ssh_exchange_identification: read: Connection reset by peer
```

## <font color=orange>解决方案</font>
```
1) 修改/etc/ssh/sshd_config中#MaxStartups 10:30:60，将其改为MaxStartups 1000

2) 重启SSH服务，/etc/init.d/ssh restart或者service ssh restart

Centos系统默认连接时间120秒，如果远程终端连接数过多，则会出现超时连接，解决办法如下：

1) 修改/etc/ssh/sshd_config中LoginGraceTime 120,将其改为LoginGraceTime 0，其中0表示不限制连接时间

2) 重启SSH服务，/etc/init.d/ssh restart或者service ssh restart
```

## <font color=orange>如果gitlab装在阿里云服务器上面</font>

有的时候阿里云检测到我们的gitlab某个ip流量过大会认为是Web攻击, 所以需要为相应的阿里云ECS添加我们的公网ip地址.
或者在阿里云的Linux服务器中, 编辑`/etc/hosts.allow`和`/etc/hosts.deny`这两个文件, 出现
```
all:all:deny
```
需要注释掉, 或者直接在`/etc/hosts.allow`中添加
```
sshd:all
#或者
all:允许的ip地址
```
在`/etc/hosts.deny`中添加的ip地址为对应解决的ip地址.

# <font color=orange>gitlab服务器占内存过大</font>
可以参考[官网配置](https://gitlab.com/gitlab-org/omnibus-gitlab/blob/master/doc/settings/unicorn.md), 把进程数改小点, 最低需要2个.
```
unicorn['worker_processes'] = 2
unicorn['worker_timeout'] = 60
```