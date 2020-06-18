---
title: "Effective Python"
date: 2020-03-16

summary: "《Effective+Python》读书笔记"
description: "Python 编程技巧"
keywords: []
tags: ["writing"]
categories: ["python"]

comment: false # 评论

toc: true # 目录
autoCollapseToc: true # 目录自动定位

postMetaInFooter: false # 包含作者，上次修改时间，markdown链接，许可信息
hiddenFromHomePage: false # 是否从homepage隐藏
contentCopyright: false # 定义另一个contentCopyright。例如contentCopyright：“copyright string”
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

# 《Effective+Python》读书笔记

个人学习笔记

如果有侵权情况，请给我发邮件通知我删除 526191197@qq.com

### 用 zip 函数同时遍历多个迭代器

用 `zip` 可以把两个或两个以上的迭代器封装成生成器，以便稍后求值

```python
names = ['aaaa', 'bb', 'ccc']
letters = [len(n) for n in names]
for name, count in zip(names, letters):
    if count > max_letters:
        longest_name = name
        max_letters = count
```

当 `zip` 封装的任意一个迭代器耗尽，`zip` 就不再产生元组

`itertools` 内置模块中的 `zip_longest` 函数可以平行地遍历多个迭代器，不用考虑长度是否相等。

### for/else 的含义

```python
for i in range(3):
    print('Loop %d' % i)
    if i == 1:
        break
else:
    print('Else block!')
```

这里在 for 循环后面紧跟 else 语句，在 for 正常执行完时会执行 else 语句；而如果经过 break 跳出，会导致程序不执行 else 块

尽量不要这样去写，很不直观

### 合理使用 try/except/else/finally

try 用来执行可能发生异常的代码块，else 用来执行 try 正常运行后的代码块，except 用来捕获 try 中发生的异常，finally 是最终都要执行的代码块，做清理工作

### 用异常表示特殊情况，而不要返回 None

很多时候我会这样写：

```python
def divide(a, b):
    try:
        return a / b
    except ZeroDivisionError:
        return None

result = divide(x, y)
if result is None:
    print('Invalid inputs')
```

但是如果输入比较特殊，如 `x=0, y=1` 时，计算结果为 0，就会发生错误

两种解决办法：

- 将 divide 修改
  ```python
  def divide(a, b):
  try:
      return True, a / b
  except ZeroDivisionError:
      return False, None
  success, result = divide(x, y)
  if not success:
      print('Invalid inputs')
  ```
- 将异常抛给上一级
  ```python
  def divide(a, b):
      try:
          return a / b
      except ZeroDivisionError as e:
          raise ValueError('Invalid inputs') from e
  x, y = 5, 2
  try:
      result = divide(x, y)
  except ValueError:
      print('Invalid inputs')
  else:
      print('Result is %.1f' % result)
  ```

`None, 0, ''` 都会被评估为 false

### 了解闭包

```python
def sort_priority(values, group):
    def helper(x):
        if x in group:
            return (0, x)
        return (1, x)
    values.sort(key=helper)
```

`helper` 函数能访问 `sort_priority` 的 group 参数，是因为它是一个闭包。

python 使用特殊的规则来比较两个元组/列表，它首先比较元组下标为 0 的对应元素，如果相等，再比较下标为 1 的对应元素。

当表达式在引用变量时，python 解释器按照如下顺序遍历各作用域，以解析该引用：

- 当前函数的作用域
- 任何外围的作用域
- 当前代码那个模块（文件）的作用域（全局作用域）
- 内置作用域（包含 `len()`、`str()` 等函数的作用域）

如果上面这几个地方都没有定义过名称相符的变量，就抛出 NameError 异常

给变量赋值时，如果当前作用域已经定义了这个变量，那么变量就会具备新值，若是当前作用域没有这个变量，python 则会把这次赋值视为对该变量的定义。

nonlocal 语句表示 如果在闭包内给该变量赋值，则修改的其实是闭包外那个作用域中的变量

global 表示对该变量赋值操作，将会直接修改模块作用域里的那个变量
