# FTP 文件传输

### CentOS 安装配置

1.安装

```sh
# yum install -y vsftpd
```

2.修改配置文件

```sh
# cpu -u vim /etc/vsftpd/vsftpd.conf{,.orig}
# vim /etc/vsftpd/vsftpd.conf
```

3.配置文件说明

[参考](https://wiki.archlinux.org/index.php/Very_Secure_FTP_Daemon_(%E7%AE%80%E4%BD%93%E4%B8%AD%E6%96%87))

[参考](https://www.ifshow.com/centos-7-install-vsftpd-with-pasv-over-ssl-explicit/)


如要使用 `SSL` 加密传输，则需要使用 `openssl` 生成证书，或者是使用申请的 `https` 证书

```sh
# yum install -y openssl
# mkdir -pv /etc/vsftpd/ssl
# mkdir -pv /etc/vsftpd/ssl/private/
# openssl req -x509 -nodes -days 720 -newkey rsa:2048 -keyout /etc/vsftpd/ssl/private/vsftpd.key -out /etc/vsftpd/ssl/vsftpd.pem
```

```bash
# 本地用户登录
local_enable=YES
# 允许上传
write_enable=YES

local_umask=022

xferlog_enable=YES
xferlog_file=/var/log/xferlog

ascii_upload_enable=YES
ascii_download_enable=YES

allow_writeable_chroot=YES

## 匿名用户设置
# 启用匿名用户 
anonymous_enable=NO
# 不设置匿名用户密码
no_anon_password=YES
# 用于匿名登录的目录
anon_root=/var/ftp/
# 允许匿名用户上传
anon_upload_enable=YES
anon_mkdir_write_enable=YES


## SSL 设置
rsa_cert_file=/etc/vsftpd/ssl/vsftpd.pem
rsa_private_key_file=/etc/vsftpd/ssl/private/vsftpd.key

ssl_enable=YES
allow_anon_ssl=YES
force_local_data_ssl=YES
force_local_logins_ssl=YES

ssl_tlsv1=YES
ssl_sslv2=NO
ssl_sslv3=NO

require_ssl_reuse=NO
ssl_ciphers=HIGH

# DNS
pasv_addr_resolve=YES
pasv_address=skylens.co

# 开启监听
listen=YES
# 设置监听端口
listen_port=2121
ftp_data_port=2122
# 设置为被动模式
pasv_enable=YES
# 设置端口范围
pasv_min_port=2123
pasv_max_port=2131

pam_service_name=vsftpd
userlist_enable=YES
tcp_wrappers=YES
```

4.防火墙设置

`firewall` 设置

```sh
# firewall-cmd --permanent --add-service=ftp
# firewall-cmd --permanent --add-port=2123-2131/tcp
# firewall-cmd --reload
```

`iptable` 设置

未结合 `ip_conntract`

```sh
# iptables -A INPUT -p tcp --dport 2121  -m state --state NEW -j ACCEPT
# iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
# iptables -A INPUT -p tcp --dport 2121:2131 -m state --state NEW -j ACCEPT
```

5. `selinux` 问题

更新