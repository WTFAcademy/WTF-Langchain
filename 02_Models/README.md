# 模型与消息

聊天模型接收有角色的消息序列。系统消息描述任务，用户消息提供输入，`invoke()` 返回 `AIMessage`。用 `batch()` 处理多组输入，用 `stream()` 逐步读取生成结果。

使用 Python 3.11+。在仓库根目录运行 `python -m pip install -r requirements.txt`。 在终端环境中设置 `OPENAI_API_KEY`。可选的 `OPENAI_MODEL` 用于指定你的项目可访问的聊天模型。

[Notebook](./Models.ipynb)

```python
import os
from langchain.messages import HumanMessage, SystemMessage
from langchain_openai import ChatOpenAI

model = ChatOpenAI(model=os.environ.get("OPENAI_MODEL", "gpt-5-mini"))
messages = [SystemMessage(content="用一句话回答。"), HumanMessage(content="什么是 LangChain？")]
response = model.invoke(messages)
print(response.content)
```

[LangChain Python 文档](https://docs.langchain.com/oss/python/langchain/overview)
