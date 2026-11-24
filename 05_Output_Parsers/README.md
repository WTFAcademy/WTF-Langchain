# 输出解析与结构化输出

已有文本可以用输出解析器转换。模型支持结构化输出时，可以通过 `with_structured_output()` 把模式直接交给模型，再用 Pydantic 校验结果。

使用 Python 3.11+。在仓库根目录运行 `python -m pip install -r requirements.txt`。 在终端环境中设置 `OPENAI_API_KEY`。可选的 `OPENAI_MODEL` 用于指定你的项目可访问的聊天模型。

[Notebook](./05_Output_Parsers.ipynb)

```python
import os
from langchain_core.output_parsers import CommaSeparatedListOutputParser
from langchain_openai import ChatOpenAI
from pydantic import BaseModel

print(CommaSeparatedListOutputParser().parse("Python, Rust, Cairo"))

class Summary(BaseModel):
    topic: str
    facts: list[str]

model = ChatOpenAI(model=os.environ.get("OPENAI_MODEL", "gpt-5-mini"))
summary = model.with_structured_output(Summary).invoke("用两个要点解释文档检索。")
print(summary.model_dump())
```

[LangChain Python 文档](https://docs.langchain.com/oss/python/langchain/overview)
