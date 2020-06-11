# **树莓派Ubuntu18.04镜像烧录及相关配置**

为了在树莓派上运行opencv，本文将从头开始介绍树莓派镜像文件的烧录，以及相关环境的配置。目前网上的资料参差不齐，在这里我将对我搜集到的相关资料进行重新的整理和归纳。

1. TOC
{:toc}

## 镜像烧录
这次重装系统的原因是这样子的：原有的Ubuntu mate系统在安装中文输入法时突然崩溃死机，强制断电重启后一直出现如下图的错误。![报错](https://raw.githubusercontent.com/acacxhm7/pictureHub/master/RPI_start_error.jpg "报错")

`[    2.551834]Error: “driver ‘sdhost-bcm2835’ already registered, aborting…`

通过多方查阅[报错信息](https://ubuntu-mate.community/t/raspberry-pi-3-model-b-plus-ubuntu-mate-installation-error-driver-sdhost-bcm2835-already-registered-aborting/19300)和询问，考虑到系统内并无重要文件，决定重装系统（前天上午因为无法连接到显示器已经重装过一次了呜呜呜）

这次烧录的是Ubuntu mate18.04镜像，镜像可以从[官网](https://ubuntu-mate.org/download/i386/bionic/thanks/?method=direct)上下载，这里我直接用同学提供的。下载链接如下图：

![链接](这里待替换！！！ "点击此链接")

首先需要格式化SD卡。这里我选择使用DiskGenius工具。具体选项如下图：

![格式化选项](https://raw.githubusercontent.com/acacxhm7/pictureHub/master/%E6%B7%98%E5%AE%9D%E5%9B%BE%E7%89%87.jpg "格式化选项")

如果格式化时发生`错误5:拒绝访问`，可以参照此处[知乎](https://www.zhihu.com/question/268567807)上的回答。
![处理方案](https://raw.githubusercontent.com/acacxhm7/pictureHub/master/diskpart.png "处理方案")


## 更换国内的apt源

Ubuntu默认的apt源位于英国，用它来安装软件包奇慢无比，因此我们有必要把它换成国内的源。在换源之前，首先安装vim。vim是一款功能强大的文本编辑器，我们安装vim后能够方便的对文件进行修改。关于vim的使用教程以及常见操作，可以戳[b站教程](https://www.bilibili.com/video/BV1Yt411X7mu)和[树莓派实验室](https://shumeipai.nxez.com/2013/12/26/linux-on-vim-editor-tutorials.html)。Ctrl+shift+T打开终端，以管理员身份输入以下命令：

`sudo apt-get install vim`

国内的apt源有很多，如阿里、清华、中科大的……此处我将选取中科大的镜像源。
先打开sources.list文件（此文件极其重要，切勿随意更改！）:

`sudo vim /etc/apt/sources.list`

将其中的`http://ports.ubuntu.com/ubuntu-ports`全部更改为`http://mirrors.ustc.edu.cn/ubuntu-ports/`

命令模式下输入`:wq`退出

执行下列命令更新：

`sudo apt update`

`sudo apt upgrade`

更新时间会比较长，耐心等待~
## 相关包的安装
### python的安装
此处将安装python3.7。（参考[此处](https://installvirtual.com/how-to-install-python-3-7-on-ubuntu-16-04-18-04/)）

首先你需要安装software-properties-common

`sudo apt-get install software-properties-common`

之后需要为python3.7添加PPA

`sudo add-apt-repository ppa:deadsnakes/ppa`

`sudo apt-get update`

接下来使用apt-get安装python3.7

`sudo apt-get install python3.7`

安装完毕后可以输入以下命令查看Python的版本。

`python3.7 -V`
### 默认启动python3和安装pip
树莓派原先默认启动的是python2，在这里我们把它改成python3：

`sudo rm -r /usr/bin/python`

`sudo ln -s /usr/bin/python3 /usr/bin/python`

pip是一个安装第三方python库的工具，安装pip的命令是：

`sudo apt-get install python3-pip`
### python IDLE的安装
python的IDE有很多，我个人比较习惯使用官方提供的python IDLE。安装python3的IDLE只需要在终端执行以下命令：

`sudo apt-get update`

`sudo apt-get install idle3`

## 中文输入法的安装

树莓派实质上是一台计算机，在使用的过程中我们经常要进行中文的输入。此处推荐使用scim中文输入法。

安装字体库`sudo apt-get install ttf-wqy-zenhei`

安装拼音输入法`sudo apt-get install scim-pinyin`，重启之后即可使用，Ctrl+空格切换中英文。

## 结语
树莓派的安装虽然简单，但若被种种小问题困扰，也是一件令人头疼的事情。完成安装之后便可以尽情探索树莓派的世界了[起飞~]
