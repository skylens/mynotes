# PXE+Kickstart部署无人值守安装

* 准备

    + PXE 服务器网络配置
    
    ```bash
    # cat /etc/sysconfig/network-scripts/ifcfg-eno16777736
    
    TYPE="Ethernet"
    BOOTPROTO="none"
    DEFROUTE="yes"
    IPV4_FAILURE_FATAL="no"
    IPV6INIT="yes"
    IPV6_AUTOCONF="yes"
    IPV6_DEFROUTE="yes"
    IPV6_FAILURE_FATAL="no"
    NAME="eno16777736"
    UUID="ecd3fa55-276e-4933-a1a9-c2e63208d28f"
    DEVICE="eno16777736"
    ONBOOT="yes"
    IPADDR="192.168.100.250"
    PREFIX="24"
    GATEWAY="192.168.100.2"
    DNS1="192.168.100.2"
    IPV6_PEERDNS="yes"
    IPV6_PEERROUTES="yes"
    IPV6_PRIVACY="no"
    ```
    + 安装`vim net-tools`
    
1.关闭`SELinux`

```bash
# setenforce 0  临时关闭
# vi /etc/selinux/config  修改配置文件永久关闭
...
SELINUX=disabled
...
(更改第七行)
```

2.安装`DHCP、tftp-server、xinetd、vsftp、syslinux`

```bash
# yum install -y dhcp xinetd tftp-server vsftp syslinux
```

3. 配置 DHCP

```bash
# vim /etc/dhcp/dhcpd.conf 

allow booting;
allow bootp;
ddns-update-style interim;
ignore client-updates;
subnet 192.168.100.0 netmask 255.255.255.0 {
        option subnet-mask      255.255.255.0;
        option domain-name-servers  192.168.100.250;
        range dynamic-bootp 192.168.100.100 192.168.100.200;
        default-lease-time      21600;
        max-lease-time          43200;
        next-server             192.168.100.250;
        filename                "pxelinux.0";
}

```

4.配置 tftp

```bash
# vim /etc/xinetd.d/tftp

//将disable的值修改为no

service tftp
{
···       
        disable                 = no
···
}
```

5.配置 syslinux

```bash
# cd /var/lib/tftpboot

# cp /usr/share/syslinux/pxelinux.0 ./

# cp /CentOS7/images/pxeboot/{vmlinuz,initrd.img} ./  将 images 目录下的 vmlinuz 和 initrd.img 拷贝到 /var/lib/tftpboot 目录下; CentOS7 是镜像的总目录

# cp /CentOS7/isolinux/{vesamenu.c32,boot.msg} ./  将 images 目录下的 vesamenu.c32 和 boot.msg 拷贝到 /var/lib/tftpboot 目录下; CentOS7 是镜像的总目录

# mkdir pxelinux.cfg/

# cp /CentOS7/isolinux/isolinux.cfg pxelinux.cfg/default  拷贝引导模板; CentOS7 是镜像的总目录

# vim pxelinux.cfg/default
//将第1行修改为：
default linux
//将第64行修改为：
append initrd=initrd.img inst.stage2=ftp://192.168.100.250 ks=ftp://192.168.100.250/pub/ks.cfg quiet
//将第70行修改为：
append initrd=initrd.img inst.stage2=ftp://192.168.100.250 rd.live.check ks=ftp://192.168.100.250/pub/ks.cfg quiet

# cp -r /CentOS7/* /var/ftp/  将所有的镜像文件拷贝到 /var/ftp/ 目录下; CentOS7 是镜像的总目录

```

6.配置 Kickstart 并编辑 Kickstart 应答文件

(Kickstart 应答文件可以从新安装的 CentOS 的 root
 目录下找到; 也可以用 system-config-kickstart 来生成) 

```bash
# cp ~/anaconda-ks.cfg /var/ftp/pub/ks.cfg
# chmod +r /var/ftp/pub/ks.cfg

# vim /var/ftp/pub/ks.cfg 
//将第6行的cdrom修改为：
 url --url=ftp://192.168.100.250
//将第21行的时区修改为：
 timezone Asia/Shanghai --isUtc
//将第28行修改为：
 clearpart --all --initlabel
```

7.重启所有服务并且设置开机自启

```bash
# systemctl start dhcpd
# systemctl enable dhcpd
# systemctl start xinetd
# systemctl enable xinetd
# systemctl start vsftpd
# systemctl enable vsftpd

//查看启动情况
# systemctl status dhcpd
# systemctl status xinetd
# systemctl status vsftpd

```

8.system-config-kickstart 生成 Kickstart 应答文件

```bash
# yum install system-config-kickstart
```