---
title: 01. Hello Langchain
tags:
  - openai
  - llm
  - langchain
---

# WTF Langchain极简入门: 01. Hello Langchain

最近在学习Langchain框架，顺手写一个“WTF Langchain极简入门”，供小白们使用（编程大佬可以另找教程）。本教程默认以下前提：
- 使用Python版本的[Langchain](https://github.com/hwchase17/langchain)
- LLM使用OpenAI的模型
- Langchain目前还处于快速发展阶段，版本迭代频繁，为避免示例代码失效，本教程统一使用版本 **0.0.235**

根据Langchain的[代码约定](https://github.com/hwchase17/langchain/blob/v0.0.235/pyproject.toml#L14C1-L14C24)，Python版本 ">=3.8.1,<4.0"。

推特：[@verysmallwoods](https://twitter.com/verysmallwoods)

所有代码和教程开源在github: [github.com/sugarforever/wtf-langchain](https://github.com/sugarforever/wtf-langchain)

-----

## Langchain 简介

大型语言模型（LLM）正在成为一种具有变革性的技术，使开发人员能够构建以前无法实现的应用程序。然而，仅仅依靠LLM还不足以创建一个真正强大的应用程序。它还需要其他计算资源或知识来源。

`Langchain` 旨在帮助开发这些类型应用程序，比如：
- 基于文档数据的问答
- 聊天机器人
- 代理

## OpenAI 简介

`OpenAI` 是LLM生态的模型层最大的玩家之一。大家目前熟知的 *GPT-3.5*，*GPT-4* 等模型都是OpenAI的产品。它的API允许开发人员通过简单的API调用来访问这些模型。

## Langchain与OpenAI

`Langchain` 作为一个开源框架，提供与OpenAI等语言模型的接口，简化了在应用程序中集成和利用语言模型能力的过程。

## 开发前的准备

在开始第一个Langchain应用程序之前，我们需要做一些必要的准备。

### Google Colab

本教程中，我们将使用 `Google Colab` 在云端运行 `Python` 代码。Google Colab（全称Google Colaboratory）是一个由Google提供的云端开发环境，用于数据分析、机器学习和深度学习任务。它基于Jupyter Notebook，提供了一个免费的、云端的Python编程环境，用户可以直接在浏览器中编写和执行Python代码。

网址：[https://colab.research.google.com/](https://colab.research.google.com/)

![Google Colab](./google_colab.png)

无法使用Google Colab的同学，可以使用Visual Studio Code配合Jupyter插件在本地运行代码。

### OpenAI API Key

在Langchain应用中使用OpenAI的模型，我们需要一个API Key。点击[https://platform.openai.com/account/api-keys](https://platform.openai.com/account/api-keys)创建你的API Key。

## 第一个 Langchain 应用

在独立的 Python 环境安装 LangChain 的 OpenAI 集成。

```shell
python -m pip install -U langchain-openai
```

代码会读取环境变量 `OPENAI_API_KEY`。未设置时，它会提示你输入密钥。可通过 `OPENAI_MODEL` 指定项目有权使用的聊天模型。详见 [LangChain ChatOpenAI 文档](https://docs.langchain.com/oss/python/integrations/chat/openai)。

[Hello_Langchain.ipynb](./Hello_Langchain.ipynb)

```python
import os
from getpass import getpass

from langchain_openai import ChatOpenAI

if not os.environ.get("OPENAI_API_KEY"):
    os.environ["OPENAI_API_KEY"] = getpass("OpenAI API key: ")

chat = ChatOpenAI(model=os.environ.get("OPENAI_MODEL", "gpt-5-mini"))
response = chat.invoke("Hello Langchain!")
print(response.content)
```

## 运行代码

在 Google Colab 的页面，在每一个代码块，按 Ctrl/Cmd + Enter 即可运行代码，非常方便。

![](./hello_langchain.png)

## 总结
本节课程中，我们简要介绍了 `Langchain`，`OpenAI` 以及它们的关系，并完成了第一个 `Langchain` 应用 —— `Hello Langchain`。

### 相关文档资料链接：
1. [Python Langchain官方文档](https://python.langchain.com/docs/get_started/introduction.html) 