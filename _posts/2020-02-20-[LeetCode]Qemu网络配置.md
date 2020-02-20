---
layout:     post                    ## 使用的布局（不需要改） 
title:      "[LeetCode]Qemu安装以及网络配置相关部分" ## 标题  
subtitle:   "网络配置，虚拟机，桥接"  #副标题 
date:       2020-02-18 21:57:00              ## 时间 
author:     "JinFei"                    ## 作者 
header-img: "img/post-bg-desk.jpg"    #这篇文章标题背景图片 
catalog: true                       ## 是否归档 
tags:                               #标签     
    - iTools
---

## qemu安装以及网络配置相关部分
### qemu相关介绍

qemu是什么，简单来说它是一个虚拟机的管理器，类似Virtualbox之类的。为了使虚机达到接近主机的性能，一般会结合kvm（或者Xen）对硬件虚拟化。kvm负责cpu+memory虚拟化，但kvm不能模拟其他设备；qemu负责模拟IO设备（网卡，usb等），两者结合能实现真正意义上的全系统仿真。
 

### QEMU模拟器基本原理
作为系统模拟器时，会模拟出一台能够独立运行操作系统的虚拟机。如下图所示，每个虚拟机对应主机(Host)中的一个QEMU进程，而虚拟机的vCPU对应QEMU进程的一个线程。
![Qemu与硬件的关系](https://static.oschina.net/uploads/img/201702/21145729_5FGx.png "Qemu模拟器与宿主host之间的关系")

系统虚拟化最主要是虚拟出CPU、内存及I/O设备。虚拟出的CPU称之为vCPU，QEMU为了提升效率，借用KVM、XEN等虚拟化技术，直接利用硬件对虚拟化的支持，在主机上安全地运行虚拟机代码(需要硬件支持)。

QEMU发起ioctrl来调用KVM接口，KVM则利用硬件扩展直接将虚拟机代码运行于主机之上，一旦vCPU需要操作设备寄存器，vCPU将会停止并退回到QEMU，QEMU去模拟出操作结果。

虚拟机内存会被映射到QEMU的进程地址空间，在启动时分配。在虚拟机看来，QEMU所分配的主机上的虚拟地址空间为虚拟机的物理地址空间。

QEMU在主机用户态模拟虚拟机的硬件设备，vCPU对硬件的操作结果会在用户态进行模拟，如虚拟机需要将数据写入硬盘，实际结果是将数据写入到了主机中的一个镜像文件中。

具体可参考
> https://my.oschina.net/kelvinxupt/blog/265108

### 安装环境说明

- Qemu命令行安装

`
sudo yum install qemu 
`


- Qemu源码编译安装
```
wget http://download.qemu-project.org/qemu-2.70.tar.xz
tar xvJf qemu-2.7.0.tar.xz
cd qemu-2.70
./configure --enable-kvm --enable-debug --enable-vnc --enable-werror  --target-list="x86_64-softmmu" ## 需要添加kvm硬件支持
make
sudo make install

configure脚本用于生成Makefile，其选项可以用./configure --help查看。这里使用到的选项含义如下：
--enable-kvm：编译KVM模块，使QEMU可以利用KVM来访问硬件提供的虚拟化服务。
--enable-vnc：启用VNC。
--enalbe-werror：编译时，将所有的警告当作错误处理。
--target-list：选择目标机器的架构。默认是将所有的架构都编译，但为了更快的完成编译，指定需要的架构即可。
```

### Qemu的启动
#### 下载迷你版的镜像即可
`
wget https://mirror.umd.edu/centos/7/isos/x86_64/CentOS-7-x86_64-Minimal-1810.iso
`
#### 利用安装好的将镜像挂载成只读的
`
mount /home/ljf/qemu/CentOS-7-x86_64-Minimal-1810.iso /mnt/ -o loop
`
#### 使用qemu-img创建启动镜像(这里以及一下步骤使用师兄已经安装好的qemu命令来操作)
`
/home/zjc/bin/qemu-3.0-pmv/bin/qemu-img create centos.img 10G
centos.img是镜像文件的名字，10G是镜像文件大小。镜像文件创建完成后，可使用qemu-system-x86来启动x86架构的虚拟机。
`
#### 装载
```
/home/zjc/bin/qemu-3.0-pmv/bin/qemu-system-x86_64 -m 2048 -enable-kvm -smp 2 ./centos.img -cdrom ./CentOS-7-x86_64-Minimal-1810.iso --nographic -bios /home/zjc/bin/qemu-3.0-pmv/share/qemu/bios.bin -append console=ttyS0 -kernel /mnt/isolinux/vmlinuz -initrd /mnt/isolinux/initrd.img
## 需要指定kernel、initrd、append参数 
由于是使用 --nographic 以非图形界面的方式启动，所以需要重定向guest的console，所以需要“-append console=ttyS0”参数，而使用该参数是必须要使用-kernel参数的，因为无法直接将append中的内核命令行参数传递到硬盘、CDROM等里面的kernel中去。
```
详细参数可参考：
> http://smilejay.com/2013/12/qemu-kvm-install-guest-in-text-mode/

#### 使用非图形化界面 --nographic 启动镜像
`
/home/zjc/bin/qemu-3.0-pmv/bin/qemu-system-x86_64 centos.img -m 2048 -enable-kvm --nographic
`
#### 启动
加载出一个只可以输入指定字符的图形化界面，配置相关信息，比如时区，管理员用户，密码等等，配置完之后就可以正常启动了。

#### 网络配置
这个时候达到的效果应该是虚拟机能够正确启动，但是网络不通，效果是宿主ping不通虚拟机，虚拟机之间也不能互相ping通，主机ping不通虚拟机。下面需要进行网络配置这一块。

##### 通过在主机上搭建虚拟网桥来实现虚拟机ping通主机虚拟机之间互相ping通

参考资料：
> https://blog.csdn.net/hzhsan/article/details/44098537

![网桥设备示意图](https://img-blog.csdn.net/20150327174701204 "通过桥接模式 网络配置模拟器与宿主host之间的关系")
1. 搭建虚拟网桥
```
cd /etc/sysconfig/network-scripts/ ## centos下网络配置文件夹
ls                                 ## 会列出网络配置的相关信息 在当前文件夹内新建一个虚拟网桥 br0
vi ifcfg-br0                ## 编辑虚拟网桥的信息
    DEVICE="br0"            ## 网桥名字
    ONBOOT="yes"            ## 开启
    TYPE="Bridge"           ## 设置桥接模式
    BOOTPROTO=none          ## 这一点不知道为啥。。
    IPADDR=192.168.10.1     ## 网桥的ip地址
    NETMASK=255.255.255.0   ## 掩码
    NM_CONTROLLED=no
    DELAY=0
```
2. 编辑修改网络设备脚本文件，修改网卡设备em3（此时的机器）
```
vi ifcfg-em3                ## 编辑主机设备的网卡信息
    DEVICE="em3"            ## 网桥名字
    ONBOOT="yes"            ## 开启
    TYPE="Ethernet"         ## 以太网
    BOOTPROTO=none          ## 这一点不知道为啥。。
    HWADDR=AA:BB:CC:DD:EE:FF ## ?
    NM_CONTROLLED=no
    BRIDGE=br0              ## 与上步虚拟网桥进行绑定
```
3. 重启网络服务

`
sudo service network restart
`

4. 给虚拟机指定的静态ip地址
参考：
> https://lintut.com/how-to-setup-network-after-rhelcentos-7-minimal-installation/
```
nmtui ## 该命令会进入配置指定的ip界面，将配置的ip，子网掩码，与上述新建的网桥保持到一个网段上即可。
sudo service network restart ## 配置好ip后，重启网络服务，可能需要等一段时间才生效，我每次是启动机器后，隔了大概几十s后，才能正确分配ip
```

5. 查看虚拟机此时的ip是否生效

`
ip addr show
`
6. 这时候可以达到的效果是虚拟机能够互相ping通，宿舍与虚拟机能够ping通，但是虚拟机仍不能访问外网
注意：如果配置了多台虚拟机，需要在qemu启动命令上有所变化，应该手动指定 mac 地址防止多个 guest 的 mac地址 重复导致 guest 之间不能互相 ping 通，比如：

`
qemu-system-x86_64 -m 1000 -enable-kvm centos.img -net nic,macaddr=52:54:00:12:34:57 -net bridge,br=br0 -vnc :1
`

7. 在上述虚拟机与主机，虚拟机与虚拟机之间能够互相ping通的的基础上，实现虚拟机通过无线接口的网络连接互联网，相当于 host 做了一个 NAT。
- 参考 > http://blog.jcix.top/2016-12-30/qemu_bridge/
- 开启路由转发：

`
sysctl -w net.ipv4.ip_forward=1 ## 执行命令，开启ipv4 路由转发，并重启host
` 
- 利用 iptables 搭建 MASQUERADE 模式的 NAT，执行下面两条命令的一条即可，本实验中效果相同：
```
iptables -t nat -A POSTROUTING -s "192.168.10.0/255.255.255.0" ! -d "192.168.10.0/255.255.255.0" -j MASQUERADE 或者
iptables -t nat -A POSTROUTING -s "192.168.10.0/255.255.255.0" -o em1 -j MASQUERADE 
```

8. 完成网络配置部分,查看是否可以ping通
`
ping baidu.com
`



