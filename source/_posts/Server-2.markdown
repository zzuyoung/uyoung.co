---
title: "服务器防火墙配置"
date: 2018-02-02 23:48:00
categories: 学习册
---

### 服务器防火墙配置
> 更新系统
> 配置iptables-restore
> 安装fail2ban



<!-- more -->

#### 更新系统
```shell

uyoung@VM-0-11-ubuntu:~$ sudo apt-get update && sudo apt-get upgrade

```

#### 配置iptables-restore

```shell

uyoung@VM-0-11-ubuntu:~$ sudo iptables -F

uyoung@VM-0-11-ubuntu:~$ sudo vi /etc/iptables.up.rules

*filter


-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

-A OUTPUT -j ACCEPT

-A INPUT -p tcp --dport 443 -j ACCEPT
-A INPUT -p tcp --dport 80 -j ACCEPT

-A INPUT -p tcp -m state --state NEW --dport 39999 -j ACCEPT

# ping
-A INPUT -p icmp -m icmp --icmp-type 8 -j ACCEPT

# log denied calls
-A INPUT -m limit --limit 5/min -j LOG --log-prefix "iptables denied:" --log-level 7

# drop incoming sensitive connections
-A INPUT -p tcp --dport 80 -i eth0 -m state --state NEW -m recent --set
-A INPUT -p tcp --dport 80 -i eth0 -m state --state NEW -m recent --update --seconds 60 --hitcount 150 -j DROP

# reject all other inbound
-A INPUT -j REJECT
-A FORWARD -j REJECT

COMMIT

"/etc/iptables.up.rules" 27L, 459C

uyoung@VM-0-11-ubuntu:~$ sudo vi /etc/iptables.up.rules
uyoung@VM-0-11-ubuntu:~$ sudo iptables-restore < /etc/iptables.up.rules
uyoung@VM-0-11-ubuntu:~$ sudo ufw status  //查看防火墙有木有启动
Status: inactive
uyoung@VM-0-11-ubuntu:~$ sudo ufw enable //激活防火墙
Command may disrupt existing ssh connections. Proceed with operation (y|n)? y
Firewall is active and enabled on system startup

uyoung@VM-0-11-ubuntu:~$ sudo vi /etc/network/if-up.d/iptables  //开启自动启动防火墙

#!/bin/sh
iptables-restore /etc/iptable.up.rules

uyoung@VM-0-11-ubuntu:~$ sudo chmod +x /etc/network/if-up.d/iptables   //给脚本权限


```



#### 安装 Fail2Ban 防御模块
```shell

uyoung@VM-0-11-ubuntu:~$sudo apt-get install fail2ban

uyoung@VM-0-11-ubuntu:~$ sudo service fail2ban status
uyoung@VM-0-11-ubuntu:~$ sudo service fail2ban stop //停止

```












