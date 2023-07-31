title: Rclone挂载Google Drive网盘
author: daoke
author_id: defaultAuthorId
language: zh-CN
tags:
  - rclone
categories:
  - vps
date: 2022-11-22 22:22:00
---

挂载GD  到这里结束配置 rclone，下面要把 Google Drive 网盘挂载到云主机/Vps 上和设置开机自启，自动挂载 Google Drive 网盘  首先安装fuse:

`apt-get install fuse`  #debian

`yum install fuse`   #centos

新建一个你要挂载的目录，例如我要挂载到

`/home/gdrive`

`mkdir -p /home/gdrive`

再执行挂载命令：

`rclone mount gd: /home/gdrive --allow-other --allow-non-empty --vfs-cache-mode writes`

gd 为Rclone的配置名称，比如你在创建配置 rclone 的时候 Name 填的 gd，/home/gdrive 为本地路径（注意空格别漏了）； 这里还可以自定义设置网盘里的文件夹路径，例如：

`rclone mount gd:backup /home/gdrive --allow-other --allow-non-empty --vfs-cache-mode writes`

卸载 Google Drive 磁盘

`fusermount -qzu /home/gdrive`

挂载只要几秒钟，但终端不会返回成功信息，关闭 SSH 重连即可。  重连后查看是否挂载成功： `df-h`
