# 回调和流式输出

通过 `config={"callbacks": [...]}` 可以观察模型开始、结束和错误。回调适合轻量记录与计时，流式应用还可通过 `stream()` 消费输出。本地模型便于重复验证事件。

使用 Python 3.11+。在仓库根目录运行 `python -m pip install -r requirements.txt`。

[Notebook](./09_Callbacks.ipynb)

```python
from langchain_core.callbacks import BaseCallbackHandler
from langchain_core.language_models.fake_chat_models import FakeMessagesListChatModel
from langchain_core.messages import AIMessage

class Recorder(BaseCallbackHandler):
    def on_chat_model_start(self, serialized, messages, **kwargs):
        print("开始", len(messages))

    def on_llm_end(self, response, **kwargs):
        print("结束", len(response.generations))

model = FakeMessagesListChatModel(responses=[AIMessage(content="你好")])
response = model.invoke("问候读者", config={"callbacks": [Recorder()]})
print(response.content)
```

[LangChain Python 文档](https://docs.langchain.com/oss/python/langchain/overview)
