title: 在LNMP环境下实现反向代理
author: daoke
tags: []
categories:
  - lnmp
date: 2022-11-10 22:25:00
---


安装LNMP稳定版只需要安装nginx

<pre class="EnlighterJSRAW" data-enlighter-language="md">wget http://soft.vpser.net/lnmp/lnmp1.9.tar.gz -cO lnmp1.9.tar.gz &amp;&amp; tar zxf lnmp1.9.tar.gz &amp;&amp; cd lnmp1.9 &amp;&amp; ./install.sh nginx</pre>



<strong>添加网站(虚拟主机)</strong>

<code>lnmp vhost add</code>

编辑网站nginx配置文件

<code>cd usr/local/nginx/conf/vhost</code>

<code>ls</code>

<code>xxx.com.conf</code>

配置Nginx文件，将下列代码插入nginx文件中，注意端口、路径要保持一致
<pre class="EnlighterJSRAW" data-enlighter-language="asm">location /app
{
proxy_redirect off;
proxy_pass http://127.0.0.1:8443;
proxy_http_version 1.1;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
proxy_set_header Host $http_host;
}</pre>
