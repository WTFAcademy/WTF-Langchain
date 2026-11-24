# 提示词和示例选择

`ChatPromptTemplate` 把变量填到不同角色的消息中。用少样本模板加入输入和期望输出，让模型看到具体格式。示例也可以先由选择器筛选，再交给模板。

使用 Python 3.11+。在仓库根目录运行 `python -m pip install -r requirements.txt`。

[Notebook](./04_Prompts.ipynb)

```python
from langchain_core.prompts import ChatPromptTemplate, FewShotChatMessagePromptTemplate

example_prompt = ChatPromptTemplate.from_messages([("human", "{word}"), ("ai", "{opposite}")])
few_shot = FewShotChatMessagePromptTemplate(
    example_prompt=example_prompt,
    examples=[{"word": "热", "opposite": "冷"}, {"word": "高", "opposite": "低"}],
)
prompt = ChatPromptTemplate.from_messages([
    ("system", "给出反义词。"), few_shot, ("human", "{word}"),
])
for message in prompt.invoke({"word": "快"}).to_messages():
    print(message.type, message.content)
```

[LangChain Python 文档](https://docs.langchain.com/oss/python/langchain/overview)
