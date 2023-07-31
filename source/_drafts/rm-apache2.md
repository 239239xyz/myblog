title: debian Ubuntu卸载apache2
author: daoke
author_id: defaultAuthorId
language: zh-CN
tags:
  - vps
categories:
  - vps
date: 2022-11-23 00:02:00
---
<pre class="EnlighterJSRAW" data-enlighter-language="md">
apt-get --purge remove apache2
apt-get --purge remove apache2.2-common
apt-get --purge remove apache2-doc
apt-get --purge remove apache2-utils
apt-get autoremove