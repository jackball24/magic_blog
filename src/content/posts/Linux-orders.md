---
title: Linux简单命令入门
published: 2023-05-08 14:36:26
tags: [linux]

---

### 前言

<div class='additional-content-after-post'>
<div style="background-color: rgba(0, 0, 0, 0);width:100% ">
最近写操作系统的实验用到了很多Linux的操作，想着虽然提前接触过Linux，但是好像并没有做过系统的归纳总结，正好借着这机会来重温一下
</div>
</div>



**仅查看命令请跳转——>[4.基础命令](http://shikongl0k1.top/loki/Linux-orders/#4-%E5%9F%BA%E7%A1%80%E5%91%BD%E4%BB%A4)**

### 1.Linux之灵魂

**Linux**作为一个开源且免费的操作系统，深受极客喜爱，它稳定且高效、安全且自由，在服务器领域运用广泛。

初次使用Linux操作系统，你会发现在这里万物皆为文件，没有Windows那复杂的系统构造，频繁的版本更新，以及修改系统文件时权限不够的问题；还能支持一些Windows和MacOS所不能的服务。Linux，yes！

目前市面上较知名的发行版有：**Ubuntu、RedHat、CentOS、Debian、Fedora、Arch Linux**等，还有一些常见的比如**kali**（基于Debian的发行版，常用于网络渗透测试、破解密码、逆向工程等，深受hacker喜爱）。

而Linux之灵魂，便是**terminal**和**command**（终端和命令），用户在终端输入命令，然后**shell**（命令解释器）翻译并执行指令，将结果返还给终端（有点像windows的cmd）。

Linux的安装十分简单，一般推荐刚入坑的萌新使用的，是一个叫Ubuntu（乌班图）的Linux发行版，第一次一般会推荐使用[VMware Workstation Pro](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html)+官网下载系统镜像[Download Ubuntu Desktop | Download | Ubuntu](https://ubuntu.com/download/desktop)，当然这可能会很慢，所以也可以去清华镜像站[清华大学开源软件镜像站 | Tsinghua Open Source Mirror](https://mirrors.tuna.tsinghua.edu.cn/)或者你邮的镜像站[南京邮电大学开源软件镜像站 | Njupt Open Source Mirror](http://mirrors.njupt.edu.cn/)（东西少但下载还挺快的）这类的国内镜像网站下载所需要的ISO镜像；当然，完整的安装步骤较多，建议找一篇较新的教程，跟着一步步走。

按照一些教程成功安装虚拟机之后便会进入一个桌面，紫红底，上面有豹猫（20.04）或者水母（22.04）之类的图案。

![22.04desktop](https://cdn.jsdelivr.net/gh/jackball24/Myblog_pic@main/2204dedsktop.jpg#pic_right)

这是带有图形化界面的Ubuntu，而他的本体只是那一个终端命令行以及所使用的Ubuntu server，也就是说：所有你能用图形化界面完成的事情，**命令行**都能完成（早期的系统都是先有命令行，再在此基础上开发适合大众使用的图形化界面）。

笔者目前使用的较多的是wsl2-kali（windows的linux子系统-kali版），所有的操作均由命令完成（VM虚拟机占空间大，wsl对我来说更轻便）。

### 2.VM虚拟机的小技巧

#### 2.1快照的使用

在主机中，如果不小心把C盘重要文件删了，电脑扑街了咋办？重装/重买/花大把时间用工具恢复。而在虚拟机中，你仿佛拥有了时间宝石，可以随时回到你创建快照的那个时间，那个样子。

这一切的实现只需要两步

1.![image-20230511204330059](https://cdn.jsdelivr.net/gh/jackball24/Myblog_pic@main/cut1.png#pic_center)

2.在未来想要回到刚才那个时间点的时候点击“恢复到快照”

#### 2.2克隆和删除

可以在左侧右击

![image-20230511204821285](https://cdn.jsdelivr.net/gh/jackball24/Myblog_pic@main/cut2.png#pic_center)

#### 2.3VMtools

没法在主机和虚拟机之间复制粘贴？我想直接把文件拽进虚拟机？安装一个vmware tools通通搞定

![image-20230511205255605](https://cdn.jsdelivr.net/gh/jackball24/Myblog_pic@main/cut3.png#pic_center)

### 3.常用的目录

- `/`   根目录，可以想象成数据结构里面的根结点
- `/root`  超级管理员的主目录
- `/home`  普通用户主目录
- `/bin`  bin 是 Binaries (二进制文件) 的缩写, 这个目录存放着最经常使用的命令。
- `/boot`  这里存放的是启动 Linux 时使用的一些核心文件，包括一些连接文件以及镜像文件。
- `/lib`  lib 是 Library(库) 的缩写这个目录里存放着系统最基本的动态连接共享库，其作用类似于 Windows 里的 DLL 文件。
- `/usr`   unix shared resources(共享资源) 的缩写，这是一个非常重要的目录，用户的很多应用程序和文件都放在这个目录下，类似于 windows 下的 program files 目录。
- `/var`  variable(变量) 的缩写，这个目录中存放着在不断扩充着的东西，我们习惯将那些经常被修改的目录放在这个目录下。包括各种日志文件。
- `/usr/bin`  系统用户使用的应用程序。

### 4.基础命令

这里不会把命令列得很全，主要还是一些最简单的操作。

#### 4.1实际场景

桌面右键后/直接找到terminal，进入终端，你可以直接使用普通用户操作，当然，你也可以使用**sudo -i**或**sudo**或**su**等命令进入root用户

三个命令有一些区别：

sudo : 暂时切换到超级用户模式以执行超级用户权限；

su ： 切换到某某用户模式，提示输入密码时该密码为切换后账户的密码，默认为root账户；

sudo -i: 为了频繁的执行某些只有超级用户才能执行的权限，而不用每次输入密码，可以使用该命令。提示输入密码时该密码为当前账户的密码。没有时间限制

##### 1.切换用户

```shell
su
```

然后输入密码（一般不显示，输完敲回车即可）

![image-20230512220257556](https://cdn.jsdelivr.net/gh/jackball24/Myblog_pic@main/cut26.png)

然后我们想干嘛呢，在windows中我们一般是先看到一堆文件，然后选择进入哪一个，那么在linux里，我们只使用命令行要怎么做？

##### 2.查看当前目录下的文件

```shell
ls
或者:
ll
```

查看当前目录的绝对路径

```
pwd
```

##### 3.进入文件夹

```shell
cd 文件名
```

去想要去的目录（可以有多级，用“/”分开）

##### 4.ls再次查看

每进入一层目录，你可能不知道里面有哪些文件

##### 5.进入上一级文件夹

如果想返回上一层，则使用

```shell
cd ..
```

（同样可以有多级，用“/”分开）

你会得到类似这样的结果：

**先回到`/`根目录，再向下级探索：**

![image-20230512220240679](https://cdn.jsdelivr.net/gh/jackball24/Myblog_pic@main/cut25.png)

**往上回溯：**

![image-20230512202440177](https://cdn.jsdelivr.net/gh/jackball24/Myblog_pic@main/cut5.png)

##### 6.新建文件夹

```shell
mkdir 文件夹名
```

新建空文件（非文件夹）

```shell
touch 文件名
```

##### 7.删除文件夹/文件（用**rmdir** 能删空目录）：

```shell
rm -rf 文件名
```

![image-20230512203434690](https://cdn.jsdelivr.net/gh/jackball24/Myblog_pic@main/cut6.png)

我创建了“学习资料”文件夹，又删除了file和new文件夹

##### 8.读文件

ls之后发现在学习资料里有个文件，

我想看一下（只读）secret.txt里到底有什么？

```shell
cat 文件名
```

![image-20230512204656750](https://cdn.jsdelivr.net/gh/jackball24/Myblog_pic@main/cut7.png)

可以看到secret.txt写的东西（





<div class='additional-content-after-post'>
<div style="background-color: rgba(0, 0, 0, 0);width:100% ">
 温馨提示：你输入过的所有命令都可以使用↑和↓键来翻阅（省去了长命令的二次输入）
</div>
</div>



#### 4.2 vi/vim操作（编辑文件）

vim为linux自带vi的升级版，两者你都可以看成是windows里的记事本，只不过更“高级”一点。

##### 1.编辑文件

```c
vim 文件名
```

我这里vim了一个hello.c文件

之后进入一个奇怪的界面，好像还动不了（这是一般模式，可以使用方向键移动光标）

![image-20230512220105587](https://cdn.jsdelivr.net/gh/jackball24/Myblog_pic@main/cut23.png)

##### 2.按下 i 进入插入模式（你可以编辑了）

![image-20230512213050224](https://cdn.jsdelivr.net/gh/jackball24/Myblog_pic@main/cut13.png)

##### 3.保存并退出

写完后，按下键盘左上角ESC退出插入模式，接着输入

```
:wq
```

然后就可以看到，hello.c有刚才的内容了

![image-20230512220045923](https://cdn.jsdelivr.net/gh/jackball24/Myblog_pic@main/cut22.png)

我要安装gcc编译它？

```
sudo apt install gcc
```

（root账户就不需要前面的sudo）

然后，就是熟悉的

```shell
gcc hello.c -o hello.out
```

```shell
./hello.out
```

##### 4.更多快捷键

一般模式下的撤销

`u键`

拷贝本行

`yy`

删除本行

`dd`

格式化全部代码

`gg=G`

gg为跳转到第一行，G为最后一行

……

#### 4.3其他命令

##### 1.查看当前ip（windows是ipconfig）

```shell
ifconfig
```

![image-20230512220022168](https://cdn.jsdelivr.net/gh/jackball24/Myblog_pic@main/cut21.png)

##### 2.用一下计网提到过的ping命令

```shell
ping [ip]
```

![image-20230512215954218](https://cdn.jsdelivr.net/gh/jackball24/Myblog_pic@main/cut20.png)

显然ping得通（因为是本地）

##### 3.关机

```shell
shutdown
```

##### 4.重启

```
reboot
```

##### 5.退出当前账户/终端

```
exit
```

##### 6.改变文件的权限

```shell
chmod -R 777 *
```

比较懒的一种用法，chmod是改变权限命令，-R修改应到目录下所有文件和子目录，777，三种用户权限都为7（读4+写2+执行1），*则是通配符，其他用法可自行查阅

##### 7.服务管理

```
1.systemctl ……
2.service ……
```

##### 8.进程

```
ps -a      //当前终端所有进程信息
ps -u
ps -x
ps -el
ps -ef
等
```

终止进程

```
kill -选项数字 进程名
```

……

### 5.换源以及安装输入法等操作

我们会发现在使用apt下载东西的时候速度很慢，还总是卡住，那是因为原来的源在国内是很慢的，就像前面下载官方镜像一样，我们需要换成国内的镜像源……未完待续



### 引用及参考

该文章部分内容参考以下网站/博客：

- https://www.runoob.com/linux

- [Linux入门到实践 – 羽墨的个人博客 (yumoyumo.top)](https://www.yumoyumo.top/295.html)

笔者才疏学浅，有不当的地方还望大佬交流指正。
