# IPtables 与 FireWall 防火墙

### FireWall 使用

[参考](https://fedoraproject.org/wiki/FirewallD/zh-cn)

```shell
# systemctl start firewalld       //开启
# systemctl enable firewalld      //启用
# systemctl stop firewalld        //关闭
# systemctl disable firewalld     //禁用
# firewall-cmd --permanent --add-service=ssh                                   //允许 SSH
# firewall-cmd --permanent --add-port=22/tcp                 //允许 22 号端口
# firewall-cmd --permanent --add-port=80/tcp --add-port=80/udp //开放80 tcp udp 端口
# firewall-cmd --permanent --add-port=60000-61000/udp     //开放60000到61000的端口范围
# firewall-cmd --query-port=80/tcp                    //检查是否生效
# firewall-cmd --permanent --add-masquerade           //启动地址转换
# firewall-cmd --permanent --remove-port=80/tcp                   //删除端口
# firewall-cmd --permanent --remove-service=ftp                  //删除 ftp 服务
# firewall-cmd --reload                         //重新加载配置
# firewall-cmd --complete-reload
# firewall-cmd --list-ports       //显示当前开放的端口
# firewall-cmd --list-service     //显示当前开放的服务
# firewall-cmd --list-all 
```

### 切换至 iptables

```shell
# systemctl stop firewalld       //关闭
# systemctl disable firewalld    //禁用
# yum install -y iptables-services
# systemctl start iptables       //开启
# systemctl enable iptables      //启用
```

### 使用 iptables

[参考](https://wiki.debian.org/iptables)

```shell
# iptables -L -n                                                    //查看本机iptables设置情况
# iptables -F                                                       //清除所有规则
# iptables -t nat -F                                                //清除 nat 表中的所有规则
# iptables -t filter -p INPUT ACCEPT
# iptables -A INPUT -p tcp -s 192.168.3.0/24 --dport 22 -j ACCEPT   //允许来自192.168.3.0/24的SSH连接
# iptables -A INPUT -p tcp --dport 22 -j DROP                       //其它任何网段不能访问sshd服务
# iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to 1.2.3.4        //SNAT改变源地址为1.2.3.4
# iptables -t nat -A PREROUTING -p tcp -i eth2 -d 1.2.3.4 --dport 80 -j DNAT --to 192.168.3.88:80   //DNAT
# iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE               //在 eth0 上开启地址伪装
```

+ 初始化操作，通过配置文件存储防火墙规则

```sh
# iptables -P OUTPUT ACCEPT
# service iptables save
# vim /etc/sysconfig/iptables
```

+ 恢复 iptbales 表

```
*filter

# Allows all loopback (lo0) traffic and drop all traffic to 127/8 that doesn't use lo0
-A INPUT -i lo -j ACCEPT
-A INPUT ! -i lo -d 127.0.0.0/8 -j REJECT

# Accepts all established inbound connections
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# Allows all outbound traffic
# You could modify this to only allow certain traffic
-A OUTPUT -j ACCEPT

# Allows HTTP and HTTPS connections from anywhere (the normal ports for websites)
-A INPUT -p tcp --dport 80 -j ACCEPT
-A INPUT -p tcp --dport 443 -j ACCEPT

# Allows SSH connections 
# The --dport number is the same as in /etc/ssh/sshd_config
-A INPUT -p tcp -m state --state NEW --dport 22 -j ACCEPT

# Now you should read up on iptables rules and consider whether ssh access 
# for everyone is really desired. Most likely you will only allow access from certain IPs.

# Allow ping
#  note that blocking other types of icmp packets is considered a bad idea by some
#  remove -m icmp --icmp-type 8 from this line to allow all kinds of icmp:
#  https://security.stackexchange.com/questions/22711
-A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT

# log iptables denied calls (access via 'dmesg' command)
-A INPUT -m limit --limit 5/min -j LOG --log-prefix "iptables denied: " --log-level 7

# Reject all other inbound - default deny unless explicitly allowed policy:
-A INPUT -j REJECT
-A FORWARD -j REJECT

COMMIT
```

恢复上述规则

```
iptables-restore < /etc/iptables.test.rules
```

+ 保存规则

```
iptables-save > /etc/iptables/rules.v4
ip6tables-save > /etc/iptables/rules.v6
```

持久化

```
/etc/network/if-pre-up.d/iptables

vim /etc/network/if-pre-up.d/iptables
#!/bin/sh
/sbin/iptables-restore < /etc/iptables/rules.v4
/sbin/iptables-restore < /etc/iptables/rules.v6
```