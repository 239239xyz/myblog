---
title:  ArmBian做ZeroTier内网穿透
tags: [ArmBian, ddns, ZeroTier]
categories: [ArmBian, DDNS]
date: 2020-10-08
---

Linux 最简单的安装方法：

```
curl -s https://install.zerotier.com/ | sudo bash
```
<!-- more -->
无任何报错显示：

```
*** Waiting for identity generation...
```

\*\*\* Success! You are ZeroTier address [ d56f99571f ].
把 zerotier 服务复制到系统服务目录并激活

```
systemctl daemon-reload
systemctl start zerotier-one
systemctl enable zerotier-one //开机启动
```

察看状态：

```
zerotier-cli info
```

演示：

[root[@instance-1 ](/instance-1) ~]# `zerotier-cli info`
200 info d56f99571f 1.2.12 ONLINE
最后加入 zerotier 网络：

```
zerotier-cli join <network> //你的Zeroiter网络ID
```

**彻底卸载 Zerotier-one**
通过 dpkg 删除 zerotier-one 服务

```
sudo dpkg -P zerotier-one
```

删除 zerotier-one 文件夹，该文件夹存储了 address 地址，删除后再次安装会获得新的 address 地址

```
sudo rm -rf /var/lib/zerotier-one/
```

服务\*\*

```
sudo dpkg -P zerotier-one
```

删除 zerotier-one 文件夹，该文件夹存储了 address 地址，删除后再次安装会获得新的 address 地址

```
sudo rm -rf /var/lib/zerotier-one/
```
