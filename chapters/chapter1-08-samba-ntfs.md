# Samba/NFS 文件共享

## 

## CentOS 7 安装 NFS网络文件系统

1.安装

```shell
# yum install nfs-utils
```

2.配置

```shell
# mkdir /nfsfile
# chmod -Rf 777 /nfsfile
# vim /etc/exports
# systemctl restart rpcbind
# systemctl enable rpcbind
# systemctl start nfs-server
# systemctl enable nfs-server
```

3.Linux客户端配置

```shell
$ sudo apt-get install nfs-common
$ showmount -e 192.168.42.153
$ mkdir /nfs
$ sudo mount -t nfs 192.168.42.153:/nfsfile /nfs    挂载到本地目录
$ sudo umount /nfs                                  
```

4.windows客户端

控制面板 -> 程序 -> 启用或关闭Windows功能 ->  NFS 服务 -> NFS 客户端

```cmd
c:\>mount 192.168.42.153:/nfsfile F:    映射到F盘
c:\>umount F:                           取消映射
```
