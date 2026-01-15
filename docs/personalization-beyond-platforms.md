# Building Your Personal AI System

> From simple memory services to full self-hosted personal AI stacks.

This guide covers advanced personalization beyond basic platform settings—building systems that truly understand you through persistent memory, knowledge bases, and custom integrations.

---

## Table of Contents

1. [Memory Services](#memory-services)
2. [Personal AI Assistants](#personal-ai-assistants)
3. [Building a Knowledge Base](#building-a-knowledge-base)
4. [Sync & Update Strategies](#sync--update-strategies)
5. [Full Stack Setup](#full-stack-setup)
6. [Privacy-First Setup](#privacy-first-setup)

---

## Memory Services

### What is AI Memory?

Memory allows AI to retain information across conversations:

```
Types of Memory
├── Short-term: Within current conversation
├── Long-term: Facts that persist across sessions
│   ├── Semantic: "User prefers Python"
│   ├── Episodic: "We discussed X last week"
│   └── Procedural: Learned behaviors
└── Knowledge Base: Searchable document store
```

### Cloud Memory Options

| Service | How It Works | Best For |
|---------|--------------|----------|
| **ChatGPT Memory** | Auto-learns from conversations | ChatGPT users |
| **Rewind** | Records everything, AI-searchable | Power users (macOS) |
| **Khoj Cloud** | Connects to your notes/docs | Multi-source context |
| **Personal AI** | Dedicated personal AI | Comprehensive memory |

### Self-Hosted Memory with MCP

The MCP Memory server provides persistent memory for Claude:

```json
{
  "mcpServers": {
    "memory": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-memory"]
    }
  }
}
```

Once configured, you can say:
- "Remember that I prefer TypeScript over JavaScript"
- "What do you remember about my preferences?"

---

## Personal AI Assistants

Self-hosted AI assistants with built-in memory and document access.

### Open WebUI

The most popular self-hosted ChatGPT alternative with memory.

**Features:**
- Persistent conversation memory
- Document upload & RAG
- Multi-model support (local + cloud)
- User management

**Quick Start:**
```bash
# With Ollama (local models)
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  --name open-webui \
  ghcr.io/open-webui/open-webui:main
```

**Link:** [GitHub](https://github.com/open-webui/open-webui)

### Khoj

Personal AI that connects to your notes, docs, and the web.

**Features:**
- Obsidian, Notion, GitHub integration
- Web search capability
- Local or cloud LLM support
- Conversation memory

**Quick Start:**
```bash
# Docker
docker run -d -p 42110:42110 \
  -v khoj_data:/root/.khoj \
  ghcr.io/khoj-ai/khoj:latest
```

**Link:** [khoj.dev](https://khoj.dev/) | [GitHub](https://github.com/khoj-ai/khoj)

### AnythingLLM

All-in-one AI application for chatting with your documents.

**Features:**
- Built-in vector database
- Support for 10+ LLM providers
- Multi-user workspaces
- Document management UI

**Quick Start:**
```bash
docker run -d -p 3001:3001 \
  -v anythingllm:/app/server/storage \
  mintplexlabs/anythingllm
```

**Link:** [GitHub](https://github.com/Mintplex-Labs/anything-llm)

---

## Building a Knowledge Base

Create a searchable database of your personal documents.

### Step 1: Choose a Vector Database

| Database | Pros | Cons | Best For |
|----------|------|------|----------|
| **Chroma** | Easy setup, good docs | Limited scale | Development |
| **LanceDB** | Embedded, no server | Newer | Desktop apps |
| **Qdrant** | Fast, production-ready | More complex | Production |
| **pgvector** | Uses existing Postgres | Postgres required | Postgres users |

### Step 2: Process Your Documents

```python
# Install: pip install langchain langchain-community unstructured

from langchain_community.document_loaders import DirectoryLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter

# Load documents
loader = DirectoryLoader(
    "~/Documents/Notes",
    glob="**/*.md",
    show_progress=True
)
docs = loader.load()

# Split into chunks
splitter = RecursiveCharacterTextSplitter(
    chunk_size=1000,
    chunk_overlap=200,
    separators=["\n\n", "\n", " ", ""]
)
chunks = splitter.split_documents(docs)
print(f"Created {len(chunks)} chunks from {len(docs)} documents")
```

### Step 3: Create Embeddings & Store

**Using Ollama (local, free):**
```python
from langchain_community.embeddings import OllamaEmbeddings
from langchain_community.vectorstores import Chroma

# Create embeddings locally
embeddings = OllamaEmbeddings(model="nomic-embed-text")

# Store in Chroma
vectorstore = Chroma.from_documents(
    documents=chunks,
    embedding=embeddings,
    persist_directory="~/.my-knowledge-base"
)
```

**Using OpenAI (cloud, paid):**
```python
from langchain_openai import OpenAIEmbeddings
from langchain_community.vectorstores import Chroma

embeddings = OpenAIEmbeddings()
vectorstore = Chroma.from_documents(
    documents=chunks,
    embedding=embeddings,
    persist_directory="~/.my-knowledge-base"
)
```

### Step 4: Query Your Knowledge

```python
# Search
results = vectorstore.similarity_search(
    "What are my notes about Python async?",
    k=5
)

for doc in results:
    print(f"From: {doc.metadata.get('source', 'unknown')}")
    print(doc.page_content[:200])
    print("---")
```

---

## Sync & Update Strategies

Keep your AI's knowledge current as your documents change.

### Strategy 1: Manual Re-sync

Simple but requires manual action.

```python
def rebuild_knowledge_base():
    # Delete old data
    vectorstore.delete_collection()
    
    # Reload and rebuild
    docs = loader.load()
    chunks = splitter.split_documents(docs)
    vectorstore = Chroma.from_documents(chunks, embeddings)
```

### Strategy 2: Scheduled Sync

Automatically sync on a schedule.

**Using cron (Linux/Mac):**
```bash
# Run daily at 2 AM
0 2 * * * /usr/bin/python3 /path/to/sync_knowledge.py
```

**sync_knowledge.py:**
```python
#!/usr/bin/env python3
import logging
from datetime import datetime
from pathlib import Path
from langchain_community.document_loaders import DirectoryLoader
from langchain_community.vectorstores import Chroma
from langchain_community.embeddings import OllamaEmbeddings

logging.basicConfig(level=logging.INFO)
logger = logging.getLogger(__name__)

NOTES_PATH = Path.home() / "Documents" / "Notes"
DB_PATH = Path.home() / ".my-knowledge-base"

def sync():
    logger.info(f"Starting sync at {datetime.now()}")
    
    # Load documents
    loader = DirectoryLoader(str(NOTES_PATH), glob="**/*.md")
    docs = loader.load()
    logger.info(f"Loaded {len(docs)} documents")
    
    # Rebuild vector store
    embeddings = OllamaEmbeddings(model="nomic-embed-text")
    vectorstore = Chroma.from_documents(
        docs, embeddings, persist_directory=str(DB_PATH)
    )
    
    logger.info("Sync complete")

if __name__ == "__main__":
    sync()
```

### Strategy 3: Real-time with File Watcher

Sync whenever files change.

```python
# Install: pip install watchdog

from watchdog.observers import Observer
from watchdog.events import FileSystemEventHandler
import time

class NotesHandler(FileSystemEventHandler):
    def on_modified(self, event):
        if event.src_path.endswith('.md'):
            print(f"File changed: {event.src_path}")
            # Trigger re-indexing of this file
            reindex_file(event.src_path)

observer = Observer()
observer.schedule(NotesHandler(), path=str(NOTES_PATH), recursive=True)
observer.start()

try:
    while True:
        time.sleep(1)
except KeyboardInterrupt:
    observer.stop()
observer.join()
```

### Strategy 4: n8n Workflow

Visual workflow automation for complex sync scenarios.

```bash
# Run n8n
docker run -d -p 5678:5678 \
  -v n8n_data:/home/node/.n8n \
  n8nio/n8n
```

Example workflow:
1. Trigger: File change in Google Drive
2. Action: Download file
3. Action: Process with Unstructured
4. Action: Update vector database

---

## Full Stack Setup

Complete personal AI stack with Docker Compose.

```yaml
# docker-compose.yml
version: '3.8'

services:
  # Local LLM Server
  ollama:
    image: ollama/ollama
    ports:
      - "11434:11434"
    volumes:
      - ollama_data:/root/.ollama
    # Uncomment for GPU support
    # deploy:
    #   resources:
    #     reservations:
    #       devices:
    #         - capabilities: [gpu]

  # Chat Interface
  open-webui:
    image: ghcr.io/open-webui/open-webui:main
    ports:
      - "3000:8080"
    volumes:
      - open_webui_data:/app/backend/data
    environment:
      - OLLAMA_BASE_URL=http://ollama:11434
    depends_on:
      - ollama

  # Vector Database
  chroma:
    image: chromadb/chroma
    ports:
      - "8000:8000"
    volumes:
      - chroma_data:/chroma/chroma

  # Workflow Automation
  n8n:
    image: n8nio/n8n
    ports:
      - "5678:5678"
    volumes:
      - n8n_data:/home/node/.n8n

volumes:
  ollama_data:
  open_webui_data:
  chroma_data:
  n8n_data:
```

**Start everything:**
```bash
docker-compose up -d

# Pull a model
docker exec -it ollama ollama pull llama3.2

# Access services:
# - Chat UI: http://localhost:3000
# - Chroma API: http://localhost:8000
# - n8n workflows: http://localhost:5678
```

---

## Privacy-First Setup

Maximum privacy with everything running locally.

### Components

```
Privacy-First Stack
├── LLM: Ollama with Llama 3.2 (runs locally)
├── Embeddings: nomic-embed-text via Ollama (runs locally)
├── Vector DB: Chroma (embedded, no network)
├── Interface: Open WebUI or LM Studio (local)
└── Documents: Local filesystem only
```

### Key Principles

1. **No cloud APIs**: Use only local models
2. **No network calls**: Embed vector DB, use local embeddings
3. **Encrypted storage**: Consider encrypting your knowledge base
4. **Audit access**: Know what data your AI can access

### Offline-Capable Setup

```bash
# 1. Install Ollama
curl -fsSL https://ollama.com/install.sh | sh

# 2. Download models while online
ollama pull llama3.2
ollama pull nomic-embed-text

# 3. Download Open WebUI image
docker pull ghcr.io/open-webui/open-webui:main

# 4. Now you can run completely offline
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  ghcr.io/open-webui/open-webui:main
```

---

## Troubleshooting

### Common Issues

**Ollama connection refused:**
```bash
# Check if Ollama is running
ollama list

# Restart if needed
ollama serve
```

**Open WebUI can't connect to Ollama:**
```bash
# Make sure to use host.docker.internal
-e OLLAMA_BASE_URL=http://host.docker.internal:11434
```

**Chroma out of memory:**
- Use smaller embedding model
- Process fewer documents at once
- Increase Docker memory limit

**Slow embedding generation:**
- Use GPU if available
- Use smaller model (e.g., `all-MiniLM-L6-v2`)
- Process in batches

---

## Further Reading

- [MCP Documentation](https://modelcontextprotocol.io/)
- [Ollama Documentation](https://ollama.com/)
- [Open WebUI Docs](https://docs.openwebui.com/)
- [LangChain RAG Tutorial](https://python.langchain.com/docs/tutorials/rag/)
- [Chroma Documentation](https://docs.trychroma.com/)

---

[← Back to README](../README.md)
