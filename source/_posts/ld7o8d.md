title: VPS采用Ed25519密钥登录
tags:
  - vps
categories: []
date: 2022-05-26 00:00:00
---
本文抄自：<a href="https://github.com/nkypy/blog/issues/17">https://github.com/nkypy/blog/issues/17</a>

正文前先对比一下：

3072的RSA的安全性=256的ECC(包括ECDSA和25519(EdDSA))

25519相对于传统NIST P-256曲线的ECDSA效率与速度更快且为确定性签名。
<!-- more -->
一、本地配置

1.1 在Windows PowerShell中运行命令

<code>ssh-keygen -t ed25519 -C "oracledubai" # 随便起个名称,本例以oracleduba</code>

<code>Enter passphrase (empty for no passphrase):</code>

#对密钥加个私人密码，输入密码或回车留空。

<code>Enter same passphrase again:</code><code># 确认密码或回车。</code>

#显示

<code>Generating public/private ed25519 key pair.Enter file in which to save the key (/home/lenovo/.ssh/id_ed25519): #文件保存位置,一般默认即可。</code>

#显示

<code>Enter passphrase (empty for no passphrase): #对密钥加个私人密码，输入密码或回车留空。
Enter same passphrase again:# 确认密码或回车。</code>

#显示
<pre><code>Your identification has been saved in /home/lenovo/.ssh/id_ed25519 #生成的私钥
Your public key has been saved in /home/lenovo/.ssh/id_ed25519.pub #生成的公钥
The key fingerprint is:
SHA256:PdwszW0JXELbXMuQXDGN+xfKfCMd6YQWEB4 oracledubai #指纹
The key's randomart image is:
+--[ED25519 256]--+
|  Eo.+ . .o ..ooo|
| . .= = .  + . +=|
| ..= = o  o o . o|
|o = o o  o B = . |
|o+ +    S = B +  |
|oo+        o .   |
|=  o             |
|..o              |
|..               |
+----[SHA256]-----+</code></pre>

1. id_ed25519<code>是私钥，</code>id_ed25519.pub是公钥。

2. 保存在C:\Users\lenovo\.ssh路径中。lenovo为计算机名，不知道是啥的看上面路径home后面的名称。

二、VPS端操作

2.1 以root登录vps

<code>mkdir /root/.ssh/</code>

2.2 上传公钥

将id_ed25519.pub上传到/root/.ssh/路径下，并运行

<code>cat /root/.ssh/id_ed25519.pub &gt;&gt;  /root/.ssh/authorized_keys</code>

2.3 修改密钥项

<code>vim /etc/ssh/sshd_config</code>

将下列两行的井号去掉

<code>#PubkeyAuthentication yes    #允许密钥认证</code>

<code>#AuthorizedKeysFile    .ssh/authorized_keys .ssh/authorized_keys2    #默认公钥位置</code>

<code>service sshd restart #重启sshd</code>

2.4 以密钥方式重新登录

注意：以密钥方式重新登录

2.5 关闭密码登录

运行的前提是：能够密钥登录VPS

<code>vim /etc/ssh/sshd_config</code>

将后面的<code>PasswordAuthentication yes</code>

修改为：<code>PasswordAuthentication no</code>

<code>service sshd restart #重启sshd</code>

三、常见的几种密钥生成方式对比

DSA: 它是不安全的，甚至从OpenSSH第7版开始就不再支持，你需要升级它!

RSA: 这取决于密钥大小。如果它有3072或4096位的长度，那么你就很好。低于这个长度，你可能要升级它。1024位的长度甚至被认为是不安全的。

ECDSA：这取决于你的机器能产生一个随机数的程度，这个随机数将被用来创建签名。在ECDSA使用的NIST曲线上也有一个可信度问题。

#Ed25519: 这是目前最值得推荐的公钥算法!

转:<a href="https://winamp.top/230.html">https://winamp.top/230.html</a>