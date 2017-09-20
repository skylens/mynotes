

## 安装

**1.CentOS 7** 

安装[RDO Quick Start](https://www.rdoproject.org/install/quickstart/)

[ubuntu初始化脚本](http://www.chenshake.com/openstack-mirror-and-password/)

```bash
$ 
```

**2.ubuntu**


## OpenStack 操作

**1.OpenStack 与 VMware 虚拟机迁移**

将 OpenStack 的 KVM 虚拟机迁移到 VMware ESXI 上，将 VMware ESXI 上的虚拟机迁移到 OpenStack
上，实际上是使用 `qemu-img` 命令进行虚拟机镜像格式的转换，镜像格式 `vmdk` 与 `qcow2` 的互相转换。

* 将 VMware ESXI 平台的虚拟机迁移到 OpenStack 平台

   + Linux 内核的虚拟机
     
     Linux 虚拟机对驱动的兼容性较好，进行虚拟机格式转换后就可以直接部署到 OpenStack 上了
     ```bash
     在 VMware ESXI 上将虚拟机导出为 OVF 模板，并将 OVF 模板中的 Kernel_linux.vmdk 的文件上
     传到 OpenStack 的任意节点上进行格式转换，(当然也可以上传到安装了 KVM 虚拟化工具的 Linux 上
     进行格式转换)
     $ qemu-img convert -f vmdk Kernel_linux_vmware.vmdk -O qcow2 \
       Kernel_linux_kvm.qcow2  //利用 `qemu-img` 将 vmdk 装换为 qcow2
     $ glance image-create --name Kernel_linux_vmware-kvm --disk-format qcow2 \
       --container-format ovf --is-public True --file Kernel_linux_kvm.qcow2
     ```
   + Windows 虚拟机
     
     Windows 虚拟机在 VMware ESXI 上使用 VMware Tools 驱动，而在 OpenStack 的 kvm 上使
     用 [virtio 驱动]()，在导出虚拟机 OVF 模板之前需要安装 [virtio 驱动 spice-guest-tools](https://www.spice-space.org/download/binaries/spice-guest-tools/)，安装完后，
     导出为 OVF 模板
     
     ```bash
     上传 OVF 模板中的 vmdk 文件到 OpenStack 上,也可以上传到安装了 KVM 虚拟化工具的 Linux 上
     $ qemu-img convert -f vmdk Windows_vmware.vmdk -O qcow2 \
       Windows_kvm.qcow2  //利用 `qemu-img` 将 vmdk 装换为 qcow2
     使用 virt-manager 为 Windows_kvm.qcow2 添加一块 virtio 磁盘，启动 Windows ，待 Windows
     正常运行时，重启 Windows ，关闭 Windows ，删除 virtio 磁盘，将原来的磁盘总线类型改为 virtio 
     再次启动 Windows ，这时 Windows 已经正常加载 virtio 驱动了，上传镜像至 OpenStack 上
     $ glance image-create --name windows_vmware-kvm --disk-foemat qcow2 \
     --container-format ovf --is-public True --file Wwindow_kvm.qcow2
     ```
     
* 将 OpenStack 平台的虚拟机迁移到 VMware ESXI 平台

  + Linux 内核的虚拟机
    登录虚拟机磁盘文件存在的宿主机，将磁盘格式转换为 vmdk
    ```bash
    $ cd /mnt/netapp/nova/instance/953eef02-f319-4dfd-9662-9efb4f98d352/
    $ qemu-img info disk
    $ qemu-img convert -f qcow2 disk -O vmdk \
     Kernel_linux_vmware.vmdk  //利用 qemu-img 将 qcow2 转换为 vmdk
    将 Kernel_linux_vmware.vmdk 导入到 VMware ESXI 中，注意 Linux 内核的虚拟机的
    SCSI 控制器类型为 SILogic Parallel
    ```  
 + Windwos 虚拟机
   
   ```bash
   $ cd /mnt/netapp/nova/instance/953eef02-f319-4dfd-9662-9efb4f98d352/
   $ qemu-img info disk
   $ qemu-img convert -f qcow2 disk -O vmdk \
     Windows_vmware.vmdk
   注意 Windows 虚拟机的 SCSI 控制器的类型为 LSILogic SAS ，网卡模式为 VMXNET 3 ，最
   后需要安装 VMware Tools
   ```


**2.云硬盘的创建及挂载使用**

参考链接:[https://community.runabove.com/kb/en/instances/attach-volume.html](https://community.runabove.com/kb/en/instances/attach-volume.html)

创建云硬盘 --> 管理已挂载的云硬盘 --> 选择要挂载到的目标云主机

对目标主机进行一下操作

```bash
$ lsblk 
$ sudo mkfs.ext4 /dev/vdb 
$ sudo mkdir /mnt/clouddisk
$ sudo mount /dev/vdb /mnt/cloddisk/
$ df -h
```

**3.虚拟机镜像**
