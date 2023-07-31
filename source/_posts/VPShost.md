title: VPS主机一键更换国内软件源脚本
author: daoke
author_id: defaultAuthorId
language: zh-CN
tags:
  - vps
categories: []
date: 2022-11-22 22:31:00
---
https://gitee.com/SuperManito/LinuxMirrors https://github.com/SuperManito/LinuxMirrors

`bash <(curl \-sSL https://gitee.com/SuperManito/LinuxMirrors/raw/main/ChangeMirrors.sh) `

如果提示 Command `curl`not found 则说明当前未安装 curl 软件包

`sudo yum install -y curl  sudo apt-get install -y curl`

如果提示 Command `wget`not found 则说明当前未安装 wget 软件包

`sudo yum install -y wget  sudo apt-get install -y wget`

如果提示 bash: /proc/self/fd/11: No such file or directory，请切换至 Root 用户执行