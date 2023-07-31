title: vps常用一键脚本与命令
author: daoke
date: 2022-11-10 22:24:20
tags:
---
<p style="text-align: left;">更新源</p>
<code>apt update &amp;&amp; apt upgrade -y</code>
<h4>安装常用软件</h4>
<code>apt install net-tools vim wget git screen curl xz-utils unzip sudo zip t -y</code>
<!-- more -->
<h4>ssh与宝塔出现乱码</h4>
<blockquote>ubuntu与debian适用</blockquote>
<code>sudo echo 'LC_ALL="en_US.UTF-8"' &gt;&gt; /etc/default/locale</code>
<h4>开启宝塔ipv6</h4>
<code>echo True &gt; /www/server/panel/data/ipv6.pl</code>

<code>bt restart</code>
<h4>服务器时间</h4>
<code>sudo dpkg-reconfigure tzdata</code>
<h4>免密登录，公钥换成自己的</h4>
<pre class="EnlighterJSRAW" data-enlighter-language="latex">mkdir /root/.ssh;echo "ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIN+I1HhcBcgv/6WGPTmnsUuV3cun1mUOAmwBdJnk7JwL xingchen
" &gt;&gt;/root/.ssh/authorized_keys</pre>
&nbsp;

<hr />

<h4>caddy安装</h4>
<code>echo "deb [trusted=yes] https://apt.fury.io/caddy/ /" | sudo tee -a /etc/apt/sources.list.d/caddy-fury.list</code>

<code>apt install caddy</code>
<h4>开启ubuntu与debian bbr加速</h4>
<pre class="EnlighterJSRAW" data-enlighter-language="abap">echo "net.core.default_qdisc=fq" &gt;&gt; /etc/sysctl.conf echo "net.ipv4.tcp_congestion_control=bbr" &gt;&gt; /etc/sysctl.conf</pre>
<pre>测试IPv4优先还是IPv6优先
<code>curl ip.p3terx.com</code></pre>
<pre>测试25端口是否开放
telnet smtp.aol.com 25</pre>

非大陆Docker安装

`wget -qO- get.docker.com | bash`

非大陆Docker-compose安装 
<pre>sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose sudo chmod +x /usr/local/bin/docker-compose</pre>