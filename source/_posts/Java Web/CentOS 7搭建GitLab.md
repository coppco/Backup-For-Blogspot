---
layout: post
title: CentOS 7搭建GitLab
comments: true
date: 2017-07-07 17:06:02
tags:
	- Java
	- Linux
	- CentOS
	- GitLab
	- git
	- 资料整理
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
* 支持的Linux系统
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
#打开ssh
sudo yum install curl policycoreutils openssh-server openssh-clients 
#ssh开机启动
sudo systemctl enable sshd 
#开启ssh
sudo systemctl start sshd 
#安装右键系统
sudo yum install postfix
#开机启动 
sudo systemctl enable postfix 
#开启右键系统
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
wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-8.8.5-ce.0.el7.x86_64.rpm

rpm -ivh gitlab-ce-8.8.5-ce.0.el7.x86_64.rpm
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
	* 5.2、修改unicorn端口, 默认使用`80`和`8080`, 如果被占用可以修改
``` 
unicorn['listen'] = '127.0.0.1'
unicorn['port'] = 18080
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
<img src="http://oak4eha4y.bkt.clouddn.com/centos_gitlab_setting.png" alt="进入设置" style="width: 70%; height:100px; text-align: center; display: block;"/>
</center>
<center>
<img src="http://oak4eha4y.bkt.clouddn.com/centos_gitlab_disableSignUp.png" alt="去掉Sign-up enabled前面的勾" style="width: 70%; text-align: center; display: block;"/>
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
	* 查看`https://gitcafe.com/larryli/gitlab`, 看是否有该版本的分支(截止到今天, 最新的汉化包是8.8.5版本)
	* 如果没有就不能汉化, 有的话即可汉化

* 新建一个文件夹并进入

```
mkdir -p /root/gitlab_cn
cd /root/gitlab_cn
```

* 克隆GitLab仓库

```
git clone https://gitlab.com/larryli/gitlab.git
#或者gitcafe镜像, 速度快点
git clone https://gitcafe.com/larryli/gitlab.git
```
* 获取汉化补丁(注意版本)

```
git diff origin/8-8-stable origin/8-8-zh > /tmp/8.8.diff
```

* 停止GitLab并执行汉化
```
gitlab-ctl stop
cd /opt/gitlab/embedded/service/gitlab-rails
git apply /tmp/8.8.diff
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

## <font color=orange>GitLab的备份、恢复和卸载</font>
待续

* 卸载GitLab

```
sudo gitlab-ctl uninstall
sudo rpm -e gitlab-ce
find / -name gitlab|xargs rm -rf
```
* 卸载后再次安装会卡在`ruby_block[supervise_redis_sleep] action run`
	* 解决方法
		* 执行:`systemctl restart gitlab-runsvdir`
		* 运行: `gitlab-ctl reconfigure`