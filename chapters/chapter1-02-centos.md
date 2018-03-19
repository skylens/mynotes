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

6.修改 SELinux (permissive 不需要重启，disable 需要重启)

```bash
# sestatus   //查看 SELinux 的状态 
# setenforce permissive   //临时修改为 permissive 
# sed -i 's/^SELINUX=.*/SELINUX=permissive/g' /etc/sysconfig/selinux   //修改配置文件
# cat /etc/sysconfig/selinux   //查看修改情况
```

7.关闭和禁用防火墙

```bash
# systemctl stop firewalld
# systemctl disable firewalld
```

9.修改主机名

```bash
// www.example.com 为主机名
# hostname www.example.com   //临时修改
# vim /etc/sysconfig/network   //永久修改
// 以及修改 hosts 文件，logout 登出，重新登录查看
# hostname -fqdn   //查看 fqdn
```

10.其他