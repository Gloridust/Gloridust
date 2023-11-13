---
title: 利用百度千帆大模型平台，我做了个针对初学者的代码诊断工具
tags: Python
layout: post
date: 2023-11-13 +0800
categories: 技术
img: /images/2023-11-13/cover.png
describe: 对于编程初学者，他们可能会在起步阶段遇到各种各样的问题。这些问题通常是非常简单、典型的；但是绝大多数人并不知道如何诊断并解决这些问题，也不知道如何查找资料。于是，我利用大模型，做了这样一个代码诊断平台。
---

## 起因

作为大一新生的一员，我依靠着我那点在过去8年积攒的少的可怜的编程经验在本专业大一年级中崭露头角。在参加一个校级比赛的时候，我正寻思还能做什么。我从需求出发，思考周围的人目前的需求。

我突然想到，他们好像在专业起步阶段每天都会遇到很多问题。这些问题通常是非常简单、典型的；但是绝大多数人并不知道如何诊断并解决这些问题，也不知道如何查找资料。他们只能选择以“提问”的方式解决问题。显然，这种情况下，“解答者”是供不应求的。我突然联想到了[解决编程自学痛点：配合 GPT 类大模型高效学习](https://gloridust.xyz/%E6%8A%80%E6%9C%AF/2023/08/24/studywithgpt.html)这篇文章的内容。

于是，我利用大模型，做了这样一个代码诊断平台。

## 链接

该项目在Github开源：[https://github.com/Gloridust/DebugThinker-python-UI](https://github.com/Gloridust/DebugThinker-python-UI) 你可以在release下载最新版的编译版本，开箱即用，内置api，无需再自行注册百度千帆平台的API接口。

编译版本也提供网盘下载：[https://cloud.gloridust.xyz/s/EOOtQ](https://cloud.gloridust.xyz/s/EOOtQ)

如果你在微信读到这篇文章并且无法打开链接，请尝试点击“阅读原文”，在我的博客中获取链接。

***

以下内容来自该项目README文档：

## DebugThinker-python-UI

一个基于百度千帆大模型平台的快速代码报错诊断工具，能帮助初学者快速解决代码问题，提高学习效率。

## 原项目

原命令行版本：[https://github.com/Gloridust/DebugThinker-python](https://github.com/Gloridust/DebugThinker-python)  
原命令行版本已停止更新与维护，新版本具有以下优势：

- 更友好的图形化交互UI界面，使用更便捷
- 支持多行换行输入，能解决多行代码

## Release日志

### v1.1.1

利用pyinstaller打包成一个可执行文件，极大程度上减少了使用成本，开箱即用。你也可以用以下命令自行打包：

```
pyinstaller --onefile start.py
```

### v1.1.0

利用threading进行了多线程优化，解决了在点击按钮提交后卡死的问题。

- Fix：提交后窗口卡死

### v1.0.0

利用tkinter制作了GUI，这是第一个可以正常使用的版本。

## 开始使用

### 获取API

1.注册并 登录 [百度千帆大模型平台控制台](https://console.bce.baidu.com/qianfan/ais/console/onlineService) 在“在线服务”区域，开通“ERNIE-Bot大模型公有云在线调用服务”。

![img1](/images/2023-11-13/1.png)

2.在“应用接入”处创建应用，得到API_KEY和SECRET_KEY并保存好。

![img1](/images/2023-11-13/2.png)

### 配置程序

1.将同目录下的[example.config.py](./example.config.py)重命名为[config.py](./config.py)文件

2.将刚刚获得的API_KEY和SECRET_KEY填入[config.py](./config.py)并保存。

### 使用

1.在项目根目录中打开终端，运行[start.py](./start.py)主程序。

```
python ./start.py
```

2.根据需求将代码和补充内容输入到对应文本框中，点击“提交”，然后稍等片刻即可得到结果。

## 排错

1.KeyError: 'result'

```
Exception in Tkinter callback
Traceback (most recent call last):
  File "C:\Users\glori\AppData\Local\Programs\Python\Python311\Lib\tkinter\__init__.py", line 1948, in __call__
    return self.func(*args)
           ^^^^^^^^^^^^^^^^
  File "E:\GitHub\DebugThinker-python-UI\start.py", line 62, in run_program
    self.print_to_output(result['result'])
                         ~~~~~~^^^^^^^^^^
KeyError: 'result'
```

返回的json中没有result这个key，请检查你的config文件是否正确。

2.远程主机强迫关闭了一个现有的连接

```
    raise ProxyError(e, request=request)
requests.exceptions.ProxyError: HTTPSConnectionPool(host='aip.baidubce.com', port=443): Max retries exceeded with url: /oauth/2.0/token?grant_type=client_credentials&client_id=%09xlebbPKwsMWyCsMvLO13Irvs&client_secret=OaTeLIwkmX3IiQUGRaAydTGYW5A5SC35 (Caused by ProxyError('Cannot connect to proxy.', ConnectionResetError(10054, '远程主机强迫关闭了一个现有的连接。', None, 10054, None)))
```

检查网络连接，是否启用代理。在少部分网络环境下，需要关闭代理运行该程序，否则百度千帆可能会禁止访问。。