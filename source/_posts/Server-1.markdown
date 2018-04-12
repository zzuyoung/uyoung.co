---
title: "服务器添加账户以及秘钥登录"
date: 2018-02-02 22:52:00
categories: 学习册
---

### 服务器添加账户以及秘钥登录
> 登录服务器
> 创建一个用户
> 实现秘钥自动登录
> 修改默认登录端口22


<!-- more -->

#### 登录服务器
> 使用SSH命令    用户名@IP  之后输入密码即可登录


#### 创建一个用户
> 使用 $sudo adduser 用户名
> 提升用户权限 $sudo gpasswd -a 用户名 sudo
> 继续 $sudo visudo 打开后 在 root ALL=(ALL:ALL) ALL 下面增加一行  用户名 ALL=(ALL:ALL) ALL  
> 然后按 Ctrl+X   然后Shift+y   回车保存并且退出


#### 实现秘钥自动登录
> 查看用户目录下是否存在 .ssh 文件以及是否生成过秘钥 
> id_res.pub是公钥 id_res是私钥
> 生成秘钥 $ssh-keygen -t rsa -b 4096 -C "zzyouyang@qq.comm" 一路回车生成
> $eval "$(ssh-agent -s)" 开启ssh代理
> ssh-add /c/User/我电脑用户名/.ssh/id_rsa   吧ksy添加到代理
> 回到服务器
> 生成秘钥 $ssh-keygen -t rsa -b 4096 -C "zzyouyang@qq.comm" 一路回车生成
> $eval "$(ssh-agent -s)" 开启ssh代理
> ssh-add /c/User/我电脑用户名/.ssh/id_rsa   吧ksy添加到代理
> 创建授权文件
```shell

uyoung@VM-0-11-ubuntu:~$ ls -a
.  ..  .bash_history  .bash_logout  .bashrc  .cache  .profile  .ssh  .viminfo
uyoung@VM-0-11-ubuntu:~$ sodu vim  .ssh/authorized_keys
// 进入后  按shift+:   输入wq退出
// 复制本地公钥内容到authorized_keys 这里

// 授权
uyoung@VM-0-11-ubuntu:~$ chmod 600 .ssh/authorized_keys
// 重启SSH服务
uyoung@VM-0-11-ubuntu:~$ sudo service ssh restart

```

![实现秘钥自动登录](https://imgone.uyoung.co/hexo/sshmiyao.png)



#### 修改默认登录端口22

```shell

uyoung@VM-0-11-ubuntu:~$ sudo vi /etc/ssh/sshd_config
// 修改端口为39999   吧Port 22改为39999
//  以及在最底部添加
AllowUsers 你的用户名

// 重启SSH服务使修改的端口生效
uyoung@VM-0-11-ubuntu:~$ sudo service ssh restart
// 本地使用 $ssh -p 39999 用户名@IP

```