---
title: ssh与expect学习记录
date: 2017-11-17 11:55:37
tags: [shell] 
toc: true
---

#### 0. 前言

这是学习shell过程中对于ssh和expect的一些学习记录，不算详细，但是基本的都有，以后接触到有用到再慢慢添加修改。

<!-- more -->

#### 1.SSH

ssh是一种网络协议，主要用于计算机之间的加密登录。ssh只是一种协议，有多种实现，比如winscp登录远程主机也可以使用ssh登录。

- 基本用法

  - 登录远程机器

    `ssh username@ip_addr`

    ssh默认使用端口22，要修改端口可以使用-p参数

    `ssh -p 5500 username@ip_addr` 

  - 登录远程并执行命令

    执行ls -la命令：`ssh username@ip_addr "ls -la" ` 

  - 如何执行多条命令？使用分号隔开就可以了。

    `ssh username@ip_addr "pwd ;  ls -la; cd /root"` 

  - 如何执行多行命令？只要写了第一个双引号（或单引号），后面一行一行写命令在最后再补完双引号（或单引号）就可以。

    ```shell
    ssh username@ip_addr "(')
    > ls -la
    > pwd
    > "(')
    ```

  - **远程执行本地脚本或操作文件**

    `ssh username@ip_addr  < a.sh`  (执行不带参数的脚本)

    `ssh username@ip_addr 'bash -s' < a.sh a b c` (执行带参数的脚本需要加上bash -s)

    将本地文件内容复制到远程机器上的文件中

    `ssh username@ip_addr 'cat >> /etc/hosts' < a.txt` 

- 密钥认证

  ssh client可以使用ssh-keygen工具来生成密钥对，将公钥复制到ssh server端中的/home/user/.ssh/authorized_keys文件中，可以实现免密登录。

  实现方式是：ssh客户端使用工具生成的私钥~/.ssh/id_rsa访问ssh服务端，服务端将接受到的私钥与本机存储的公钥进行匹配，如果匹配成功了，就将公钥加密发回给客户端，客户端在使用私钥进行解密，从而实现加密传输。

  - 创建密钥对

    `ssh-keygen -t rsa`  rsa表示以rsa方式加密

    不设置密码的话就一路回车吧。

    如果ssh目录不存在，则会自动创建一个，里面有两个新生成的文件：

    id_rsa(私钥)，id_rsa.pub(公钥)。

  - 配置免密登录

    使用**ssh-copy-id**,将生成的公钥复制到远程机器的authorized_keys文件中。

    `ssh-copy-id username@ip_addr` 

    > 注意：在/etc/ssh/sshd-config文件中，有几项需要把注释去掉才可以。
    >
    > PubkeyAuthentication yes
    > AuthorizedKeysFile .ssh/authorized_keys
    >
    > 然后重启ssh服务！

#### 2.Expect

expect是用于处理交互的，能够实现自动化完成，特别是在一些需要登录输入密码之类的，比如SSH远程登录，ftp登录等，无须人工参与。

- 指令介绍

  - spawn:

    **用于启动一个新的进程** ,通常都是用spawn开启一个交互式进程，然后再与send，expect等命令结合使用完成交互过程。

  - send

    **向进程发送一个字符串** 。一般发送的是命令，比如shell命令ls.

    > 注意：后面要带一个\r,表示回车的意思。

  - expect

    **用于等待进程返回字符串** ，支持多分支的模式（类似switch-case模式）

    比如：expect "hello\r",表示等待进程返回hello字符串。

  - interact

    **让用户交互，进程停留在命令行模式下，等待用户操作** 

    > 注意：通常expect都是以expect eof或者interact结尾作为结束，
    >
    > 不同点是expect eof自动结束，而interact会回到命令行，让用户自行敲命令。

  - set

    **用于设置环境变量** 

    比如set timeout 30 表示等待30秒结束。

- expect操作

  - expect和send的关系

    expect和send其实是刚好相反，一个向进程发送命令，一个是等待进程返回字符串。

    比如：`expect "hello\r" send "hello zzy\r"` 

    这两句的意思是：在标准输入中等到了hello和换行的话，就向标准输出输出hello zzy。

  - expect的模式

    expect可以有单一分支模式也可以有多分支模式。

    单一分支如上所示。

    多分支模式结构一般为：

    ```shell
    expect {
      "a" {send "I am a\r";exp_continue}
      "b" {send "I am b\r";exp_continue}
      "c" {send "I am c\r"}
    }
    ```

    exp_continue表示继续执行下面匹配。

- spawn操作

  spawn是用来启动一个新的进程，启动后就是与这个新的进程进行交互，下面通过例子来演示：

  ```shell
  set timeout 100
  spwan ssh xxx@xxxx		#打开新的进程执行ssh命令
  expect {
    "yes/no" { send "yes\r"; exp_continue}#进程返回yes/no的话发送yes
    "password:" { send "xxxx\r"}#进程返回password的话发送xxxx
  }
  send "ls\r" #发送ls命令

  ....
  ```

- 使用命令参数运行expect

  expect也可以单独写成一个脚本，但是个人觉得麻烦，还不如直接在shell脚本中嵌套，这样就不用产生太多expect脚本。

  要在shell脚本或者python脚本中嵌套，可以采用expect -c参数，表示只使用命令行参数去执行命令。

  以下是演示：

  ```shell
  expect -c "
  	set timeout -1 #永不超时
  	spawn ssh root@192.168.137.110
  	expect {
        \"yes/no\" {send \"yes\r\";exp_continue}
        \"password:\" {send \"123456\r\"}
  	}
  	send \"ls -l \r\"
  	send \"exit\r\"
  	expect eof
  "
  ```

  > expect -c 后面带着字符串，字符串内容就是expect脚本的内容。
  >
  > 以下几点要注意：
  >
  > 1.分号用来隔开同一行不同命令
  >
  > 2.因为expect -c已经带了双引号，所以字符串内部的双引号需要转义
  >
  > 3.需要有exit，否则会停留在远程机器的命令行上，无法回到本机的shell
  >
  > 4.expect必须以expect eof结束或者interact结束，自动化任务以expect eof结束。

- 贴上写过的代码

  ```shell 
  expect -c "
  	expect <<-END
  	set timeout 1200
  	spawn ssh root@$USER_IP
  	expect {
  	\"yes/no\"  {send \"yes\r\"; exp_continue}
  	\"password:\" {send \"$USER_ROOT_PWD\r\";}
  }
  	sleep 1
  	send \"if id $USER_NAME;\r\"
  	send \"then echo the $USER_NAME is exists...\r\"
  	send \"else useradd -m -d /home/$USER_NAME -s /bin/bash $USER_NAME;\r\"
  	send \"echo $USER_NAME:$USER_PWD|chpasswd;\r\"
  	send \"sed -i -r '/root.*ALL=/a $USER_NAME    ALL=\(ALL\)    NOPASSWD: ALL ' /etc/sudoers;\r\"
  	send \"fi\r\"
  	send \"exit\r\"
  	expect eof
  	exit
  	END
  ">/dev/null 2>&1
  ```

  > 说明：这段代码作用是ssh远程登录，判断用户是否存在，不存在则自动创建用户，并且设置密码









