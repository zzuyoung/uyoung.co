---
title: "搭建Nodejs生产环境"
date: 2018-02-03 00:59:00
categories: 学习册
---

### 搭建Nodejs生产环境
> 安装模块
> 使用mp2管理Nodejs进程
> 安装Nginx
> MongDB安装+管理权限设置
> MongDB 数据迁移
> MongDB 自动备份



<!-- more -->

#### 安装模块
```shell

uyoung@VM-0-11-ubuntu:~$ sudo apt-get install vim openssl build-essential libssl-dev wget curl git

uyoung@VM-0-11-ubuntu:~$ nvm install 8.9.4

uyoung@VM-0-11-ubuntu:~$ nvm use v8.9.4
Now using node v8.9.4 (npm v5.6.0)
uyoung@VM-0-11-ubuntu:~$ nvm alias default v8.9.4
default -> v8.9.4
uyoung@VM-0-11-ubuntu:~$ node -v
v8.9.4
uyoung@VM-0-11-ubuntu:~$ npm --registry=https://registry.npm.taobao.org install -g npm


uyoung@VM-0-11-ubuntu:~$ echo fs.inotify.max_user_watches=524288 | sudo tee -a /etc/sysctl.conf && sudo sysctl -p

```

#### 使用mp2管理Nodejs进程
```shell

pm2 start app.js

pm2 list

pm2 show app

pm2 log

```


####  安装Nginx
```shell

// 服务器可能预装apache  
uyoung@VM-0-11-ubuntu:~$ sudo service apache2 stop

// 更新包列表
uyoung@VM-0-11-ubuntu:~$ sudo apt-get update
//  安装Nginx
uyoung@VM-0-11-ubuntu:~$ sudo apt-get install nginx

// 进入 /etc/nginx/conf.d/ 创建文件 z-uyonug-cn-8081.conf
// sudo nginx -t 检查文件
// 重启 Nginx
uyoung@VM-0-11-ubuntu:/etc/nginx/conf.d$ sudo nginx -s reload
uyoung@VM-0-11-ubuntu:/etc/nginx$ sudo service nginx reload

```

####  MongDB安装+管理权限设置

```shell

// 开启MongoDB
uyoung@VM-0-11-ubuntu:/etc/nginx$ sudo service mongod start
// 停止开启MongoDB
uyoung@VM-0-11-ubuntu:/etc/nginx$ sudo service mongod stop
// 重启
uyoung@VM-0-11-ubuntu:/etc/nginx$ sudo service mongod restart
// 进入mongo命令行
> use admin //输入
switched to db admin
> db.createUser({user:'用户名',pwd:'密码',roles:[{role:'userAdminAnyDatabase',db:'admin'}]}) //创建全局管理员

Successfully added user: {
	"user" : "uyoung",
	"roles" : [
		{
			"role" : "userAdminAnyDatabase",
			"db" : "admin"
		}
	]
}
// 为imooc-chat 数据库设置单独读写账户
> db.createUser({user:'imooc_chat',pwd:'imooc',roles:[{role:'readWrite',db:'imooc-chat'}]})

// 只读账户用于备份程序
db.createUser({user:'imooc_chat_wheel',pwd:'beifen',roles:[{role:'read',db:'imooc-chat'}]})

// 开启验证模式
uyoung@VM-0-11-ubuntu:~$ sudo vi /etc/mongod.conf
security:
  authorization: 'enabled'

// 进入数据库后
> use admin
> db.auth('用户名', '密码') //返回1认证成功

$ mongo 127.0.0.1:27017/imooc-chat -u imooc_chat -p imooc

```


####  MongDB 数据迁移


####  MongDB 自动备份
```shell

//  创建脚本
uyoung@VM-0-11-ubuntu:~$ mkdir task
uyoung@VM-0-11-ubuntu:~$ mv task tasks
uyoung@VM-0-11-ubuntu:~$ cd tasks/


```
--------
```shell

#!/bin/sh


backUpFolder=/home/uyoung/backup/imooc-chat
date_now=`data +%Y_%m_%d_%H%M`
backFileName=imooc-chat_$date_now



cd backUpFolder
mkdir -p $backFileName

mongodump -h 127.0.0.1:27017 -d imooc-chat -u imooc_chat_wheel -p beifen -o $backFileName


tar zcvf $backFileName.tar.gz $backFileName

rm -rf $backFileName

```


```shell

$ sudo sh ./tasks/imooc-chat.backup.sh
// 为脚本设置定时
uyoung@VM-0-11-ubuntu:~$ crontab -e 

// 测试定时命令
13 00 * * * SH /home/uyoung/tasks/imooc-chat.backup.sh

// 完善定时命令 凌晨4点执行
00 4 * * * SH /home/uyoung/tasks/imooc-chat.backup.sh

```