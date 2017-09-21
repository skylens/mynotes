# Virtualization

## KVM

1.查看`CPU`是否支持虚拟化

```bash
$ egrep '(vmx|svm)' /proc/cpuinfo 
$ egrep -c '(vmx|svm)' /proc/cpuinfo
```

2.安装虚拟化软件

* **CentOS 7**

```bash
$
```

* **CentOS 6**

```bash
$
```

* **ubuntu**

```bash
$ sudo apt-get install qemu-kvm qemu virt-manager libvirt-bin bridge-utils
```

3.加载`kvm`模块

```bash
$ modprobe kvm-intel //加载kvm模块
$ lsmod | grep kvm  //查看是否加载
```

4.`virt-manager`使用

```bash
$ sudo virt-manager
```

5.命令行使用

```bash
$ 
```

6.`virsh` 使用

```bash
$ virsh  //进入 virsh 命令行
$ virsh list  //列出所有的虚拟机
$ virsh vncdisplay 
```

## VMware

