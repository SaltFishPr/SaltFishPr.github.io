---
title: "Aliang's Video"
date: 2020-03-04
lastmod: 2020-03-04

summary: ""
description: ""
keywords: []
tags: ["technique"]
categories: []

comment: false # 评论

toc: false # 目录
autoCollapseToc: false # 目录自动定位

postMetaInFooter: false # 包含作者，上次修改时间，markdown 链接，许可信息
hiddenFromHomePage: false # 是否从 homepage 隐藏
contentCopyright: false # 定义另一个 contentCopyright。例如 contentCopyright：“这是另一种版权。”
reward: false # 文章打赏

mathjax: false # 启用 mathjax
mathjaxEnableSingleDollar: false # 是否使用 $...$ 即可進行 inline latex 渲染
mathjaxEnableAutoNumber: false # 公式自动编号

hideHeaderAndFooter: false # 您未列出的帖子，您可能不希望显示页眉或页脚
enableOutdatedInfoWarning: false # 启用或禁用过时的内容警告

flowchartDiagrams: # 流程图
  enable: false
  options: ""
sequenceDiagrams: # 序列图
  enable: false
  options: ""
---

推送好友的 b 站视频，顺便测试 iframe 嵌入式网页（狗头)

<!--more-->

<div>
  <iframe margin:auto src="//player.bilibili.com/player.html?aid=93178052&cid=159088790&page=1" scrolling="no" border="0"
    id="video-iframe" width="100%" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>
  <script>
    var em = document.getElementById("video-iframe");
    console.log(em.clientWidth);
    em.height = em.clientWidth * 0.75
  </script>
</div>
