---
title: Cuda10 installation on Ubuntu 16.04
published: 2019-10-18T16:04:36+08:00
slug: issues-encountered-when-installing-cuda10-on-ubuntu16
description: ''
tags:
- CUDA
- Linux
category: 工程实践
draft: false
lang: zh_CN
---

由于项目要用到的代码对tensorflow版本要求较高，我需要将机器上的tensorflow1.0+cuda-8.0升级到tensorflow2.0+cuda-10.0。然而在重装了cuda10.0+cudnn7.5之后，运行`nvidia-smi`显示

```bash
NVIDIA-SMI has failed because it couldn't communicate with the NVIDIA driver. Make sure that the latest NVIDIA driver is installed and running.
```

我理解为是nvidia driver的版本不够高，所以直接从nvidia-384升级到nvidia-430,并重启了计算机。结果就发现重启之后电脑黑屏了。最终费了好大的周折将这个问题解决，现在将具体过程记录如下。

1. 解决黑屏问题。

    首先重启之后屏幕一直处于黑屏状态，我怀疑是nvidia显卡驱动没有正常工作，于是将显示器的输入口接到主板的集显的输出口。终于显示器上出现了低分辨率的变形的输出。为了避免忍受这种痛苦，`ctrl+alt+f1`进入tty1的shell中。既然显卡驱动出现了问题，我们考虑彻底卸载nvidia驱动和cuda以重新开始。

    ```bash
    sudo apt purge nvidia*
    sudo /usr/cuda/bin/uninstall_cuda_10.0.pl
    ```

    卸载完成之后重启，遇到了Ubuntu重复登录界面的问题。即便是输入了正确的用户名和口令，也会继续重复登录。
2. 解决重登录问题。

    在上网搜索了上多解决方案之后，大多数人都归因与nouveau和opengl的一些异常。遵照前人的方法，首先，禁用掉nouveau驱动。

    ```bash
    sudo vim etc/modprobe.d/blacklist
    #在该文件中加入下面两行代码
    blacklist nouveau
    options nouveau modeset=0
    ```

    重新安装nvidia驱动。需要前往nvidia官网下载自己系统对应的可用驱动，建议选择run file。

    ```bash
    #首先关掉x-window manager lightdm
    sudo service lightdm stop
    #安装驱动
    sudo **/NVIDIA-Linux-x86_64-VERSION.run –no-x-check –no-nouveau-check –no-opengl-files
    sudo reboot
    ```

    这里需要注意，后面的三个参数很重要，分别指定关闭x-window 服务，禁用nouveau，不安装opengl文件。重启之后，终于可以登入桌面。接着将显示器的接口插在显卡输出口上，同样也出现了桌面。但是新的问题也出现了：Ubuntu桌面坏了，任务栏消失了，快捷键失灵。
3. 解决桌面问题。

    初步确定为是桌面程序出现故障，所以选择最简单直接的办法：重装。

    ```bash
    sudo apt purge ubuntu-desktop
    sudo apt autoremove
    sudo apt install ubuntu-desktop
    #这里可以选择不同的桌面，例如gnome，unity
    sudo apt install unity
    sudo reboot
    ```

    重启之后在登录界面选择`ubuntu`桌面，即可进入熟悉的桌面。
4. 解决显卡输出帧数降低，显示卡顿的问题。

    新的问题接踵而至。由于驱动版本过高(430)，我的显卡不太支持，图形化界面掉帧严重，特别是观看网页视频的时候目测只有30-40帧。于是回滚到较低版本的nvidia驱动，这里我选择了418。除了从官方网站下载驱动之外，这里介绍另外一种方案。
    - software and updates -> additional drivers -> 选择418驱动 -> apply changes -> reboot
    终于，我们显示器画面丝滑流畅！