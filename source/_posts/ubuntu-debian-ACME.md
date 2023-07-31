title: ubuntu-debian安装ACME
author: daoke
author_id: defaultAuthorId
language: zh-CN
tags:
  - ACME
  - ubuntu
  - debian
categories:
  - VPS
date: 2022-11-30 11:21:00
---
获取 acme.sh
acme.shshell 脚本可自动颁发和续订来自 Let's Encrypt 的免费证书。您可以通过直接从 Web 下载脚本或克隆其 git 项目来获取 acme.sh 脚本。
从网络下载 acme.sh
运行以下两个命令中的任何一个以下载并执行 acme.sh 脚本。
`curl https://get.acme.sh | sh -s email=xx@cc.com`
`wget -O -  https://get.acme.sh | sh -s email=xx@xx.net`
安装出现错误，安装crontab与socat
`apt install cron socat -y`
一旦看到“安装成功！”消息，您都可以关闭终端窗口并再次打开它以验证安装。
若要查看 acme.sh 使用情况信息，请运行下一个命令。
`acme.sh -h`
您也可以运行以下命令来检查 acme.sh 版本。
`acme.sh --version`
生成证书
要为单个域生成单个证书，请运行以下命令。

将`yourdomain.com`替换为您注册的域。此外，根据需要将`/home/www/yourdomain.com`替换为您域的网站根文件夹。
`acme.sh --issue -d yourdomain.com -w /var/www/yourdomain.com`
对于共享同一网站根文件夹的多个域/子域，您可以运行下一个命令来颁发证书。
`acme.sh --issue -d yourdomain.com -d www.yourdomain.com -d subdomain.yourdomain.com -w /var/www/yourdomain.com`
生成的证书将存储在 ~/.acme.sh/yourdomain.com 中
#### 使用 acme 在 NGINX 上安装证书
通过 acme.sh 脚本生成证书后，下一步是将其安装在NGINX上。首先，创建一个文件夹，将生成的证书复制到该文件夹。
`sudo mkdir -p /etc/nginx/certs/yourdomain.com`
运行下一个命令以安装证书。不要忘记将yourdomain.com替换为您注册的域。
`acme.sh --install-cert -d yourdomain.com --key-file /etc/nginx/certs/yourdomain.com/key.pem --fullchain-file /etc/nginx/certs/yourdomain.com/cert.pem --reloadcmd "service nginx force-reload"`
更新 NGINX 服务器块文件
最后一步是更新域的服务器块文件以包含与 SSL 相关的指令。
运行以下命令以编辑服务器块文件。
`sudo nano /etc/nginx/sites-available/yourdomain.com`
接下来，添加以下行。
```
listen [::]:443 ssl ipv6only=on;
listen 443 ssl;
ssl_certificate /etc/nginx/certs/cloudindevs.com/cert.pem;
ssl_certificate_key /etc/nginx/certs/cloudindevs.com/key.pem;
```
保存更改并关闭文件。
![](https://cdn.daoke.bid/web123/acme-nginx.webp)

使用以下命令重新启动 NGINX：

`sudo systemctl restart nginx`
在浏览器中访问您的网站，以确认现在已启用安全通信。

证书续订
Let's Encrypt颁发的证书将每60天自动续订一次。

但是，如果您愿意，也可以手动续订证书。运行以下命令。

`acme.sh --renew -d yourdomain.com --force`
若要停止证书续订，请运行以下命令。

`acme.sh --remove -d yourdomain.com`
升级 acme.sh
建议始终使用最新版本的 acme.sh。运行以下命令以确保自动更新 acme.sh。

`acme.sh --upgrade --auto-upgrade`
若要禁用 acme.sh 的自动升级，请运行下一个命令。

`acme.sh --upgrade --auto-upgrade 0`
如果您不希望 acme.sh 自动升级，请使用以下命令手动更新它。

`acme.sh --upgrade`
结论
在本指南中，我们描述了使用 Ubuntu 上的 acme.sh shell 脚本从 Let's Encrypt 获取和续订免费 SSL/TLS 证书的步骤。此方法是使用 Certbot 工具的替代方法。我们想听听您使用这些工具的经验。