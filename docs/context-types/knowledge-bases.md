# Knowledge Bases & Vector Storage

> Building persistent, searchable knowledge stores for AI agents using embeddings and vector databases.

## Overview

Knowledge bases provide AI agents with access to large amounts of information that would otherwise exceed context window limits. By converting text into vector embeddings and storing them in specialized databases, agents can retrieve the most relevant information for any given query.

## Core Concepts

### What are Embeddings?
Embeddings are numerical representations of text that capture semantic meaning. Similar concepts have similar embeddings, enabling semantic search.

```
Text: "How to train a neural network"
     ↓ Embedding Model
Vector: [0.023, -0.156, 0.892, ..., 0.445]  (1536 dimensions)
```

### How Vector Search Works
1. **Indexing**: Convert documents to embeddings, store in vector DB
2. **Query**: Convert user question to embedding
3. **Search**: Find vectors closest to query (cosine similarity, etc.)
4. **Retrieve**: Return original text chunks

## Vector Databases

### Managed Solutions
| Database | Description | Best For | Link |
|----------|-------------|----------|------|
| **Pinecone** | Fully managed vector database | Production apps, simplicity | [Website](https://www.pinecone.io/) |
| **Weaviate Cloud** | Managed Weaviate service | Hybrid search needs | [Website](https://weaviate.io/) |
| **Qdrant Cloud** | Managed Qdrant hosting | High performance | [Website](https://qdrant.tech/) |
| **Zilliz Cloud** | Managed Milvus | Enterprise scale | [Website](https://zilliz.com/) |

### Self-Hosted / Open Source
| Database | Description | Best For | Link |
|----------|-------------|----------|------|
| **Chroma** | AI-native embedding database | Rapid prototyping, local dev | [GitHub](https://github.com/chroma-core/chroma) |
| **Weaviate** | Vector search engine | Hybrid search, GraphQL API | [GitHub](https://github.com/weaviate/weaviate) |
| **Qdrant** | Vector similarity search | Performance-critical apps | [GitHub](https://github.com/qdrant/qdrant) |
| **Milvus** | Cloud-native vector database | Large-scale deployments | [GitHub](https://github.com/milvus-io/milvus) |
| **pgvector** | Postgres vector extension | Existing Postgres users | [GitHub](https://github.com/pgvector/pgvector) |
| **LanceDB** | Serverless vector database | Embedded applications | [GitHub](https://github.com/lancedb/lancedb) |

### Comparison Matrix

| Feature | Pinecone | Chroma | Weaviate | Qdrant |
|---------|----------|--------|----------|--------|
| Managed Service | Yes | No | Yes | Yes |
| Self-Hosted | No | Yes | Yes | Yes |
| Hybrid Search | Limited | No | Yes | Yes |
| Filtering | Yes | Yes | Yes | Yes |
| Pricing | Per vector | Free | Free/Paid | Free/Paid |
| Setup Complexity | Low | Very Low | Medium | Medium |

## Embedding Models

### OpenAI Embeddings
```python
from openai import OpenAI

client = OpenAI()
response = client.embeddings.create(
    model="text-embedding-3-small",  # or text-embedding-3-large
    input="Your text here"
)
embedding = response.data[0].embedding
```

**Models:**
- `text-embedding-3-small`: 1536 dims, cheaper, fast
- `text-embedding-3-large`: 3072 dims, more accurate
- [OpenAI Embeddings Guide](https://platform.openai.com/docs/guides/embeddings)

### Open Source Embeddings
| Model | Dimensions | Description | Link |
|-------|------------|-------------|------|
| **all-MiniLM-L6-v2** | 384 | Fast, lightweight | [HuggingFace](https://huggingface.co/sentence-transformers/all-MiniLM-L6-v2) |
| **bge-large-en-v1.5** | 1024 | High quality | [HuggingFace](https://huggingface.co/BAAI/bge-large-en-v1.5) |
| **nomic-embed-text** | 768 | Long context (8192) | [HuggingFace](https://huggingface.co/nomic-ai/nomic-embed-text-v1.5) |
| **e5-large-v2** | 1024 | Microsoft, high quality | [HuggingFace](https://huggingface.co/intfloat/e5-large-v2) |

```python
from sentence_transformers import SentenceTransformer

model = SentenceTransformer('all-MiniLM-L6-v2')
embeddings = model.encode(['Your text here'])
```

### Cohere Embeddings
```python
import cohere

co = cohere.Client('your-api-key')
response = co.embed(
    texts=["Your text here"],
    model="embed-english-v3.0",
    input_type="search_document"
)
```

## Building a Knowledge Base

### Step 1: Document Processing
```python
from langchain.document_loaders import DirectoryLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter

# Load documents
loader = DirectoryLoader('./docs', glob="**/*.md")
documents = loader.load()

# Split into chunks
splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200,
    separators=["\n\n", "\n", " ", ""]
)
chunks = splitter.split_documents(documents)
```

### Step 2: Create Embeddings & Store
```python
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import Chroma

embeddings = OpenAIEmbeddings()
vectorstore = Chroma.from_documents(
    documents=chunks,
    embedding=embeddings,
    persist_directory="./chroma_db"
)
```

### Step 3: Query the Knowledge Base
```python
# Similarity search
results = vectorstore.similarity_search(
    "What is context engineering?",
    k=5
)

# With relevance scores
results_with_scores = vectorstore.similarity_search_with_score(
    "What is context engineering?",
    k=5
)
```

## RAG (Retrieval-Augmented Generation)

### Basic RAG Pipeline
```python
from langchain.chains import RetrievalQA
from langchain.chat_models import ChatOpenAI

llm = ChatOpenAI(model="gpt-4")
qa_chain = RetrievalQA.from_chain_type(
    llm=llm,
    chain_type="stuff",
    retriever=vectorstore.as_retriever(search_kwargs={"k": 5})
)

response = qa_chain.invoke("What is context engineering?")
```

### Advanced RAG Techniques

#### 1. Hybrid Search
Combine vector search with keyword search:
```python
from langchain.retrievers import BM25Retriever, EnsembleRetriever

# BM25 for keyword matching
bm25_retriever = BM25Retriever.from_documents(chunks)

# Vector retriever
vector_retriever = vectorstore.as_retriever()

# Combine both
ensemble_retriever = EnsembleRetriever(
    retrievers=[bm25_retriever, vector_retriever],
    weights=[0.3, 0.7]
)
```

#### 2. Contextual Compression
Re-rank and compress retrieved documents:
```python
from langchain.retrievers import ContextualCompressionRetriever
from langchain.retrievers.document_compressors import LLMChainExtractor

compressor = LLMChainExtractor.from_llm(llm)
compression_retriever = ContextualCompressionRetriever(
    base_compressor=compressor,
    base_retriever=vector_retriever
)
```

#### 3. Parent Document Retrieval
Retrieve small chunks but return larger context:
```python
from langchain.retrievers import ParentDocumentRetriever
from langchain.storage import InMemoryStore

parent_splitter = RecursiveCharacterTextSplitter(chunk_size=2000)
child_splitter = RecursiveCharacterTextSplitter(chunk_size=400)

store = InMemoryStore()
retriever = ParentDocumentRetriever(
    vectorstore=vectorstore,
    docstore=store,
    child_splitter=child_splitter,
    parent_splitter=parent_splitter
)
```

## Best Practices

### Chunking Strategies
1. **Size matters**: 256-1024 tokens typically works well
2. **Overlap**: 10-20% overlap preserves context
3. **Semantic boundaries**: Split at paragraphs/sections when possible
4. **Metadata**: Include source, date, section info

### Index Optimization
1. **Choose right index type**: HNSW for speed, IVF for memory efficiency
2. **Tune parameters**: `ef_construction`, `M` for HNSW
3. **Monitor recall**: Test retrieval quality regularly
4. **Update strategy**: Incremental updates vs full rebuilds

### Query Optimization
1. **Query expansion**: Rephrase queries for better matching
2. **Filtering**: Use metadata filters to narrow results
3. **Reranking**: Use cross-encoders for better precision
4. **Hybrid search**: Combine semantic and keyword search

## Tools & Frameworks

### RAG Frameworks
| Framework | Description | Link |
|-----------|-------------|------|
| **LlamaIndex** | Data framework for LLM apps | [Website](https://www.llamaindex.ai/) |
| **LangChain** | Building LLM applications | [Website](https://www.langchain.com/) |
| **Haystack** | NLP framework with RAG support | [Website](https://haystack.deepset.ai/) |
| **Verba** | RAG assistant by Weaviate | [GitHub](https://github.com/weaviate/Verba) |

### Evaluation Tools
| Tool | Description | Link |
|------|-------------|------|
| **Ragas** | RAG evaluation framework | [GitHub](https://github.com/explodinggradients/ragas) |
| **TruLens** | LLM app evaluation | [GitHub](https://github.com/truera/trulens) |
| **Phoenix** | ML observability | [GitHub](https://github.com/Arize-ai/phoenix) |

## Platform Integration

### Claude + Knowledge Base
Use Claude's Projects feature to upload documents:
- Upload PDFs, code files, text documents
- Claude indexes and searches automatically
- Limited to 200K tokens per project

### ChatGPT + Retrieval
- Use ChatGPT's file upload feature
- Or build custom GPT with retrieval
- [Custom GPTs with Knowledge](https://help.openai.com/en/articles/8673914-gpts-knowledge)

### Cursor + Codebase
- Cursor automatically indexes your codebase
- Use `@codebase` to search across files
- Add documentation to `.cursor/` directory

## Example: Full Knowledge Base Setup

```python
# Complete example: Building a documentation knowledge base

import os
from langchain.document_loaders import DirectoryLoader, TextLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import Chroma
from langchain.chat_models import ChatOpenAI
from langchain.chains import ConversationalRetrievalChain

# 1. Load documents
loader = DirectoryLoader(
    './documentation',
    glob="**/*.md",
    loader_cls=TextLoader
)
docs = loader.load()

# 2. Split documents
splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200
)
chunks = splitter.split_documents(docs)

# 3. Create vector store
embeddings = OpenAIEmbeddings()
vectorstore = Chroma.from_documents(
    chunks,
    embeddings,
    persist_directory="./db"
)

# 4. Create conversational chain
llm = ChatOpenAI(model="gpt-4", temperature=0)
qa_chain = ConversationalRetrievalChain.from_llm(
    llm=llm,
    retriever=vectorstore.as_retriever(search_kwargs={"k": 5}),
    return_source_documents=True
)

# 5. Query
chat_history = []
result = qa_chain({
    "question": "How do I implement feature X?",
    "chat_history": chat_history
})
print(result["answer"])
```

## Further Reading

- [Pinecone RAG Guide](https://www.pinecone.io/learn/retrieval-augmented-generation/)
- [LlamaIndex RAG Tutorial](https://docs.llamaindex.ai/en/stable/getting_started/concepts/)
- [Choosing Vector Database](https://www.sicara.fr/blog-technique/how-to-choose-your-vector-database-in-2024)
- [RAG Best Practices](https://www.anthropic.com/news/contextual-retrieval)

---

[← Back to README](../../README.md)
