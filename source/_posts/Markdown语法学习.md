---
title: Markdown语法学习
date: 2017-09-17 20:56:26
tags: [MarkDown,语法]
toc: false
---

### 一、前言

​	本文只是MarkDown语法的简单学习，最重要的还是要熟练，多运用。

​	Markdown在写作，做笔记方面比较好。

<!-- more -->

### 二、语法的简要规则

#### 1.标题

标题总共有六个等级，使用的符号为#，在输入#后加一个空格。

代码为：

```
# 第一级标题
## 第二级标题
### 第三级标题
#### 第四级标题
##### 第五级标题
###### 第六级标题
```

效果为：

# 第一级标题
## 第二级标题
### 第三级标题
#### 第四级标题
##### 第五级标题
###### 第六级标题

------

#### 2.斜体和粗体

- 粗体：使用一对的两个星号包含。

  代码：

  `**我是粗体**`

  效果为：

  ** 我是粗体 **

- 斜体：使用一对的一个星号包含。

  代码：

  `*我是斜体*`

  效果为：

  *我是斜体*

#### 3.列表

- 无序列表只需要在文字前面加上一个减号和一个空格就可以了，有序列表直接按数字排列（一个数字+一个点+一个空格）

代码：

```
- 列表1
- 列表2
- 列表3

1. 有序1
2. 有序2
3. 有序3
```

效果为：

- 列表1
- 列表2
- 列表3

1. 有序1
2. 有序2
3. 有序3

- 无序二级列表为：四个空格 + 一个减号 + 一个空格，有序二级列表为：四个空格 + 一个数字 + 一个点 + 一个空格

代码为：

```
- 列表1
    - 列表1.1
    - 列表1.2
- 列表2

1. 有序1
    1.1 有序1.1
    1.2 有序1.2
2. 有序2
```

效果为：

- 列表1
  - 列表1.1
  - 列表1.2
- 列表2

-------

1. 有序1   

   1.1 有序1.1

   1.2 有序1.2

2. 有序2



#### 4.图片和链接

- 链接：格式为`[]()`，[]为要显示的文本，()里面为具体的网址链接

代码为：

`[百度](http://www.baidu.com/)`

效果为：

[百度](http://www.baidu.com/)

- 图片：格式为`![]()`，()内为要显示的图片的链接地址

代码为：

`![](http://upload-images.jianshu.io/upload_images/259-0ad0d0bfc1c608b6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)`

效果为：

![](http://upload-images.jianshu.io/upload_images/259-0ad0d0bfc1c608b6.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

- 获取网页上图片的方式：找到图片，右键复制图片地址

#### 5.引用

引用的格式很简单，在要引用的文字前面加上>就可以了。

代码为：

`> 这是一段引用`

效果为：

> 这是一段引用

#### 6.代码引用

- 如果只是引用一行，只需要用`(一般在键盘上的ESC键下面那一个)将语句包括起来。
- 如果引用多行，则需要在这段文字前后用三个`包裹起来，分别放在首行和末行。（注意是三个)

#### 7.删除线

删除线的格式是前后有两个波浪线~~

代码：

` ~~我是删除线~~ `

效果为：

~~我是删除线~~

#### 8.分割线

分割线可以用三个或更多的减号-，或者多个*号，必须要单独的一行，可以含有空格。

代码：

```
你好
-------
我是分割线
*******
哎呀，被分割了
```

效果为：

你好

-------

我是分割线

*******

哎呀，被分割了

#### 9.目录

Markdown的目录可以使用`[TOC]`，但是每个编辑器都有不同的方式，不一定支持。

sublime text中需要使用插件MarkDownTOC,安装后在**Preferences -> Package Settings -> MarkdownTOC -> Setting - >User**进行配置：

{

"default_autolink": true, 目录以链接形式呈现


"default_bracket": "round", 链接以圆括号包裹


"default_depth": 0 无限目录深度

}

sublime text中插入目录的方法是：**tools -> MarkDownTOC -> insert TOC**





​    



































































































