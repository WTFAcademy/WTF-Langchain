# 用 Runnable 组合多步流程

`|` 将模板、模型和解析器组合成一个 Runnable，每一步的输出会传给下一步。这里用本地测试模型展示数据流。换成 `ChatOpenAI` 后仍可使用相同的 `invoke()` 和 `batch()` 调用。

使用 Python 3.11+。在仓库根目录运行 `python -m pip install -r requirements.txt`。

[Notebook](./06_Chains.ipynb)

```python
from langchain_core.language_models.fake_chat_models import FakeMessagesListChatModel
from langchain_core.messages import AIMessage
from langchain_core.output_parsers import StrOutputParser
from langchain_core.prompts import ChatPromptTemplate

prompt = ChatPromptTemplate.from_template("用一句话介绍 {topic}。")
model = FakeMessagesListChatModel(responses=[AIMessage(content="LangChain 可以组合语言模型应用。")])
chain = prompt | model | StrOutputParser()
print(chain.invoke({"topic": "LangChain"}))
```

[LangChain Python 文档](https://docs.langchain.com/oss/python/langchain/overview)
