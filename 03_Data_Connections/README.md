# 数据连接

文档读取、切分、嵌入和检索是独立步骤。先保留 `Document` 的元数据，再切分内容。下面用可重复的本地嵌入测试索引流程。实际应用把 `DeterministicFakeEmbedding` 换成 `OpenAIEmbeddings`，用同一模型建立索引和查询。

使用 Python 3.11+。在仓库根目录运行 `python -m pip install -r requirements.txt`。

[Notebook](./03_Data_Connections.ipynb)

```python
from langchain_core.documents import Document
from langchain_core.embeddings import DeterministicFakeEmbedding
from langchain_core.vectorstores import InMemoryVectorStore
from langchain_text_splitters import RecursiveCharacterTextSplitter

documents = [Document(page_content="分块将长文档拆成可检索的小段。" * 4, metadata={"source": "lesson"})]
chunks = RecursiveCharacterTextSplitter(chunk_size=30, chunk_overlap=5).split_documents(documents)
store = InMemoryVectorStore.from_documents(chunks, embedding=DeterministicFakeEmbedding(size=8))
for match in store.similarity_search("分块", k=2):
    print(match.metadata, match.page_content)
```

[LangChain Python 文档](https://docs.langchain.com/oss/python/langchain/overview)
