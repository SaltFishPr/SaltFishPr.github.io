# git 常用命令


# Git

## 初次运行 Git 前的配置

设置用户名和邮箱

```shell
git config --global user.name "Salt Fish"
git config --global user.email saltfishpr@gmail.com
```

配置默认文本编辑器

```shell
git config --global core.editor code
```

检查配置信息

```shell
git config --list
```

## 设置保存密码

```shell
# 记住密码（默认15分钟）
git config --global credential.helper cache

# 自己设置时间
git config --global credential.helper cache --timeout=3600

# 长期存储密码
git config --global credential.helper store
```

增加远程地址的时候带上密码

```
git remote -v

# https://yourname:password@github.com/saltfishpr/go-learning.git
```

## Git clone/push 太慢

在国内，github 域名被限制，导致 git clone 很慢，只有 40KB/s 的速度

### Windows

使用 `nslookup` 查询 github 对应的 IP 地址

```shell
nslookup github.global.ssl.fastly.net
nslookup github.com
```

把查询到的结果添加到 `C:\Windows\System32\drivers\etc\hosts` 中

```text
github.global.ssl.fastly.net 69.63.184.14
github.com 140.82.112.3
```

刷新 DNS 缓存

```shell
ipconfig /flushdns
```

### linux

使用 `nslookup` 查询 github 对应的 IP 地址

```shell
nslookup github.global.ssl.fastly.net
nslookup github.com
```

把查询到的结果添加到 `/etc/hosts` 中

```text
github.global.ssl.fastly.net 69.63.184.14
github.com 140.82.112.3
```

刷新 DNS 缓存

```shell
sudo nscd -i hosts
```

