---
title: "mp2使用"
date: 2018-02-03 00:59:00
categories: 学习册
tags:
	- ubuntu
	- 服务器
	- 防火墙
	- Linux
---

### mp2使用
>  权限问题



<!-- more -->

#### 权限问题
> current 当前的服务所允运行的文件夹
> shared 啥子源代码
> source 日志问题什么的
```shell
// 在本地执行
$ pm2 deploy ecosystem.json production setup
--> Deploying to production environment
--> on host 服务器的IP
  ○ hook pre-setup
mkdir: cannot create directory ‘/www/website/production’: Permission denied
mkdir: cannot create directory ‘/www/website/production’: Permission denied
mkdir: cannot create directory ‘/www/website/production’: Permission denied

  setup paths failed

Deploy failed
// 是由于权限问题 在服务器执行下面
$ sudo chmod 777 website

```

```shell
$ pm2 deploy ecosystem.json production
--> Deploying to production environment
--> on host 118.89.249.88
  ○ deploying origin/master
  ○ executing pre-deploy-local
  ○ hook pre-deploy
  ○ fetching updates
  ○ full fetch
Fetching origin
Warning: Permanently added the ECDSA host key for IP address '116.211.167.14' to the list of known hosts.
  ○ resetting HEAD to origin/master
HEAD is now at 3a5d2c9 Merge branch 'master' of gitee.com:uyang/backend-website
  ○ executing post-deploy `export NODE_ENV=production && pm2 startOrRestart ecosystem.json --env production`
bash: pm2: command not found

  post-deploy hook failed

Deploy failed

// 出现上方问题是 回到服务器用户目录
$ cd ~
$ vi .bashrc
// 注释掉一下代码
case $- in
    *i*) ;;
      *) return;;
esac
// 执行下面即可
$ source .bashrc

```

#### pm2配置文件
```json

// ecosystem.json
{
    "apps":[
        {
            "name": "名称",
            "script": "启动文件",
            "env": {
                "COMMON_VARIABLE": "true"
            },
            "env_production": {
                "NODE_ENV":"production"
            }
        }
    ],
    "deploy": {
        "production": {
            "user": "uyoung",
            "host": ["IP"],
            "port": "ssh端口",
            "ref": "origin/master",
            "repo": "git@项目地址",
            "path": "/www/recruitapp/production",
            "ssh_options": "StrictHostKeyChecking=no",
            "post-deploy": "npm install && yarn run build && pm2 startOrRestart ecosystem.json --env production",
            "env": {
                "NODE_ENV": "production"
            }
        }
    }
}

```
```

