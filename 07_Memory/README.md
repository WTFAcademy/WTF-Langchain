# 对话状态和短期记忆

LangGraph 的 checkpointer 按 `thread_id` 保存消息。同一个 ID 继续对话，新 ID 开始独立会话。内存 checkpointer 适用于本地示例，进程重启会丢失记录。

使用 Python 3.11+。在仓库根目录运行 `python -m pip install -r requirements.txt`。 在终端环境中设置 `OPENAI_API_KEY`。可选的 `OPENAI_MODEL` 用于指定你的项目可访问的聊天模型。

[Notebook](./07_Memory.ipynb)

```python
import os
from langchain.agents import create_agent
from langchain_openai import ChatOpenAI
from langgraph.checkpoint.memory import InMemorySaver

model = ChatOpenAI(model=os.environ.get("OPENAI_MODEL", "gpt-5-mini"))
agent = create_agent(model=model, tools=[], checkpointer=InMemorySaver())
session = {"configurable": {"thread_id": "lesson-07"}}
agent.invoke({"messages": [{"role": "user", "content": "我叫小明。"}]}, config=session)
answer = agent.invoke({"messages": [{"role": "user", "content": "我叫什么？"}]}, config=session)
print(answer["messages"][-1].content)
```

[LangChain Python 文档](https://docs.langchain.com/oss/python/langchain/overview)
