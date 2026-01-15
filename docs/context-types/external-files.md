# External Files as Context

> Leveraging documents, spreadsheets, and other file formats to provide rich context for AI agents.

## Overview

External files represent one of the most powerful ways to provide domain-specific knowledge to AI agents. By incorporating PDFs, spreadsheets, presentations, and other document formats, you can give AI assistants access to your organization's institutional knowledge, research papers, technical documentation, and more.

## Supported File Types

### Documents
| Format | Description | Best For |
|--------|-------------|----------|
| **PDF** | Portable Document Format | Research papers, manuals, reports |
| **DOCX** | Microsoft Word | Business documents, proposals |
| **TXT** | Plain text | Simple notes, logs, configuration |
| **MD** | Markdown | Technical documentation, READMEs |
| **RTF** | Rich Text Format | Cross-platform documents |

### Spreadsheets
| Format | Description | Best For |
|--------|-------------|----------|
| **XLSX** | Microsoft Excel | Data analysis, financial models |
| **CSV** | Comma-Separated Values | Data imports, simple datasets |
| **TSV** | Tab-Separated Values | Database exports |

### Presentations
| Format | Description | Best For |
|--------|-------------|----------|
| **PPTX** | Microsoft PowerPoint | Slide decks, visual content |
| **KEY** | Apple Keynote | macOS presentations |

### Code & Technical
| Format | Description | Best For |
|--------|-------------|----------|
| **JSON** | JavaScript Object Notation | Configuration, API responses |
| **YAML** | YAML Ain't Markup Language | Configuration files |
| **XML** | Extensible Markup Language | Data interchange |

## Processing Techniques

### 1. Direct Upload
Most AI platforms support direct file upload:
- **Claude**: Drag & drop files into chat or Projects
- **ChatGPT**: File uploads with Advanced Data Analysis
- **Cursor**: Add files to context via `@file` or project knowledge

### 2. Chunking Strategies
For large documents, consider these chunking approaches:

```
Semantic Chunking
├── Split by sections/headings
├── Maintain paragraph integrity
└── Preserve context windows

Fixed-Size Chunking
├── Split by token count (e.g., 512 tokens)
├── Add overlap (e.g., 50 tokens)
└── Simple but may break context

Recursive Chunking
├── Try large chunks first
├── Split only if exceeds limit
└── Preserves document structure
```

### 3. Document Parsing

**PDF Parsing Tools:**
- [PyPDF2](https://github.com/py-pdf/pypdf) - Pure Python PDF library
- [pdfplumber](https://github.com/jsvine/pdfplumber) - Extract text and tables
- [Unstructured](https://github.com/Unstructured-IO/unstructured) - Multi-format document processing
- [LlamaParse](https://github.com/run-llama/llama_parse) - GenAI-native document parsing

**Spreadsheet Processing:**
- [pandas](https://pandas.pydata.org/) - Data manipulation and analysis
- [openpyxl](https://openpyxl.readthedocs.io/) - Read/write Excel files
- [csv module](https://docs.python.org/3/library/csv.html) - Built-in CSV handling

## Best Practices

### Document Preparation
1. **Clean your documents**: Remove watermarks, headers/footers if not relevant
2. **Use OCR for scans**: Tools like Tesseract or cloud OCR services
3. **Add metadata**: Include title, author, date for better retrieval
4. **Structure content**: Use clear headings and sections

### Context Optimization
1. **Prioritize relevance**: Only include documents that add value
2. **Summarize when possible**: Create abstracts for long documents
3. **Version control**: Track document updates
4. **Consider recency**: Newer documents may be more relevant

### Privacy & Security
1. **Redact sensitive data**: PII, financial info, credentials
2. **Check data retention policies**: Understand where files are stored
3. **Use local processing**: For sensitive documents, process locally first

## Platform-Specific Guides

### Claude Projects
- Upload up to 200,000 tokens of project knowledge
- Files persist across conversations
- Supports PDF, TXT, CSV, and code files
- [Official Documentation](https://support.anthropic.com/en/articles/9519177-how-can-i-create-and-manage-projects)

### ChatGPT with Code Interpreter
- Supports various file formats
- Can process and analyze spreadsheets
- Files persist within conversation
- [ChatGPT File Upload Guide](https://help.openai.com/en/articles/8555545-file-uploads-faq)

### Cursor
- Add files to `.cursor/` directory for persistent context
- Use `@file` mentions for specific file context
- Supports codebase indexing
- [Cursor Context Documentation](https://docs.cursor.com/context/)

## Tools & Services

### Document Processing Platforms
- [Unstructured.io](https://unstructured.io/) - ETL for LLMs, handles 25+ file types
- [LlamaIndex](https://www.llamaindex.ai/) - Data framework with document loaders
- [LangChain Document Loaders](https://python.langchain.com/docs/integrations/document_loaders/) - 100+ document integrations

### Vector Storage for Documents
- [Pinecone](https://www.pinecone.io/) - Managed vector database
- [Chroma](https://www.trychroma.com/) - Open-source embedding database
- [Weaviate](https://weaviate.io/) - Vector search engine

## Example Workflow

```python
# Example: Loading PDF documents for RAG
from langchain.document_loaders import PyPDFLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain.embeddings import OpenAIEmbeddings
from langchain.vectorstores import Chroma

# 1. Load document
loader = PyPDFLoader("research_paper.pdf")
documents = loader.load()

# 2. Split into chunks
text_splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200
)
chunks = text_splitter.split_documents(documents)

# 3. Create embeddings and store
embeddings = OpenAIEmbeddings()
vectorstore = Chroma.from_documents(chunks, embeddings)

# 4. Query relevant context
results = vectorstore.similarity_search("What are the key findings?")
```

## Related Resources

- [RAG Best Practices](https://www.pinecone.io/learn/retrieval-augmented-generation/)
- [Document AI with LLMs](https://www.llamaindex.ai/blog/how-to-build-a-document-ai-agent)
- [Handling Large Documents](https://python.langchain.com/docs/how_to/document_loader_large_files/)

---

[← Back to README](../../README.md)
