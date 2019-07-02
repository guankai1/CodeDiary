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
* df  #查看文件系统的磁盘占用情况
* du -h /xxx/xxx #查询路径下目录及子目录占用情况,
  - h补充了单位k/M,
  - s只显示当前目录占用磁盘情况
* fdisk -l #查询分区情况
* fdish /dev/sda #对sda操作
  - m #帮助
  - n #添加新分区
    - p #主分区, 编号1到4
    - l #逻辑分区, 编号从5开始
  -

### Linux分区简介
* 硬盘标准
  - ATA -> IDE, 并口设备
  - SCSI
  - SATA 是ATA的升级, 串口设备，目的是替换 IDE

* 硬盘种类
  - IDE
  - SATA
  - SCSI

* IDE/SATA # 硬盘与主板物理连接方式分为这两种，前者通风不良，后者是主流
* /dev #device缩写
* /dev/hda #hda 是 Hard Disk A 的缩写
* /dev/sda #SCSI hard Disk A


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
*
