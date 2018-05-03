# IPtables 与 FireWall 防火墙

### FireWall 使用

```shell
# systemctl start firewalld       //开启
# systemctl enable firewalld      //启用
# firewall-cmd --permanent --add-service=ssh       //允许 SSH
# firewall-cmd --permanent --zone=public --add-port=22/tcp    //允许 22 号端口
# firewall-cmd --permanent --zone=public --add-port=80/tcp --add-port=80/udp   //开放80 tcp udp 端口
# firewall-cmd --reload           //重新加载配置
# firewall-cmd --zone=public --list-ports   //显示当前开放的端口
# firewall-cmd --zone=public --list-service   //显示当前开放的服务
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

```shell
# iptables -F                                                       //清除所有规则
# iptables -t nat -F                                                //清除 nat 表中的所有规则
# iptables -t filter -p INPUT ACCEPT
# iptables -A INPUT -p tcp -s 192.168.3.0/24 --dport 22 -j ACCEPT   //允许来自192.168.3.0/24的SSH连接
# iptables -A INPUT -p tcp --dport 22 -j DROP                       //其它任何网段不能访问sshd服务
# iptables -t nat -A POSTROUTING -o eth0 -j SNAT --to 1.2.3.4        //SNAT改变源地址为1.2.3.4
# iptables -t nat -A PREROUTING -p tcp -i eth2 -d 1.2.3.4 --dport 80 -j DNAT --to 192.168.3.88:80   //DNAT
# iptables -t nat -I POSTROUTING -o eth0 -j MASQUERADE               //在 eth0 上开启地址伪装
```