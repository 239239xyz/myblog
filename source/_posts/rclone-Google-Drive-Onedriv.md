title: 使用 rclone 每天定时备份博客网站内容及mysql数据库到 Google Drive/Onedrive等网盘
author: daoke
author_id: defaultAuthorId
language: zh-CN
tags:
  - rclone
  - 备份
categories:
  - vps
date: 2022-11-22 11:56:00
---

## RCLONE 官方提供了一键安装脚本

`curl https://rclone.org/install.sh | sudo bash`
rclone config
<h2>备份脚本编写及授权</h2>
创建脚本文件：

<code>mkdir /home/Backup</code>

<code>chmod +x /home/Backup</code>

<code class="hljs">touch /home/Backup/backup.sh
vi /home/Backup/backup.sh</code>


<pre class="EnlighterJSRAW" data-enlighter-language="md">#!/bin/bash
# 定义GOOGLE DRIVE的备份目录
GD_PATH="GdriveBackup:Backup"

# 定义备份的目录及文件，不同的目录用空格分开
BACKUP_SRC="/home/wwwroot/omo.moe/usr"

# 定义临时文件存放目录
BACKUP_DST="/home/Backup"

# 设置MYSQL基本信息 
MYSQL_SERVER="localhost"
MYSQL_USER="root"
MYSQL_PASS="your password"

# 定义想要备份的数据库，多个数据库用空格分开
BACKUP_DATABASE="typecho_omo"

# 定义文件前缀名
NOW=$(date +"%Y.%m.%d")
OLD=$(date -d -5day +"%Y.%m.%d")

# 备份网站数据文件
zip -r $BACKUP_DST/auto_fileData_$NOW.zip $BACKUP_SRC

# 备份mysql数据库
mysqldump -u $MYSQL_USER -h $MYSQL_SERVER -p$MYSQL_PASS --databases $BACKUP_DATABASE &gt; $BACKUP_DST/$NOW-auto-Databases.sql

# 使用rclone上传到google drive
rclone copy -v --stats 15s --bwlimit 40M $BACKUP_DST/ --include "$NOW-auto-Databases.sql" --include "auto_fileData_$NOW.zip" $GD_PATH

# 删除本地的临时文件
rm -f $BACKUP_DST/$NOW-auto-Databases.sql $BACKUP_DST/auto_fileData_$NOW.zip

# 删除5天前的备份
rclone delete $GD_PATH/ --include "$OLD-auto-Databases.sql" --include "auto_fileData_$OLD.zip"</pre>


使用chmod指令赋予执行权限：

<code>chmod +x /home/Backup/backup.sh</code>

三、创建自动备份任务并测试

使用 crontab 每天4点定时执行自动备份脚本：

<code>crontab -e</code>

复制以下内容粘贴并输入:wq保存：

<code>0 4 * * * /bin/bash /home/backup/backup.sh &gt;/dev/null 2&gt;&amp;1</code>

手动测试看看脚本是否正确运行：

<code>bash /home/Backup/backup.sh</code>

时区设置为东八区：

<code>timedatectl set-timezone Asia/Shanghai</code>

重启定时任务：

<code>service crond restart</code>

重启系统日志：

<code>service rsyslog restart</code>

最后观察下系统日志尾巴状态，是否时区已经调整成功：

<code>tail -f /var/log/cron</code>

转：https://omo.moe/archives/616/