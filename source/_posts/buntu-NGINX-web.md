title: 如何在 Ubuntu 上安装 NGINX
author: daoke
author_id: defaultAuthorId
language: zh-CN
tags:
  - nginx
categories:
  - vps
date: 2022-11-29 22:47:00
---
NGINX是一个开源的Web服务器软件。您可以将NGINX部署为独立的Web服务器，也可以部署为其他Web服务器前面的代理（实质上是反向代理）。Nginx 是托管高流量网站的最佳网络服务器之一。

在本教程中，我们将重点介绍如何在Ubuntu 20.04 上将 NGINX 安装为独立的 Web 服务器。

在 Ubuntu 20.04 上安装 NGINX
首先运行`sudo apt-get update`以检索有关新的和更新的软件包的信息，然后再继续安装NGINX。

Nginx 在 Ubuntu 软件包存储库中可用。因此，使用以下命令很容易安装 Nginx：

`sudo apt-get install nginx`
检查 NGINX 服务状态
让我们做一个快速检查以确认NGINX服务的状态，运行以下命令：

`sudo systemctl status nginx`
上述命令的输出确认 NGINX 处于活动状态并正在运行。如果您收到一条消息，指示 NGINX 处于非活动状态、未启动或未运行，则可以通过运行以下命令手动启动 NGINX 服务。

`systemctl start nginx`
要检查 Nginx 版本，请运行：

`sudo dpkg -l nginx`
输出显示，在编写本教程时，Nginx 版本 1.18.0正在 Ubuntu 20.04 上运行。

测试 NGINX Web 服务器
确认NGINX服务处于活动状态并正在运行后，您现在可以通过打开首选的Web浏览器并输入安装了NGINX的服务器的IP地址（http：//your_server_ip）来测试Web服务器。

您应该会看到标题为“欢迎来到nginx！"
要在端口 443 上允许 NGINX，请执行以下操作：

`sudo ufw allow 'Nginx HTTPS'`
设置 NGINX 服务器块
如果您想在同一台NGINX Web服务器上托管多个网站，则需要设置服务器块。服务器块也称为虚拟主机（主要在 Apache 中）。

NGINX只预配置了一个服务器块，这是默认网站（/etc/nginx/sites-available）的配置细节存储的地方（/var/www/html）。

一起来看看吧。
> sudo ls -l /etc/nginx/sites-available
total 8
-rw-r--r-- 1 root root 2416 Mar 26  2020 default

运行以下命令以显示默认服务器块文件的内容。

`sudo cat /etc/nginx/sites-available/default | more`
按键盘上的空格键可一次向下滚动一页。您将看到该文件包含默认的服务器配置详细信息，例如侦听端口号，文档根目录（即存储网站内容的基本文件夹），索引文件和服务器名称。

您还应该看到标题为“虚拟主机配置”的部分，如下所示。您可以在此处配置其他网站，但最好创建一个单独的服务器块文件并保留默认文件。
`/etc/nginx/sites-available/default`
```
# Virtual Host configuration for example.com
#
# You can move that to a different file under sites-available/ and symlink that
# to sites-enabled/ to enable it.
#
#server {
#	listen 80;
#	listen [::]:80;
#
#	server_name example.com;
#
#	root /var/www/example.com;
#	index index.html;
#
#	location / {
#		try_files $uri $uri/ =404;
#	}
#}
```
同时，复制上面的示例配置信息并将其保存在文本编辑器中。我们将很快使用这些信息。

创建网站根目录
接下来，您需要在 /var/www下创建一个根文件夹来存储其他网站的内容。例如，我将为我的 domain1.com 网站创建一个名为 domain1.com 的文件夹。

注意：您应该将域名 1 替换为您自己的注册域名。您还应该更新DNS记录，以将您的域名指向NGINX Web服务器的公共IP地址。

`sudo mkdir /var/www/domain1.com`
创建索引文件
索引文件是打开网站时显示的主网页。运行以下命令为其他网站创建索引文件。

`sudo nano /var/www/domain1.com/index.html`
我在这个例子中使用了nano，但你可以使用你最喜欢的文本编辑器。接下来，您可以复制并粘贴以下 HTML 代码以进行测试。
```html
<!DOCTYPE html>
<html>
<head>
<title>Welcome to Domain1!</title>
<style>
body {
width: 35em;
margin: 0 auto;
font-family: Tahoma, Verdana, Arial, sans-serif;
}
</style>
</head>
<body>
<h1>Welcome to Domain1!</h1>
<p>If you see this page, the Domain1 website is working!</p>
</body>
</html>
```
保存更改并关闭文本编辑器。

创建服务器块
下一步是创建服务器块文件以保存其他网站的配置详细信息。运行以下命令。

`sudo nano /etc/nginx/sites-available/domain1`
复制之前保存的示例虚拟主机配置信息，并将其粘贴到新文件中。从“服务器”行开始，确保删除所有#符号以取消注释指令。另外，请记住将“domain1”替换为您自己的注册域名。
```html
# Virtual Host configuration for example.com
#
# You can move that to a different file under sites-available/ and symlink that
# to sites-enabled/ to enable it.
#
server {
       listen 80;
       listen [::]:80;

       server_name domain1.com www.domain1.com;

       root /var/www/domain1.com;
       index index.html;

       location / {
               try_files $uri $uri/ =404;
       }
}
```
保存更改并关闭此文件。
启用服务器块
要让 NGINX 知道其他网站可用，请运行以下命令以创建指向服务器块文件的符号链接。

` ln -s /etc/nginx/sites-available/domain1 /etc/nginx/sites-enabled`
测试您的配置
运行`sudo nginx -t`以测试您的服务器块配置。您应该会看到一条消息，指示一切正常。
```
sudo nginx -t
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
```
您可以运行sudo 服务 nginx重新加载来重新加载配置文件。

测试您的新网站
打开网络浏览器并输入您的新网站地址。您应该看到为新网站创建的索引文件的内容，而不是默认的NGINX网页。
控制NGINX的基本命令
让我们学习基本的Nginx命令来管理你的Web服务器。

重新启动命令将停止该服务，然后重新启动它。

`sudo systemctl restart nginx`
reload命令告诉 NGINX 重新加载其配置文件，但不停止服务。
`sudo systemctl reload nginx`

stop命令将停止 NGINX 服务。

 `sudo systemctl stop nginx`
要使Nginx 服务能够在启动时启动，请运行

`sudo systemctl enable nginx`
注意：默认情况下，Nginx服务启用为在服务器启动时自动启动。

基本NGINX配置和日志文件
`/etc/nginx` --包含所有 NGINX 配置文件

`/etc/nginx/sites-available`--包含服务器块文件，这些文件存储为一个或多个网站提供服务的配置详细信息

`/etc/nginx/sites-enabled` --包含一个或多个已启用网站的配置文件

`/etc/nginx/nginx.conf` --主配置文件，也读取其他文件中的配置指令

`/var/log/nginx/access.log` --用于存储有关您网站的所有访问的信息的默认位置

`/var/log/nginx/error.log` --存储 NGINX 错误的默认位置

结论
通过遵循本指南，您应该能够在 Ubuntu 20.04 服务器上启动并运行一个或多个网站。但是，如果您遇到任何问题，请随时在下面的评论部分告诉我们，我们将尽最大努力为您提供帮助。
转:https://linoxide.com/install-nginx-on-ubuntu-20-04/