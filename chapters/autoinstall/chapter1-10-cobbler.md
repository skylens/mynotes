# [Cobbler](https://cobbler.github.io/) 安装配置

### 环境 CentOS 7

* 准备

1. 禁用 `selinux` 

```bash
setenforce 0;sed -i '/SELINUX/s/enforcing/disabled/' /etc/selinux/config;sestatus
```

2. 设置 `firewalld` 或者禁用

```bash
firewall-cmd --add-service={tftp,http} --permanent
firewall-cmd --add-port={25150/tcp,25151/tcp} --permanent
firewall-cmd --relaod
```

禁用

```bash
systemctl disable firewalld;systemctl stop firewalld
```

3. 设置静态 IP 地址

```bash

IPADDR=192.168.99.254
GATEWAY=192.168.99.254
NETMASK=255.255.255.0
DNS1=114.114.114.114
DNS2=223.5.5.5

/etc/init.d/network restart
```

4. 设置软件源，并安装必要软件

```bash
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.orig
curl -o /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo
yum makecache
yum install -y vim net-tools epel-release tmux bash-completion lrzsz nano wget git
```

5. 设置时间同步

查看同步情况，并检查时间

```bash
yum -y install chrony;systemctl start chronyd;systemctl status chronyd
chronyc sources -v
timedatectl
```

6. 修改主机名

```bash
hostnamectl set-hostname centos7.skylens.co
```

* 安装 Cobbler

[参考](https://cobbler.github.io/manuals/quickstart/)        [参考](http://www.zyops.com/autoinstall-cobbler/)

[参考](https://www.ibm.com/developerworks/cn/linux/l-cobbler/index.html)        [参考](https://www.jianshu.com/p/2e62cead05f4)

[参考](http://www.cnblogs.com/shhnwangjian/p/5858900.html)        [参考](https://www.centoshowtos.org/installation/kickstart-and-cobbler/)

```bash
yum install cobbler cobbler-web pykickstart httpd tftp dhcp xinetd fence-agents debmirror
```

* 配置

+ setting 配置

生成加密密码

```bash
openssl passwd -1
Password: 123456
Verifying - Password: 123456
$1$ruR7Phop$ra.MuVkEfBKuRQ7v.pAK9.
```

修改为如下内容

```bash
vim /etc/cobbler/setting

manage_dhcp: 1

manage_dns: 1

pxe_just_once: 1

server: 192.168.99.254

next_server: 192.168.99.254

# 使用 openssl 生成的加密密码
default_password_crypted: $1$ruR7Phop$ra.MuVkEfBKuRQ7v.pAK9.
```

+ dhcp 配置

修改如下内容

```bash
vim /etc/cobbler/dhcp.template

subnet 192.168.99.0 netmask 255.255.255.0 {
     option routers             192.168.99.254;
     option domain-name-servers 192.168.99.254;
     option subnet-mask         255.255.255.0;
     range dynamic-bootp        192.168.99.100 192.168.99.250;
     default-lease-time         21600;
     max-lease-time             43200;
     next-server                $next_server;

```

+ debmirror 配置

注释如下内容

```bash
vim /etc/debmirror.conf

# @dists=sid
# @arches=i386
```

+ tftp 配置

/etc/xinetd.d/tftp

`disable` 设置为 `no`

+ 下载启动文件

```bash
cobbler get-loaders
```

+ 启动服务，检查配置，更新配置

```bash
systemctl start httpd ; systemctl enable httpd
systemctl start cobblerd ; systemctl enable cobblerd
systemctl start rsyncd ; systemctl enable rsyncd
systemctl start xinetd ; systemctl enable xinetd

cobbler check

cobbler sync
```

* 管理 cobbler

+ 管理 distro

```bash
mount /dev/cdrome /mnt

cobbler import --path=/mnt/ --name=CentOS-7.1 --arch=x86_64
// 查看 distro
cobbler distro list
// 查看 distro 细节
cobbler distro report --name=CentOS-7.1-x86_64
```

+ 管理 profile

```bash
// 查看 profile， 导入 distro 会自动生成 profile
cobbler profile list

cobbler profile edit --name=CentOS-7.1-x86_64 --kickstart=/var/lib/cobbler/kickstarts/CentOS-7.1-x86_64.cfg
cobbler profile edit --name=CentOS-7.1-x86_64 --kopts='net.ifnames=0 biosdevname=0'
```

+ 管理 system

```bash
// 添加一个 system 对象
cobbler system add --name=test --profile=CentOS-7.1-x86_64
// 编辑指定 test 的参数 (指定接口、mac地址、ip地址、子网掩码、dns服务器、网关、主机名)
cobbler system edit --name=test --interface=eth0 --mac=00:11:22:AA:BB:CC --ip-address=192.168.99.100 --netmask=255.255.255.0 --static=1 --dns-name=test.mydomain.com --gateway=192.168.1.1 --hostname=test.mydomain.com
```

+ ks 文件

```
install
url --url=$tree
text
lang en_US.UTF-8
keyboard us
zerombr
bootloader --location=mbr --driveorder=sda --append="crashkernel=auto rhgb quiet"
# Network information
$SNIPPET('network_config')
timezone --utc Asia/Shanghai
authconfig --enableshadow --passalgo=sha512
rootpw  --iscrypted $default_password_crypted
clearpart --all --initlabel
part /boot --fstype xfs --size 1024
part swap --size 1024
part / --fstype xfs --size 1 --grow
firstboot --disable
selinux --disabled
firewall --disabled
logging --level=info
reboot

%pre
$SNIPPET('log_ks_pre')
$SNIPPET('kickstart_start')
$SNIPPET('pre_install_network_config')
# Enable installation monitoring
$SNIPPET('pre_anamon')
%end

%packages
```