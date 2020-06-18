---
title: "Python Encode"
date: 2020-03-06
lastmod: 2020-03-06

summary: "python编码问题"
keywords: []
tags: []
categories: ["python"]

comment: false # 评论

toc: true # 目录
autoCollapseToc: false # 目录自动定位

postMetaInFooter: true # 包含作者，上次修改时间，markdown 链接，许可信息
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

# 编码

&emsp;&emsp;编码是数据从一种格式变为另一种格式的过程。通过编码，我们可以把数据以不同的格式保存和转移。

## Python3 字符串类型

python3 中字符串有 `str` 和 `byte` 两种类型，前者的实例**包含**原始的字节，后者的实例**包含** Unicode 字符。

str(Unicode) 通过 `encode` 编码后形成 byte(二进制数据)。
二进制数据通过`decode`转换成 Unicode 字符。

示例 1：

```python
s1 = "中文"
s2 = s1.encode("utf-8")
print(type(s1), s1)
print(type(s2), s2)
```

output 1:

```
<class 'str'> 中文
<class 'bytes'> b'\xe4\xb8\xad\xe6\x96\x87'
```

&emsp;&emsp;文件中存储的都是 byte 这样一个一个二进制数，所以在将 str 存入文件或从文件读取内容时需要指明编码类型。

## 经验总结

> 写 python 程序的时候，把编码和解码操作放在界面的最外围来做，程序的核心部分使用 Unicode 字符类型(Python3 中的 str)。
>
> <p align="right">——Effective+Python</p>

1. 在写文件时注明编码类型

   ```python
   with open(file_path, 'w',encode='utf-8') as f:
       f.write(data)
   ```

2. 在使用 `requests` 库获取响应信息时，指明编码类型

   ```python
   def send_req(url):
       headers = {}
       req = requests.get(url, headers)
       req.encoding = "utf-8"
       if req.ok:
           print(req.text)
           return req.text
       else:
           return Exception("访问失败")
   ```

3. base64 编码

   Base64 是网络上最常见的用于传输 8Bit 字节码的编码方式之一，可用于在 HTTP 环境下传递较长的标识信息。采用 Base64 编码具有不可读性，需要解码后才能阅读。

   将文件转换成 base64 编码形式进行传输。

   ```python
   import base64
   with open(file_path, 'rb') as f:
      res = base64.b64encode(f.read())
   ```

4. python3 中 str 和 bytes 互换函数

   ```python
   def to_str(bytes_or_str):
       if isinstance(bytes_or_str, bytes):
               value = bytes_or_str.decode('utf-8')
           else:
               value = bytes_or_str
           return value

   def to_bytes(bytes_or_str):
       if isinstance(bytes_or_str, str):
               value = bytes_or_str.encode('utf-8')
           else:
               value = bytes_or_str
           return value
   ```

参考资料：

1. [base64 编码](https://baike.baidu.com/item/base64/8545775?fr=aladdin "百度百科：base64编码")
2. Effective+Python
