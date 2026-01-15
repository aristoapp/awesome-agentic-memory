# Awesome Context Engineering [![Awesome](https://awesome.re/badge.svg)](https://awesome.re)

> **How to make your AI agents truly understand you?**

This guide walks you through **Context Engineering**—the practice of giving AI assistants the information they need to understand who you are, how you work, and what you need. From simple platform settings to building your own personal memory system.

---

## The Problem

AI assistants are incredibly powerful, but they start every conversation knowing nothing about you:

- They don't know your coding style or preferred frameworks
- They don't remember what you discussed yesterday
- They can't access your notes, documents, or data
- They treat you the same as everyone else

**The result?** You repeat yourself constantly, get generic responses, and never feel like the AI truly "gets" you.

## The Solution: Context Engineering

**Context Engineering** is the practice of systematically providing AI with information about you, your work, and your preferences. It's how you transform a generic assistant into a personalized one that understands your context.

```
Generic AI ──────────────────────────────────────────> Personalized AI
            + Your preferences
            + Your documents
            + Your conversation history
            + Your connected apps
            + Your persistent memory
```

This guide covers everything from basic platform settings to building full personal memory systems.

---

## Contents

1. [Understanding Your Context](#understanding-your-context)
2. [Level 1: Platform Settings](#level-1-platform-settings)
3. [Level 2: Adding Your Data](#level-2-adding-your-data)
4. [Level 3: Memory Systems](#level-3-memory-systems)
5. [Level 4: Full Personal AI Stack](#level-4-full-personal-ai-stack)
6. [Detailed Guides](#detailed-guides)
7. [Resources](#resources)

---

## Understanding Your Context

Before customizing AI, understand what "context" you can provide.

### Types of Context

| Type | What It Is | Examples |
|------|------------|----------|
| **Instructions** | Rules and preferences | "Always use TypeScript", "Be concise" |
| **Background** | Who you are | Role, expertise, company, projects |
| **Documents** | Your files and data | PDFs, notes, spreadsheets, code |
| **Conversation History** | Past interactions | Previous chats, decisions made |
| **External Apps** | Connected services | Notion pages, Slack messages, emails |
| **Memory** | Learned facts | Preferences discovered over time |

### Detailed Guides by Context Type

| Context Type | Guide |
|--------------|-------|
| External Files (PDF, XLSX, etc.) | [📄 Guide](docs/context-types/external-files.md) |
| Conversation & Session History | [💬 Guide](docs/context-types/session-history.md) |
| External Apps (Notion, Slack, etc.) | [🔗 Guide](docs/context-types/external-apps.md) |
| Knowledge Bases & Vector DBs | [🧠 Guide](docs/context-types/knowledge-bases.md) |
| Personal Data & Preferences | [👤 Guide](docs/context-types/personal-data.md) |

---

## Level 1: Platform Settings

**Difficulty: Easy | Time: 5 minutes | No technical skills required**

Start by using built-in personalization features in AI platforms.

### What You Can Do

| Platform | Feature | What It Does |
|----------|---------|--------------|
| **ChatGPT** | Custom Instructions | Tell ChatGPT about yourself and how to respond |
| **ChatGPT** | Memory | ChatGPT learns and remembers facts about you |
| **Claude** | Custom Instructions | Set default behavior and preferences |
| **Claude** | Projects | Upload files and set project-specific context |
| **Gemini** | Saved Info | Tell Gemini facts to remember |
| **Gemini** | Extensions | Connect to Gmail, Drive, Calendar |
| **Cursor** | Rules | Set coding preferences and project context |
| **Copilot** | Instructions | Guide code suggestions |

### Quick Setup Examples

**ChatGPT Custom Instructions:**
```
About me:
- Senior software engineer, 8 years experience
- Primary languages: Python, TypeScript
- Working at a fintech startup

How to respond:
- Be concise, skip basic explanations
- Always include code examples
- Consider security implications
```

**Claude Project Instructions:**
```
You're helping me with a Next.js e-commerce project.
- Framework: Next.js 14 with App Router
- Database: PostgreSQL with Prisma
- Styling: Tailwind CSS

Always suggest TypeScript solutions with proper types.
```

**Cursor Rules (`.cursor/rules`):**
```
- Use TypeScript with strict mode
- Prefer functional components
- Follow project structure in /src
- Include error handling in all functions
```

### Platform Guides

| Platform | Guide |
|----------|-------|
| ChatGPT | [📖 Full Guide](docs/platforms/chatgpt.md) |
| Claude | [📖 Full Guide](docs/platforms/claude.md) |
| Gemini | [📖 Full Guide](docs/platforms/gemini.md) |
| Cursor | [📖 Full Guide](docs/platforms/cursor.md) |
| GitHub Copilot | [📖 Full Guide](docs/platforms/copilot.md) |

---

## Level 2: Adding Your Data

**Difficulty: Medium | Time: 30 min - 2 hours | Some technical knowledge helpful**

Go beyond basic settings by connecting AI to your actual data.

### Option A: Upload Files Directly

Most platforms support file uploads:

| Platform | Method | Limits |
|----------|--------|--------|
| **Claude Projects** | Upload to project | ~200K tokens |
| **ChatGPT** | Upload in chat or GPTs | Per-conversation |
| **Gemini** | Google Drive integration | Via extensions |
| **Cursor** | Add to workspace | Indexed automatically |

**Best for:** Project documentation, reference materials, style guides

### Option B: Model Context Protocol (MCP)

MCP connects AI assistants to your local files and apps.

**Setup for Claude Desktop:**

Create config file at `~/Library/Application Support/Claude/claude_desktop_config.json`:

```json
{
  "mcpServers": {
    "my-documents": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-filesystem", 
               "/Users/you/Documents"]
    },
    "my-notes": {
      "command": "npx",
      "args": ["-y", "mcp-obsidian", 
               "/Users/you/ObsidianVault"]
    },
    "notion": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-notion"],
      "env": {
        "NOTION_API_KEY": "your-key"
      }
    }
  }
}
```

**Available MCP Servers:**

| Server | Connects To | Install |
|--------|-------------|---------|
| Filesystem | Local files | `@modelcontextprotocol/server-filesystem` |
| Obsidian | Obsidian vaults | `mcp-obsidian` |
| Notion | Notion pages | `@modelcontextprotocol/server-notion` |
| Google Drive | Drive files | `@modelcontextprotocol/server-gdrive` |
| Slack | Slack messages | `@modelcontextprotocol/server-slack` |
| GitHub | Repositories | `@modelcontextprotocol/server-github` |
| Memory | Persistent memory | `@modelcontextprotocol/server-memory` |

**Resources:**
- [MCP Documentation](https://modelcontextprotocol.io/)
- [Official MCP Servers](https://github.com/modelcontextprotocol/servers)
- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers)

### Option C: Browser Extensions

Add AI context from any webpage:

| Extension | Features | Link |
|-----------|----------|------|
| **Sider** | AI sidebar, page context | [sider.ai](https://sider.ai/) |
| **Monica** | Browser AI with memory | [monica.im](https://monica.im/) |
| **Merlin** | ChatGPT on any site | [getmerlin.in](https://getmerlin.in/) |

---

## Level 3: Memory Systems

**Difficulty: Medium-Hard | Time: 1-4 hours | Technical knowledge required**

Build systems that remember and learn about you over time.

### Understanding Memory Types

```
Memory Systems
├── Short-term (within conversation)
│   └── What we just discussed
├── Long-term (across conversations)
│   ├── Facts: "User prefers Python"
│   ├── Episodes: "We built X last week"
│   └── Preferences: Learned over time
└── Knowledge Base (searchable documents)
    └── Your notes, docs, files
```

### Option A: Cloud Memory Services

Use managed services for easy setup.

| Service | What It Does | Best For | Link |
|---------|--------------|----------|------|
| **ChatGPT Memory** | Auto-learns from chats | ChatGPT users | Built-in |
| **Claude Projects** | Project-based context | Claude users | Built-in |
| **Rewind** | Records & searches everything | Power users | [rewind.ai](https://www.rewind.ai/) |
| **Khoj Cloud** | Personal AI assistant | All-in-one solution | [khoj.dev](https://khoj.dev/) |
| **Notion AI** | AI over your Notion | Notion users | [notion.so](https://notion.so/) |
| **Mem** | AI-powered notes | Note-takers | [mem.ai](https://mem.ai/) |

### Option B: Self-Hosted Memory

Run your own memory system for privacy and control.

**Personal AI Assistants:**

| Tool | Description | Setup | Link |
|------|-------------|-------|------|
| **Open WebUI** | ChatGPT-like UI with memory | Docker | [GitHub](https://github.com/open-webui/open-webui) |
| **Khoj** | AI connected to your notes | Docker/pip | [GitHub](https://github.com/khoj-ai/khoj) |
| **AnythingLLM** | Chat with documents | Docker/Desktop | [GitHub](https://github.com/Mintplex-Labs/anything-llm) |
| **PrivateGPT** | Fully private document chat | Local | [GitHub](https://github.com/imartinez/privateGPT) |

**Quick Start with Open WebUI:**

```bash
# Start with Docker (connects to Ollama)
docker run -d -p 3000:8080 \
  --add-host=host.docker.internal:host-gateway \
  -v open-webui:/app/backend/data \
  -e OLLAMA_BASE_URL=http://host.docker.internal:11434 \
  --name open-webui \
  ghcr.io/open-webui/open-webui:main
```

Features: Persistent memory, document RAG, multiple models, user management

### Option C: Build Your Own Knowledge Base

Create a searchable database of your documents.

**Vector Database Options:**

| Database | Type | Best For | Link |
|----------|------|----------|------|
| **Chroma** | Self-hosted | Local development | [GitHub](https://github.com/chroma-core/chroma) |
| **LanceDB** | Embedded | Desktop apps | [GitHub](https://github.com/lancedb/lancedb) |
| **Qdrant** | Self-hosted | Production | [GitHub](https://github.com/qdrant/qdrant) |
| **Pinecone** | Cloud | Managed service | [pinecone.io](https://pinecone.io/) |
| **Supabase Vector** | Cloud | Postgres users | [supabase.com](https://supabase.com/) |

**Simple Knowledge Base Setup:**

```python
# pip install chromadb langchain langchain-community

from langchain_community.document_loaders import DirectoryLoader
from langchain.text_splitter import RecursiveCharacterTextSplitter
from langchain_community.vectorstores import Chroma
from langchain_community.embeddings import OllamaEmbeddings

# 1. Load your documents
loader = DirectoryLoader("~/Documents/Notes", glob="**/*.md")
docs = loader.load()

# 2. Split into searchable chunks
splitter = RecursiveCharacterTextSplitter(chunk_size=1000, chunk_overlap=200)
chunks = splitter.split_documents(docs)

# 3. Create searchable database
embeddings = OllamaEmbeddings(model="nomic-embed-text")
vectorstore = Chroma.from_documents(
    chunks, embeddings, 
    persist_directory="~/.my-knowledge-base"
)

# 4. Search your knowledge
results = vectorstore.similarity_search("What did I write about Python?")
```

---

## Level 4: Full Personal AI Stack

**Difficulty: Hard | Time: 1 day+ | Strong technical skills required**

Build a complete personal AI system with full control.

### Architecture Overview

```
┌─────────────────────────────────────────────────────────────┐
│                    Your Personal AI Stack                    │
├─────────────────────────────────────────────────────────────┤
│  Interface Layer                                             │
│  ├── Open WebUI / LibreChat (Chat interface)                │
│  └── Browser extensions (Sider, Monica)                     │
├─────────────────────────────────────────────────────────────┤
│  LLM Layer                                                   │
│  ├── Cloud: OpenAI, Anthropic, Google                       │
│  └── Local: Ollama, LM Studio, vLLM                         │
├─────────────────────────────────────────────────────────────┤
│  Memory & Knowledge Layer                                    │
│  ├── Vector DB: Chroma, Qdrant, Pinecone                    │
│  ├── Document processing: Unstructured, LlamaParse          │
│  └── Memory: MCP Memory server, custom solutions            │
├─────────────────────────────────────────────────────────────┤
│  Data Sources                                                │
│  ├── Local: Files, Obsidian, code                           │
│  ├── Cloud: Notion, Google Drive, GitHub                    │
│  └── Communication: Slack, Email                            │
├─────────────────────────────────────────────────────────────┤
│  Sync & Update Layer                                         │
│  ├── Scheduled sync: cron, n8n, Zapier                      │
│  └── Real-time: webhooks, file watchers                     │
└─────────────────────────────────────────────────────────────┘
```

### Complete Docker Setup

```yaml
# docker-compose.yml - Full personal AI stack
version: '3.8'

services:
  # Local LLM
  ollama:
    image: ollama/ollama
    volumes:
      - ollama_data:/root/.ollama
    ports:
      - "11434:11434"
    deploy:
      resources:
        reservations:
          devices:
            - capabilities: [gpu]

  # Chat interface with memory
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

  # Vector database
  chroma:
    image: chromadb/chroma
    ports:
      - "8000:8000"
    volumes:
      - chroma_data:/chroma/chroma

  # Workflow automation
  n8n:
    image: n8nio/n8n
    ports:
      - "5678:5678"
    volumes:
      - n8n_data:/home/node/.n8n
    environment:
      - GENERIC_TIMEZONE=Asia/Seoul

volumes:
  ollama_data:
  open_webui_data:
  chroma_data:
  n8n_data:
```

### Sync & Update Strategies

**How to keep your AI's knowledge current:**

| Strategy | Method | Best For |
|----------|--------|----------|
| **Manual** | Re-upload files | Occasional updates |
| **Scheduled** | Cron job / n8n workflow | Daily/weekly sync |
| **Real-time** | File watcher / Webhooks | Frequently changing data |
| **On-demand** | MCP servers | Live data access |

**Example: Sync Obsidian notes daily**

```python
# sync_notes.py - Run daily via cron
import os
from pathlib import Path
from langchain_community.document_loaders import ObsidianLoader
from langchain_community.vectorstores import Chroma
from langchain_community.embeddings import OllamaEmbeddings

VAULT_PATH = Path.home() / "ObsidianVault"
DB_PATH = Path.home() / ".ai-knowledge"

def sync():
    # Load notes
    loader = ObsidianLoader(str(VAULT_PATH))
    docs = loader.load()
    
    # Update vector store
    embeddings = OllamaEmbeddings(model="nomic-embed-text")
    vectorstore = Chroma(
        persist_directory=str(DB_PATH),
        embedding_function=embeddings
    )
    
    # Clear and rebuild (simple strategy)
    vectorstore.delete_collection()
    vectorstore = Chroma.from_documents(
        docs, embeddings, persist_directory=str(DB_PATH)
    )
    print(f"Synced {len(docs)} notes")

if __name__ == "__main__":
    sync()
```

### Privacy Considerations

| Concern | Cloud Solution | Self-Hosted Solution |
|---------|---------------|---------------------|
| Data leaves device | Use trusted providers | All data stays local |
| Who sees your data | Provider's privacy policy | Only you |
| Data retention | Provider controls | You control |
| Cost | Subscription | Hardware + electricity |
| Model quality | GPT-4, Claude 3.5 | Llama 3.2, Mistral |

---

## Detailed Guides

### By Context Type
- [External Files (PDF, docs, spreadsheets)](docs/context-types/external-files.md)
- [Session & Conversation History](docs/context-types/session-history.md)
- [External Apps (Notion, Slack, etc.)](docs/context-types/external-apps.md)
- [Knowledge Bases & Vector Storage](docs/context-types/knowledge-bases.md)
- [Personal Data & Preferences](docs/context-types/personal-data.md)

### By Platform
- [ChatGPT Personalization](docs/platforms/chatgpt.md)
- [Claude Personalization](docs/platforms/claude.md)
- [Gemini Personalization](docs/platforms/gemini.md)
- [Cursor Personalization](docs/platforms/cursor.md)
- [GitHub Copilot Personalization](docs/platforms/copilot.md)

### Advanced Topics
- [Building Personal AI Systems](docs/personalization-beyond-platforms.md)
- [All Services & Tools Directory](docs/services.md)

---

## Resources

### Official Documentation
- [Anthropic: Context Engineering for Agents](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Model Context Protocol](https://modelcontextprotocol.io/)
- [OpenAI: Context Personalization](https://cookbook.openai.com/examples/agents_sdk/context_personalization)

### Community
- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers)
- [cursor.directory](https://cursor.directory/)
- [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/)
- [r/ChatGPT](https://www.reddit.com/r/ChatGPT/)

---

## Contributing

Contributions welcome! See [CONTRIBUTING.md](CONTRIBUTING.md).

---

## License

Apache License 2.0 - see [LICENSE](LICENSE).

[![License](https://img.shields.io/badge/License-Apache_2.0-blue.svg)](https://opensource.org/licenses/Apache-2.0)

---

**Star this repo if it helps you build a better AI experience!** ⭐
