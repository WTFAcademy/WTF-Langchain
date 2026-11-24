# Agent 和工具调用

工具由 Python 函数定义。参数注解与文档字符串帮助模型选择工具并生成参数。`create_agent()` 负责模型与工具之间的循环。示例只调用纯函数，可通过 `recursion_limit` 限制图执行步数。

使用 Python 3.11+。在仓库根目录运行 `python -m pip install -r requirements.txt`。 在终端环境中设置 `OPENAI_API_KEY`。可选的 `OPENAI_MODEL` 用于指定你的项目可访问的聊天模型。

[Notebook](./08_Agents.ipynb)

```python
import os
from langchain.agents import create_agent
from langchain.tools import tool
from langchain_openai import ChatOpenAI

@tool
def square(number: int) -> int:
    """返回整数的平方。"""
    return number * number

model = ChatOpenAI(model=os.environ.get("OPENAI_MODEL", "gpt-5-mini"))
agent = create_agent(model=model, tools=[square])
answer = agent.invoke(
    {"messages": [{"role": "user", "content": "请使用工具计算 17 的平方。"}]},
    config={"recursion_limit": 10},
)
print(answer["messages"][-1].content)
```

[LangChain Python 文档](https://docs.langchain.com/oss/python/langchain/overview)
