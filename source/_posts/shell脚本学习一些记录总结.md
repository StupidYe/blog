---
title: shell脚本学习一些记录总结
date: 2017-11-16 19:14:56
tags: [shell]
toc: true
---

[TOC]

#### 0.前言

这是最近完成shell脚本一键部署kubernetes的学习中，遇到的一些问题一些知识点的记录。

方便以后学习查看~~

#### 1.知识记录

- **./lib/lsb/init-function** 作用

  这是在Ubuntu中的一个文件，在shell中调用的作用是使定义在该文件的所有Shell函数在当前脚本中生效。

  比如log_action_msg用作打印提示信息，在shell脚本中调用。

- 对于ssh的处理问题比较多，有几点要注意：

  1. 正常情况下所有引号需要转义，前面加\\ ,但是在一些命令里不需要加。

     比如：`docker_bip=$(cat /run/flannel/subnet.env | grep "FLANNEL_SUBNET" )`这里grep后的双引号不用转义。 

  2. 在shell中**单引号是不会输出变量的值的，而是原样输出** 。如果在单引号中要输出变量的值，方法为：**对变量名加单引号** 。

  3. 使用ssh-copy-id复制ssh key到其他机器实现免密登录，会复制到其他机器的宿主目录下的`./ssh` ,名字为:authorized_keys.

     比如root用户：`/root/.ssh/authorized_keys`

     比如普通用户：`/home/xxx/.ssh/authorized_keys` 

- 对于**$?** ,shell只会判断上一句结果是否为真，而不会判断是否正确执行，所以有些判断不能用这个判断。

- 设置普通用户的root权限

  操作：在/etc/sudoers文件内，`root ALL=（ALL）	ALL`的下一行添加一行

  `xxx(用户名)	ALL=(ALL)	NOPASSWD:ALL` 

  NOPASSWD可加可不加，加上后，每次sudo不需要输入密码。

- 执行脚本可以使用./ 也可以使用source，但是source不支持sudo执行，./支持

- 注意使用sudo 操作的话，一些文件是会放在root的宿主目录/root下的，而不会在用户的宿主目录。

  比如在普通用户下执行 sudo ssh-keygen 生成的key文件会放在/root/.ssh下，而不加sudo，就会放在/home/xxxx/.ssh下。

- 添加普通用户的环境变量，export到~/.bashrc下，需要执行source ~/.bashrc,如果在脚本中执行这一句，那么在执行整个脚本时候不能用./,而要用source（具体为啥我也不知道）。否则source ~/.bashrc这一条指令不会执行。

#### 2.错误记录

- `[: too many arguments`

  原因：if语句中的[]的字符串太长了。

- `[: =: unary operator expected`

  原因：在匹配字符串是否相等时，变量没有加上双引号，因为变量的值有可能是空的。

  比如：

  ```shell
  if [ $FLANNEL_STATUS  ==  "yes" ]; then
  	echo "hello"
  fi
  ```

  如果变量的值是空，那么语句就会变成了`[ = "yes" ],这样子是将[ 与yes 作比较，使得if语句缺少了一个[

  解决方法就是变量加上双引号。

  ​