TunSecur Project
================

Open source security software developed by St2Py team

## About TunSecur

**TunSecur** is an open source secure tunnel software. It uses **RSA** + **AES** technologies to ensure **the army level** network communication security. 
It supports both TCP and UDP secure tunnel, used to encrypt network communication such as http proxy, socks proxy, dns server etc. 
At present, it supports both password and RSA authentication. We plan to add database authentication in the future.

It is developed by using Golang, supports **Linux**, **Windows**, **OSX**, **FreeBSD** four main operating systems.

For more details, please go to the project page [http://st2py.com/en/tunsecur/](http://st2py.com/en/tunsecur/)

## Project License

The MIT License (MIT)

Copyright (c) 2015 St2Py


## 关于 TunSecur

**TunSecur** 是一款开源安全隧道软件。它使用 **RSA** + **AES** 加密技术，确保**军事级别**的网络通信安全。
它同时支持TCP和UDP安全隧道，可用于HTTP代理、SOCKS代理、DNS服务器等网络通信的加密。
当前，它支持密码认证和RSA证书认证两种认证方式。未来，我们还计划添加数据库认证方式。

它使用Go语言开发，支持 **Linux**，**Windows**，**OSX**，**FreeBSD** 四大主流操作系统。

更多详情，请移步项目主页 [http://st2py.com/cn/tunsecur/](http://st2py.com/cn/tunsecur/)

## 项目许可

The MIT License (MIT)

Copyright (c) 2015 St2Py

##

TunSecur是Tunnel Security的缩写，是一款开源安全隧道软件，由客户端和服务器端两部分组成，具有以下特点：
采用MIT协议发布，完全开源，完全免费
采用Go语言开发，安全、高效、易于部署
支持密码认证、RSA认证两种认证方式
支持RSA 1024、2048、4096三种长度的密钥
支持AES 128、192、256三种长度的密钥
支持CFB、CTR、OFB三种迭代模式
支持 TCP、UDP两种传输协议
支持Linux、Windows、OSX、FreeBSD四大系统
适用场景
TunSecur适用于需要安全网络通信的所有场景，包括但不限于下列情况：
安全DNS代理
安全SSH代理
安全VPN代理
安全Web代理
安装步骤
TunSecur采用Go语言开发，请在安装本软件前确保已经装好了Go语言开发环境。详情可参考：Install the Go tools
本地安装
1
go get -u github.com/st2py/tunsecur
跨平台编译
Go语言具有强大的跨平台编译能力：
1
2
3
4
5
6
7
8
9
10
11
12
13
14
15
16
17
18
git clone https://github.com/st2py/tunsecur.git
cd tunsecur

OS="linux darwin freebsd windows"
ARCH="386 amd64"
for GOOS in $OS
do
    for GOARCH in $ARCH
    do
        echo Building tunsecur for $GOOS $GOARCH
        go build -o $GOOS-$GOARCH-tunsecur *.go
        
    done
done

GOOS=linux GOARCH=arm
echo Building tunsecur for $GOOS $GOARCH
go build -o $GOOS-$GOARCH-tunsecur *.go
简明教程
TunSecur的通信过程涉及到4个端点，分别是用户端、客户端、服务端、目标端：
用户端是真正的通信发起端点，如Web浏览器、DNS客户端等
客户端是用户端连接到的端点，即Tunnel Client
服务端是客户端连接到的端点，即Tunnel Server
目标端是最终的通信接收端点，如Web网站，DNS服务器
其中，用户端和客户端、以及服务端和目标端之间的网络通信，都是未加密的；只有客户端和服务端之间的通信是加密通信。这点类似于IPSec VPN协议。具体的TunSecur通信构架如下：

查看帮助
1
tunsecur -h
生成RSA密钥对
1
2
3
4
5
# -gen 生成密钥 -bit 指定密钥长度，可选值为 1024，2048，4096 #
tunsecur -gen -bit 2048

# -dir 指定保存密钥的目录 #
tunsecur -gen -bit 2048 -dir some/directory
TunSecur会生成public.pem和private.pem两个文件：前者是公钥，用于Tunnel Client端；后者是私钥，用于Tunnel Server端。
注意：请妥善备份好这两个文件，丢失密钥对会导致无法使用RSA认证方式连接Tunnel Server！
Tunnel 运行参数
-ts 设置为 Tunnel 服务端模式
-tc 设置为 Tunnel 客户端模式
-tcp 设置为 TCP Tunnel
-udp 设置为 UDP Tunnel
-lp 设置侦听端口 Host:Port
-remote 设置转发端口 Host:Port
-passwd 设置为密码认证模式
-pub 设置为RSA认证模式，Tunnel客户端的RSA公钥文件
-priv 设置为RSA认证模式，Tunnel服务端的RSA私钥文件
-aes 设置AES加密模式，可选 ctr，cfb，ofb
-len 设置AES密钥长度，可选 16，24，32
-log 设置日志级别，默认只输出错误日志，设为7则输出所有级别的日志
TCP Tunnel
服务端：
1
tunsecur -ts -tcp -passwd test123 -remote 8.8.8.8:53 -lp 0.0.0.0:12345
客户端：
1
tunsecur -tc -tcp -passwd test123 -remote 127.0.0.1:12345 -lp 0.0.0.0:53
UDP Tunnel
服务端：
1
tunsecur -ts -udp -passwd test123 -remote 8.8.8.8:53 -lp 0.0.0.0:12345
客户端：
1
tunsecur -tc -udp -passwd test123 -remote 127.0.0.1:12345 -lp 0.0.0.0:53
密码认证模式
服务端：
1
tunsecur -ts -tcp -passwd test123 -remote 8.8.8.8:53 -lp 0.0.0.0:12345
客户端：
1
tunsecur -tc -tcp -passwd test123 -remote 127.0.0.1:12345 -lp 0.0.0.0:53
RSA认证模式
首先使用 tunsecur -gen 命令产生RSA公钥文件和私钥文件。其中，私钥文件用于服务端，公钥文件用于客户端。
服务端：
1
tunsecur -ts -tcp -priv private.pem -remote 8.8.8.8:53 -lp 0.0.0.0:12345
客户端：
1
tunsecur -tc -tcp -pub public.pem -remote 127.0.0.1:12345 -lp 0.0.0.0:53
工作原理
密码认证模式
在密码认证模式下，TunSecur客户端的加密过程如下：
客户端和服务端约定好 flag 为 0x23571719
客户端和服务端约定好 password
客户端产生随机的AES密钥种子 tx_seed_key, rx_seed_key 和 向量种子 tx_seed_iv, rx_seed_iv
客户端采用 AES-CTR-128 算法 和 password 加密 (flag, tx_seed_key, tx_seed_iv, rx_seed_key, rx_seed_iv)
客户端把加密的 (flag, tx_seed_key, tx_seed_iv, rx_seed_key, rx_seed_iv) 发送给服务端
客户端用 tx_seed_key 和 tx_seed_iv 生成AES密钥，加密用户端数据，发送给服务端
客户端用 rx_seed_key 和 rx_seed_iv 生成AES密钥，解密服务端数据，发送给用户端
在密码认证模式下，TunSecur服务端的加密过程如下：
服务端和客户端约定好 flag 为 0x23571719
服务端和客户端约定好 password
服务端接收客户端发来的加密种子 (flag, tx_seed_key, tx_seed_iv, rx_seed_key, rx_seed_iv)
服务端采用 AES-CTR-128 算法 和 password 解密 (flag, tx_seed_key, tx_seed_iv, rx_seed_key, rx_seed_iv)
服务端验证解密得到的 flag 和约定的 flag 是否相等：如果不相等认证失败，断开连接；如果相等则继续
服务端用 tx_seed_key 和 tx_seed_iv 生成AES密钥，解密客户端数据，发送给目标端
服务端用 rx_seed_key 和 rx_seed_iv 生成AES密钥，加密目标端数据，发送给客户端
RSA认证模式
在RSA认证模式下，TunSecur客户端的加密过程如下：
客户端和服务端约定好 flag 为 0x23571719
客户端和服务端约定好 RSA公钥和私钥
客户端产生随机的AES密钥种子 tx_seed_key, rx_seed_key 和 向量种子 tx_seed_iv, rx_seed_iv
客户端采用 RSA公钥文件加密 (flag, tx_seed_key, tx_seed_iv, rx_seed_key, rx_seed_iv)
客户端把加密的 (flag, tx_seed_key, tx_seed_iv, rx_seed_key, rx_seed_iv) 发送给服务端
客户端用 tx_seed_key 和 tx_seed_iv 生成AES密钥，加密用户端数据，发送给服务端
客户端用 rx_seed_key 和 rx_seed_iv 生成AES密钥，解密服务端数据，发送给用户端
在RSA认证模式下，TunSecur服务端的加密过程如下：
服务端和客户端约定好 flag 为 0x23571719
服务端和客户端约定好 RSA公钥和私钥
服务端接收客户端发来的加密种子 (flag, tx_seed_key, tx_seed_iv, rx_seed_key, rx_seed_iv)
服务端采用 RSA私钥文件 解密(flag, tx_seed_key, tx_seed_iv, rx_seed_key, rx_seed_iv)
服务端验证解密得到的 flag 和约定的 flag 是否相等：如果不相等认证失败，断开连接；如果相等则继续
服务端用 tx_seed_key 和 tx_seed_iv 生成AES密钥，解密客户端数据，发送给目标端
服务端用 rx_seed_key 和 rx_seed_iv 生成AES密钥，加密目标端数据，发送给客户端
架设安全DNS代理
前提条件：必须要有一个国外的VPS服务器，国内的无法防止DNS污染和DNS劫持。具体原因不解释。
强烈推荐： Linode 或 DigitalOcean
Ubuntu VPS服务器上架设Tunnel Server：
sudo tunsecur -ts -udp -passwd your_password -remote 8.8.8.8:53 -lp 0.0.0.0:15353 &
sudo netstat -antupl | grep tunsecur | grep 15353
个人电脑上架设Tunnel Client：
tunsecur -tc -udp -passwd your_password -remote your_vps_ip:15353 -lp 127.0.0.1:53
设置个人电脑DNS服务器为 127.0.0.1：
Windows、Linux、Mac OSX设置各不相同，自己搜索看怎么设置吧。
架设安全Web代理
前提条件：必须要有一个国外的VPS服务器，国内的无法防止TCP重置和MITM。具体原因不解释。
强烈推荐： Linode 或 DigitalOcean
Ubuntu VPS服务器上架设Tunnel Server：
sudo apt-get install polipo -y
sudo vim /etc/polipo/config
其中，/etc/polipo/config 内容如下：
# This file only needs to list configuration variables that deviate
# from the default values.  See /usr/share/doc/polipo/examples/config.sample
# and "polipo -v" for variables you can tweak and further information.

logSyslog = true
logFile = /var/log/polipo/polipo.log
proxyAddress = 127.0.0.1
proxyPort = 18123
maxAge = 7d
maxExpiresAge = 9d
dnsNameServer = 8.8.8.8
daemonise = true
disableLocalInterface = true
allowedPorts = 80,443
tunnelAllowedPorts = 80,443

sudo polipo
sudo netstat -antupl | grep polipo | grep 18123
sudo tunsecur -ts -tcp -passwd your_password -remote 127.0.0.1:18123 -lp 0.0.0.0:18080 &
sudo netstat -antupl | grep tunsecur | grep 18080
个人电脑上架设Tunnel Client：
tunsecur -tc -tcp -passwd your_password -remote your_vps_ip:18080 -lp 127.0.0.1:18080
设置个人电脑Web代理服务器为 127.0.0.1:18080：
Windows、Linux、Mac OSX设置各不相同，自己搜索看怎么设置吧。
