---
title: Linux命令学习1：cut命令
date: 2017-11-16 19:59:17
tags: Linux
toc: true
---

#### cut命令​介绍 

cut命令是一个选取命令，是处理一行数据的命令。

​	格式：

​	`cut [-c] file`  

​	`cut [-df] file` 

​	`cut [b] file`

<!-- more -->

​	cut命令是从文件中的**每一行** 进行字节，字符和字段的处理，并将结果输出到标准输出。如果cut命令后面跟file，则会从标准输入中读取数据。

#### 参数说明

- -b

  以字节为单位进行截取

- -c

  以字符为单位进行截取

- -d

  自定义分隔符，默认为制表符

- -f

  与-d一起使用，表示截取哪部分区域

- -n

  取消分隔多字节字符，与-b一起使用

#### cut命令的3种方式

- 以字符为单位进行处理

  ```shell
  root@ubuntu:~# who
  zzy      tty7         2017-09-28 11:20 (:0)

  #截取第一个字符
  root@ubuntu:~# who | cut -c 1
  z
  #截取第一个到第十个之间所有的字符
  root@ubuntu:~# who | cut -c 1-10
  zzy      t
  #截取第一个和第十个字符
  root@ubuntu:~# who | cut -c 1,10
  zt
  ```

  > 注意：-c选项是截取第几个字符，其中“-”表示一个区间，“。”表示的是单个字符。

- 以字节为单位进行处理

  ```shell
  root@ubuntu:~# who
  zzy      tty7         2017-09-28 11:20 (:0)

  root@ubuntu:~# who | cut -b 10-
  tty7         2017-09-28 11:20 (:0)
  ```

- 以自定义字符为单位进行处理

  - 以:为分隔符

    ```shell
    root@ubuntu:~# echo $PATH
    /usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games

    #以冒号为分隔符，截取第五个位置数据
    root@ubuntu:~# echo $PATH | cut -d: -f5
    /sbin
    ```

#### 总结

​	cut命令在处理数据上是很方便的，特别是在截取数据上。唯一不好的地方就是对于**空格作为分隔符的情况** 。