---
title: "Pip and Conda"
date: 2020-03-26

summary: "pip 和 conda 常用命令"
description: ""
keywords: ["conda", "pip"]
tags: ["technique"]
categories: ["python"]

comment: false # 评论

toc: false # 目录
autoCollapseToc: false # 目录自动定位

postMetaInFooter: true # 包含作者，上次修改时间，markdown链接，许可信息
hiddenFromHomePage: false # 是否从homepage隐藏
reward: false # 文章打赏

mathjax: false # 启用mathjax
mathjaxEnableSingleDollar: false # 是否使用 $...$ 即可進行inline latex渲染
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

# conda 常用

### 换源

创建 .condarc 配置文件

```bash
conda config --set show_channel_urls yes
```

用文本编辑器打开 `~/.condarc` 填入以下内容

```
channels:
  - defaults
show_channel_urls: true
channel_alias: https://mirrors.tuna.tsinghua.edu.cn/anaconda
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/pro
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

### 命令

创建有最新版本的 python 的环境 `conda create -n <env-name> python=3`

删除环境 `conda remove -n <env-name> --all`

清除缓存 `conda clean -a`

---

# pip 常用

### 换源

在 `~/.config/pip/pip.conf` 中添加如下内容

```
[global]
index-url = https://mirrors.aliyun.com/pypi/simple/

[install]
trusted-host=mirrors.aliyun.com
```

### 删除缓存

找到 `~/.cache/pip` 文件夹，删除即可

在安装时使用

```bash
pip install <package-name> --no-cache-dir
```

# 导入导出 python 环境

今天在学习数据挖掘的时候，nolearn 和 lasagne 两个库的时候给我的 jypyterlab 环境搞崩了，只好 remove --all 重新配起，真后悔没有先搞个环境备份( ´•︵•` )

## conda

conda 是个好东西，可以自己处理环境依赖，缺点就是。。包有点老，有些包还找不到

### 导入环境

```bash
conda create --name <your env name> --file <this file> --yes
```

### 导出环境

```bash
conda list -e > requirements.txt
```

## pip

pip 也有很多优点，我一般是在 conda 找不到模块的时候使用 pip

### 导入环境

```bash
pip install -r requirements.txt
```

### 导出环境

```bash
pip freeze > requirements.txt
```

---

总之，用 conda 创造独立的环境，用 pip 安装~~新鲜~~的模块，两个工具结合起来用甚好
