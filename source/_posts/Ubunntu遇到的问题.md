---
title: Ubunntu遇到的问题
date: 2018-03-03 22:59:04
tags: [Ubuntu]
toc: false
---

### Ubuntu问题1："Ubuntu is running in low-graphics mode"

​	有时候无怨无故的开启Ubuntu虚拟机，突然就报这样的错，一开始完全懵逼了~~

​	以前是直接放弃了那个ubuntu，或者是只使用命令行来操作，现在有了解决方法，已验证是可以的。

​	不过不同的笔记本可能问题不一定能解决。

<!-- more -->

- 步骤

  - 遇到这样子时候，直接按**ctrl+alt+f1** ，这样就可以进入到**命令行** ，进入命令行登录，输入账号密码，一般我忘了用户名，都是直接输入root，在输密码。

  - 进入命令行后，输入以下内容

    ```shell
    cd /etc/X11  
    cp xorg.conf.failsafe xorg.conf 
    ```

  - 最后reboot重启就可以了。