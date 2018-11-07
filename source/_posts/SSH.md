---
title: 创建SSH连接
date: 2018-5-25 12:09:13
tags:
- git
---

当我们从GitHub或者GitLab上clone项目或者参与项目时，我们需要证明我们的身份。
一种可能的解决方法是我们在每次访问的时候都带上账户名、密码，另外一种办法是在本地保存一个唯一key，在你的账户中也保存一份该key，在你访问时带上你的key即可。GitHub、GitLab就是采用key来验证你的身份的，并且利用RSA算法来生成这个密钥。
 <!-- more  -->

链接方法

### 1 首先你需要在github上或者gitlab上有一个自己的账户

 ### 2 打开git bash，输入命令ls -al ~/.ssh。

  检查是否显示有id_rsa.pub或者id_dsa.pub存在，如果存在请直接跳至第4步。

 ### 3 在git bash中键入ssh-keygen -t rsa -C "your_email@example.com"，注意将这里的邮箱地址替换 成你自己的邮箱地址。在显示如下的输出后，
一直按回车就可以了。然后就显示成这样：
![](1.jpg)


在这里可以看到id_rsa和 `id_rsa.pub` 文件已经生成。并且生成的路径也已显示。

 ### 4  cat id_rsa.pub 复制内容粘贴到github账户里。

  然后选择Add SSH key按钮，将刚刚复制的内容粘贴进去即可，然后点击add key。