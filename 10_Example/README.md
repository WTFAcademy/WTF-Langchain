# 用自己的 PDF 做检索问答

准备一份本地 PDF。先把文档拆成小段，再检索与问题最相关的段落。最后把检索结果放到提示词里。OpenAI 嵌入接口会接收文档片段，请使用允许发送到模型提供商的文档。

使用 Python 3.11+。在仓库根目录运行 `python -m pip install -r requirements.txt`。 在终端环境中设置 `OPENAI_API_KEY`。可选的 `OPENAI_MODEL` 用于指定你的项目可访问的聊天模型。 通过 `TUTORIAL_PDF` 设置 PDF 路径，默认读取当前目录的 `example.pdf`。

[Notebook](./10_Example.ipynb)

```python
import os
from langchain_community.document_loaders import PyMuPDFLoader
from langchain_core.prompts import ChatPromptTemplate
from langchain_core.vectorstores import InMemoryVectorStore
from langchain_openai import ChatOpenAI, OpenAIEmbeddings
from langchain_text_splitters import RecursiveCharacterTextSplitter

pages = PyMuPDFLoader(os.environ.get("TUTORIAL_PDF", "example.pdf")).load()
chunks = RecursiveCharacterTextSplitter(chunk_size=800, chunk_overlap=100).split_documents(pages)
embeddings = OpenAIEmbeddings(model="text-embedding-3-small")
store = InMemoryVectorStore.from_documents(chunks, embedding=embeddings)
question = "这份文档的主要结论是什么？"
matches = store.similarity_search(question, k=3)
context = "\n\n".join(f"[{i}] {match.page_content}" for i, match in enumerate(matches, 1))
prompt = ChatPromptTemplate.from_messages([
    ("system", "只使用给定的片段回答并标注编号。如果材料不足，说明缺少什么。"),
    ("human", "问题：{question}\n\n片段：\n{context}"),
])
model = ChatOpenAI(model=os.environ.get("OPENAI_MODEL", "gpt-5-mini"))
print((prompt | model).invoke({"question": question, "context": context}).content)
```

[LangChain Python 文档](https://docs.langchain.com/oss/python/langchain/overview)
