title: Hexo的配置
author: daoke
author_id: defaultAuthorId
language: zh-CN
tags:
  - hexo
categories:
  - hexo
date: 2022-11-22 22:15:00
---
<pre class="EnlighterJSRAW" data-enlighter-language="md">
docker exec -it hexo /bin/bash 

git config --global user.email "xxxx@gmail.com"
git config --global user.name "xxxx"

生成 和 发布
hexo clean && hexo d github

生成 hexo g && hexo generate

发布hexo d   && hexo deploy