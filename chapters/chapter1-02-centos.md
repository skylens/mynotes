# CentOS/Redhat/Oracle Linux 安装后的设置

__安装完 CentOS/RedHat/Oracle Linux 以 `root` 登录__ 

1.网络配置及DNS

```bash
# ip addr    # 查看网卡信息
# cd /etc/sysconfig/network-scripts/
# cp ifcfg-enp0s3 ifcfg-enp0s3.bak    # 备份配置文件
# vi ifcfg-enp0s3

TYPE=Ethernet
BOOTPROTO=static
DEFROUTE=yes
PEERDNS=yes
PEERROUTES=yes
IPV4_FAILURE_FATAL=no
IPV6INIT=yes
IPV6_AUTOCONF=yes
IPV6_DEFROUTE=yes
IPV6_PEERDNS=yes
IPV6_PEERROUTES=yes
IPV6_FAILURE_FATAL=no
NAME=eno16777736
UUID=be068d11-d1de-4342-9f6f-96aaa53b957f
DEVICE=eno16777736
ONBOOT=yes
IPADDR=192.168.2.7
GATEWAY=192.168.2.2
NETMASK=255.255.255.0
DNS1=114.114.114.114
DNS2=202.203.132.100

# /etc/init.d/network restart    # 重启网络服务
# ip addr    # 查看IP地址是否正确
# ping -c 3 www.163.com    # 测试网路连通性及DNS
```

2.更新一下源

```bash
# yum list
# yum update
```

3.安装 epel 源

```bash
# yum install epel-release
# yum update
```

4.安装常用软件

```bash
# yum install -y sudo vim bash bash-completion nano tmux ntpdate tar zip unzip
```

5.设置NTP同步时间

```bash
# ntpdate 202.108.6.95
# date    # 查看时间
```

6.关闭SELinux

```bash
$ sestatus
$ vi /etc/selinux/config
SELINUX=permissive
$ setenforce permissive
```

7.其他
