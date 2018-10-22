---
layout: post
title: CentOS 7安装SS服务端以及相关客户端
comments: true
toc: true
date: 2018-04-19 14:40:11
tags:
	- Shadowsocks
	- VPS
	- 翻墙
---

最近入手一个相对便宜的($8.5/年)[VPS服务器](https://www.neq3host.com), 支持支付宝支付, IP是纽约的, 延时大约300ms, 对于我这种要求不是很高的用户来说足够了, 用途么你懂的^.^

<!--more-->

## <font color=orange>安装CentOS服务端ss程序</font>
* 安装系统
	* 购买好之后VPS之后, 我们安装好操作系统, 选择CentOS 7.
* 安装SS服务
	* 修改系统时间
```
data -s '19/04/2018 14:40:11' //这里改成当前时间
hwclock -w  //将修改后的时间写入硬件
```
	* 安装ss服务
```
wget --no-check-certificate -O shadowsocks-libev.sh https://raw.githubusercontent.com/teddysun/shadowsocks_install/master/shadowsocks-libev.sh

chmod +x shadowsocks-libev.sh

./shadowsocks-libev.sh 2>&1 | tee shadowsocks-libev.log
```
		* 安装过程中会提示设置密码、端口和加密方式(加密方式推荐chacha20, 速度快安全).
* 启动、停止、重启和状态命令
	* 启动：`/etc/init.d/shadowsocks start`
	* 停止：`/etc/init.d/shadowsocks stop`
	* 重启：`/etc/init.d/shadowsocks restart`
	* 查看状态：`/etc/init.d/shadowsocks status`

## <font color=orange>安装BBR加速服务</font>
BBR简称TCP-BBR拥塞控制算法，目的是要尽量跑满带宽, 并且尽量不要有排队的情况, 提升VPS网速效果明显。BBR算法出自谷歌.
* 更新系统&&安装内核
```
yum update -y
rpm --import https://www.elrepo.org/RPM-GPG-KEY-elrepo.org
rpm -Uvh http://www.elrepo.org/elrepo-release-7.0-2.el7.elrepo.noarch.rpm
yum --enablerepo=elrepo-kernel install kernel-ml
grub2-set-default 0
```
* 重启系统
```
reboot
```
* 配置tcp
```
#编辑配置文件
vi /etc/sysctl.conf
#加入如下两行配置信息
net.core.default_qdisc = fq
net.ipv4.tcp_congestion_control = bbr
```
* 运行BBR服务
```
#运行BBR服务
sysctl -p
#查看BBR服务是否生效
lsmod | grep bbr
```

## <font color=orange>相关客户端</font>
* iOS可以使用PP助手下载安装`Shadowrocket`
* Mac和Windows: 使用`Shadowsocks客户端`

### <font color=orange>CentOS 7使用ss客户端</font>
* 安装epel源、安装pip包管理
```
sudo yum -y install epel-release
sudo yum -y install python-pip
```
* 安装Shadowsocks客户端
```
sudo pip install shadowsocks
```
* 配置Shadowsocks连接
```
{
    # Shadowsocks服务器地址
    "server":"x.x.x.x",
    # Shadowsocks服务器端口
    "server_port":1035,
    # 本地IP
    "local_address": "127.0.0.1",
    # 本地端口
    "local_port":1080,
    # Shadowsocks连接密码  
    "password":"password",
    # 等待超时时间 
    "timeout":300,  
    # 加密方式
    "method":"aes-256-cfb",  
    # true或false。开启fast_open以降低延迟，但要求Linux内核在3.7+
    "fast_open": false,  
    #工作线程数 
    "workers": 1  
}
```
* 配置自启动 
	* `vi /etc/systemd/system/shadowsocks.service`
	* 添加以下内容
```
[Unit]
Description=Shadowsocks
[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/sslocal -c /etc/shadowsocks/shadowsocks.json
[Install]
WantedBy=multi-user.target
```
* 开机启动、启动、重启、停止、状态
	* 开机启动: `systemctl enable shadowsocks.service`
	* 启动: `systemctl start shadowsocks.service`
	* 重启: `systemctl restart shadowsocks.service`
	* 停止: `systemctl stop shadowsocks.service`
	* 状态`systemctl status shadowsocks.service`
	
* 如果加密方式选择的是`chacha20`, 会启动不成功, 需要编译 libsodium 以支持 chacha20 加密方式
	* 安装相关开发工具
```
yum groupinstall "Development Tools" -y
yum install wget -y
```
	* 下载 libsodium 最新版本
```
#官网地址
wget https://download.libsodium.org/libsodium/releases/LATEST.tar.gz
```
	* 解压
```
tar xzvf LATEST.tar.gz
```
	* 进入解压文件夹, 生成配置文件
```
cd libsodium*

./configure
```
	* 编译并安装
```
make -j8 && make install
```
	* 添加运行库位置并加载运行库
```
echo /usr/local/lib > /etc/ld.so.conf.d/usr_local_lib.conf

ldconfig
```
* 验证Shadowsocks客户端服务是否正常运行
```
#会返回你的Shadowsock服务器IP
curl --socks5 127.0.0.1:1080 http://httpbin.org/ip
```
* 安装配置privoxy,Shadowsocks使用的socks5协议,而终端很多工具目前只支持http和https等协议，所以我们要用工具把socks5转成http协议。
```
yum install privoxy -y
systemctl enable privoxy
systemctl start privoxy
systemctl status privoxy
```
* 配置privoxy, `vi /etc/privoxy/config`
```
listen-address 127.0.0.1:8118 # 8118 是默认端口，不用改
forward-socks5t / 127.0.0.1:1080 . #转发到本地端口，注意最后有个点, 端口是上面ss安装时的本地端口
```
* 设置http、https代理, `vi /etc/profile`在最后添加如下信息
```
PROXY_HOST=127.0.0.1
export http_proxy=http://$PROXY_HOST:8118
export https_proxy=http://$PROXY_HOST:8118
```
* 重载环境变量
```
source /etc/profile
```
* 测试代理
```
curl -I www.google.com 
#查看ip信息
curl ip.cn
```
* 取消使用代理
```
while read var; do unset $var; done < <(env | grep -i proxy | awk -F= '{print $1}')
```