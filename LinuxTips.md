## 设置root账户的密码
```
sudo passwd root
```
## vi编辑模式下Backspace无法退格删除[Ref](https://blog.csdn.net/u011304490/article/details/81367490)
```
set nocp
```

## vi编辑模式下上下左右出现字母
```
sudo vi  /etc/vim/vimrc.tiny
```
修改　　set compatible  为  set nocompatible      设置是否兼容
添加　　set backspace=2　　　　　　　　　     设置 backspace可以删除任意字符

## root切换
* su #进入root
* su fred  #su 用户名
## 无法连接虚拟设备 sata0:1，因为主机上没有相应的设备
破解版的 VMware Fusion, 关机->设置->硬盘->高级选项->总线类型:SATA
## 配置scp传送文件

## 打开当前路径
```
nautilus ./
```

## 安装yum
```
sudo apt-get install build-essential
sudo apt-get install yum
```

## Ubuntu内建命令[Ref](https://blog.csdn.net/chenyoper/article/details/78260007)
```
apt-get XXX
```
删除
```
dd #删除整行
d1G #删除当前行到顶
dG #删除当前行到底
```
## Bash
* 文件扩展名为.bash
* #!/bin/bash, 在宣告这个scripts内的语法是bash
* #注解
* echo = printf, -e = 开启转义

## 扩容硬盘
### 命令
* ```df```  #查看文件系统的磁盘占用情况
* ```du -h``` /xxx/xxx #查询路径下目录及子目录占用情况,
  - h补充了单位k/M,
  - s只显示当前目录占用磁盘情况
* ```fdisk -l``` #查询分区情况
* ```fdisk /dev/sda``` #对sda操作
  - m #帮助
  - n #添加新分区
    - p #主分区, 编号1到4
    - l #逻辑分区, 编号从5开始
  -

### Linux分区简介
#### 硬盘标准
  - ATA -> IDE, 并口设备
  - SCSI
  - SATA 是ATA的升级, 串口设备，目的是替换 IDE

#### 硬盘种类
* IDE
* SATA
* SCSI

  - IDE/SATA # 硬盘与主板物理连接方式分为这两种，前者通风不良，后者是主流


#### 系统目录结构
![Linux 树状结构](https://raw.githubusercontent.com/fredsun/RES/master/LinuxTreeStructure.jpg)

* /dev #device缩写
* /dev/hda #hda 是 Hard Disk A 的缩写
* /dev/sda #SCSI hard Disk A
* /dev/sda1 #第一块SCSI硬盘的主分区（1-4 为主分区）

#### 系统分区结构
分区与目录挂钩称为挂载
##### 分区分类
* 主分区，1-4，硬盘的容量=主分区+扩展分区
* 扩展分区，0-1, 且主+扩展最多4个，扩展分区=各逻辑分区之和，是一个概念
  - 逻辑分区，如swap

##### 分区表
* MBR, Master Boot Reord主引导记录, 在磁盘的第一个扇区，分为三部分 **用fdisk分区**
  - bootloader
  - DPT, 分区表，64个字节， 一个分区表16个字节，所以只能存放四个分区表
  - 结束字， 55AA
* GPT，GUID Partition Table Scheme, 支持128个分区，支持18EB, **用gdisk分区**

### Ubuntu16.04重启报错
自身问题是之前扩充了一次整体硬盘后未继续分区合并, 解决方案[Ref: Vixual](http://www.vixual.net/blog/archives/213)
```
sd 2:0:0:0 [sda] Assuming drive cache: write through
/dev/sda1 contains a file system with errors, check forced.
Inodes that were part of a corrupted orphan linked list found.

/dev/sda1: UNEXPECTED INCONSISTENCY; RUN fsck MANUALLY.
  (i.e., without -a or -p options)
fsck exited with status code 4
The root filesystem on /dev/sda1 requires a manual fsck

BusyBox v1.22.1 (Ubuntu 1:1.22.0-15ubuntu1) built-in shell (ash)
Enter 'help' for a list of build-in commands.
(initramfs)
```

#### 解决:
翻译后发现，需要手动调用fsck进行自动修复
```
$ fsck -y /dev/sda1
返回一长串后
$ reboot
```

### 1. 软件扩充Linux虚拟机硬盘
### 2. Linux下将扩充的空间用于分区
```
sudo fdisk -l
sudo fdisk /dev/sda
n
p
3
自己输入 #最后的分区开始作为first sector!
回车 #默认到最后的分区即可
w
sudo fdisk -l
```
Command (m for help): w
The partition table has been altered.
Calling ioctl() to re-read partition table.
Re-reading the partition table failed.: Device or resource busy

The kernel still uses the old table. The new table will be used at the next reboot or after you run partprobe(8) or kpartx(8).


### linux无法扩充硬盘
```
fred@ubuntu:~$ sudo fdisk -l
[sudo] password for fred:
Disk /dev/sda: 500 GiB, 536870912000 bytes, 1048576000 sectors
Units: sectors of 1 * 512 = 512 bytes
Sector size (logical/physical): 512 bytes / 512 bytes
I/O size (minimum/optimal): 512 bytes / 512 bytes
Disklabel type: dos
Disk identifier: 0xe8217d63

Device     Boot    Start      End  Sectors  Size Id Type
/dev/sda1  *        2048 39942143 39940096   19G 83 Linux
/dev/sda2       39944190 41940991  1996802  975M  5 Extended
/dev/sda5       39944192 41940991  1996800  975M 82 Linux swap / Solaris
```
如上的系统按照下面的命令执行后选择 ```Last sector, +sectors or +size{K,M,G,T,P} (39942144-39944189, default 39944189):``` 输入```50G```报错 **```Value out of range.```**, 原因是FirstSector选择的是sda1与sda2之间的扇区, 连续区域只有这么大
```
Command (m for help): n
Partition type
   p   primary (1 primary, 1 extended, 2 free)
   l   logical (numbered from 5)
Select (default p): p
Partition number (3,4, default 3): 3
First sector (39942144-1048575999, default 39942144):
Last sector, +sectors or +size{K,M,G,T,P} (39942144-39944189, default 39944189): 50G
Value out of range.
```
#### 解决：
1. 如果空隙的是swap分区[Ref:LinuxQuestion](https://www.linuxquestions.org/questions/linux-desktop-74/value-out-of-range-error-on-a-disk-with-40-gb-free-space-4175479882/)
```
swapoff -av  # 关闭swap分区的 所有交换设备，和显示版本信息
mkswap /dev/sda5 # 设置swap分区
swapon -av # 打开swap分区的
```
2. 如果空隙的是extended分区，先跳过
