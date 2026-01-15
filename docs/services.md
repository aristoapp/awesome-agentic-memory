# Services & Tools Directory

> Complete directory of services and tools for personalizing AI, organized by complexity level.

---

## Level 1: Built-in Platform Features

No additional tools needed—just configure settings in the platforms you already use.

| Platform | Feature | Description | Link |
|----------|---------|-------------|------|
| **ChatGPT** | Custom Instructions | Tell ChatGPT about yourself | [Guide](https://help.openai.com/en/articles/8096356-custom-instructions-for-chatgpt) |
| **ChatGPT** | Memory | ChatGPT learns from conversations | [Guide](https://help.openai.com/en/articles/8590148-memory-faq) |
| **Claude** | Custom Instructions | Set default preferences | [Guide](https://support.claude.com/en/articles/10185728-understanding-claude-s-personalization-features) |
| **Claude** | Projects | Upload files, set project context | [Guide](https://support.anthropic.com/en/articles/9519177-how-can-i-create-and-manage-projects) |
| **Gemini** | Saved Info | Store facts for Gemini to remember | [Guide](https://support.google.com/gemini) |
| **Gemini** | Extensions | Connect Gmail, Drive, Calendar | [Guide](https://support.google.com/gemini/answer/13695044) |
| **Gemini** | Gems | Create custom Gemini personas | [Guide](https://support.google.com/gemini/answer/14575153) |

---

## Level 2: Data Connection Tools

### MCP (Model Context Protocol)

Connect AI to your local files and apps.

| Server | Connects To | Install Command |
|--------|-------------|-----------------|
| **Filesystem** | Local files | `npx @modelcontextprotocol/server-filesystem` |
| **Memory** | Persistent memory | `npx @modelcontextprotocol/server-memory` |
| **Obsidian** | Obsidian vault | `npx mcp-obsidian` |
| **Notion** | Notion pages | `npx @modelcontextprotocol/server-notion` |
| **Google Drive** | Drive files | `npx @modelcontextprotocol/server-gdrive` |
| **Slack** | Slack messages | `npx @modelcontextprotocol/server-slack` |
| **GitHub** | Git repos | `npx @modelcontextprotocol/server-github` |
| **PostgreSQL** | Database | `npx @modelcontextprotocol/server-postgres` |

**Resources:**
- [MCP Documentation](https://modelcontextprotocol.io/)
- [Official Servers](https://github.com/modelcontextprotocol/servers)
- [Awesome MCP Servers](https://github.com/punkpeye/awesome-mcp-servers)

### Browser Extensions

| Extension | Features | Pricing | Link |
|-----------|----------|---------|------|
| **Sider** | AI sidebar, page context | Freemium | [sider.ai](https://sider.ai/) |
| **Monica** | Browser AI with memory | Freemium | [monica.im](https://monica.im/) |
| **Merlin** | ChatGPT on any site | Freemium | [getmerlin.in](https://getmerlin.in/) |
| **MaxAI** | Multi-model browser AI | Freemium | [maxai.me](https://www.maxai.me/) |

### Knowledge Management with AI

| Tool | Type | AI Features | Link |
|------|------|-------------|------|
| **Notion AI** | Cloud | Q&A over pages, writing assist | [notion.so](https://notion.so/) |
| **Mem** | Cloud | Auto-organization, AI search | [mem.ai](https://mem.ai/) |
| **Reflect** | Cloud | Built-in AI assistant | [reflect.app](https://reflect.app/) |
| **Obsidian + Copilot** | Local | Plugin-based AI | [obsidian.md](https://obsidian.md/) |
| **Logseq** | Local | AI plugins available | [logseq.com](https://logseq.com/) |

---

## Level 3: Memory & Context Services

### Cloud Memory Services

Services that provide persistent memory across conversations.

| Service | What It Does | Best For | Link |
|---------|--------------|----------|------|
| **ChatGPT Memory** | Auto-learns from chats | ChatGPT users | Built-in |
| **Rewind** | Records screen, searchable by AI | Power users, macOS | [rewind.ai](https://rewind.ai/) |
| **Khoj Cloud** | Personal AI with document access | All-in-one | [khoj.dev](https://khoj.dev/) |
| **Personal AI** | AI that learns from your data | Personal assistant | [personal.ai](https://personal.ai/) |
| **Granola** | AI notepad for meetings | Meeting notes | [granola.ai](https://granola.ai/) |

### Self-Hosted Personal AI

Run your own AI assistant with memory.

| Tool | Features | Setup | Link |
|------|----------|-------|------|
| **Open WebUI** | Memory, RAG, multi-model | Docker | [GitHub](https://github.com/open-webui/open-webui) |
| **Khoj** | Connect notes, docs, web search | Docker/pip | [GitHub](https://github.com/khoj-ai/khoj) |
| **AnythingLLM** | Document chat, multi-user | Docker/Desktop | [GitHub](https://github.com/Mintplex-Labs/anything-llm) |
| **LibreChat** | Multi-provider, plugins | Docker | [GitHub](https://github.com/danny-avila/LibreChat) |
| **PrivateGPT** | Fully offline document chat | Local | [GitHub](https://github.com/imartinez/privateGPT) |

### Vector Databases

Store and search your documents semantically.

**Cloud Options:**

| Service | Free Tier | Best For | Link |
|---------|-----------|----------|------|
| **Pinecone** | 100K vectors | Production, managed | [pinecone.io](https://pinecone.io/) |
| **Supabase Vector** | Yes | Postgres users | [supabase.com](https://supabase.com/) |
| **Upstash Vector** | Yes | Serverless | [upstash.com](https://upstash.com/) |
| **Weaviate Cloud** | Limited | Hybrid search | [weaviate.io](https://weaviate.io/) |

**Self-Hosted Options:**

| Database | Difficulty | Best For | Link |
|----------|------------|----------|------|
| **Chroma** | Easy | Development, prototyping | [GitHub](https://github.com/chroma-core/chroma) |
| **LanceDB** | Easy | Embedded/desktop apps | [GitHub](https://github.com/lancedb/lancedb) |
| **Qdrant** | Medium | Production performance | [GitHub](https://github.com/qdrant/qdrant) |
| **Weaviate** | Medium | Hybrid search | [GitHub](https://github.com/weaviate/weaviate) |
| **pgvector** | Easy | Existing Postgres | [GitHub](https://github.com/pgvector/pgvector) |

---

## Level 4: Full Stack Components

### Local LLM Inference

Run AI models on your own hardware.

**Desktop Apps:**

| Tool | Difficulty | Features | Link |
|------|------------|----------|------|
| **Ollama** | Easy | CLI, many models | [ollama.com](https://ollama.com/) |
| **LM Studio** | Easy | GUI, model browser | [lmstudio.ai](https://lmstudio.ai/) |
| **Jan** | Easy | ChatGPT-like UI | [jan.ai](https://jan.ai/) |
| **GPT4All** | Easy | Offline, document chat | [gpt4all.io](https://gpt4all.io/) |
| **Msty** | Easy | RAG built-in | [msty.app](https://msty.app/) |

**Server/Self-Hosted:**

| Tool | Difficulty | Best For | Link |
|------|------------|----------|------|
| **Ollama** | Easy | Development | [ollama.com](https://ollama.com/) |
| **LocalAI** | Medium | OpenAI API replacement | [GitHub](https://github.com/mudler/LocalAI) |
| **vLLM** | Hard | High throughput | [GitHub](https://github.com/vllm-project/vllm) |
| **Text Gen WebUI** | Medium | Experimentation | [GitHub](https://github.com/oobabooga/text-generation-webui) |

### Document Processing

Extract text from files for AI consumption.

| Tool | Type | Formats | Link |
|------|------|---------|------|
| **Unstructured** | OSS | 25+ formats | [GitHub](https://github.com/Unstructured-IO/unstructured) |
| **LlamaParse** | Cloud | Complex PDFs | [llamaindex.ai](https://llamaindex.ai/) |
| **Marker** | OSS | PDF to Markdown | [GitHub](https://github.com/VikParuchuri/marker) |
| **Docling** | OSS | PDF, Office | [GitHub](https://github.com/DS4SD/docling) |

### Embeddings

Convert text to vectors for semantic search.

**Cloud:**

| Service | Quality | Link |
|---------|---------|------|
| **OpenAI** | High | [openai.com](https://platform.openai.com/docs/guides/embeddings) |
| **Cohere** | High | [cohere.com](https://cohere.com/) |
| **Voyage AI** | High | [voyageai.com](https://voyageai.com/) |

**Local:**

| Tool | Speed | Link |
|------|-------|------|
| **Ollama** | Fast | Built-in with `nomic-embed-text` |
| **Sentence Transformers** | Medium | [GitHub](https://github.com/UKPLab/sentence-transformers) |
| **FastEmbed** | Fast | [GitHub](https://github.com/qdrant/fastembed) |

### Sync & Automation

Keep your AI's knowledge updated.

**Cloud:**

| Service | Features | Link |
|---------|----------|------|
| **Zapier** | 6000+ app integrations | [zapier.com](https://zapier.com/) |
| **Make** | Visual workflows | [make.com](https://make.com/) |
| **Pipedream** | Developer-friendly | [pipedream.com](https://pipedream.com/) |

**Self-Hosted:**

| Tool | Difficulty | Link |
|------|------------|------|
| **n8n** | Medium | [GitHub](https://github.com/n8n-io/n8n) |
| **Activepieces** | Easy | [GitHub](https://github.com/activepieces/activepieces) |

---

## Quick Start Recommendations

### "I just want better AI responses" (5 min)
1. Set up **ChatGPT Custom Instructions** or **Claude Projects**
2. Tell AI about yourself, your role, your preferences

### "I want AI to know my documents" (30 min)
1. Use **Claude Projects** to upload key files
2. Or install **MCP filesystem server** for Claude Desktop

### "I want a private AI assistant" (1-2 hours)
1. Install **Ollama** + **Open WebUI**
2. Upload documents for RAG
3. Chat with your data privately

### "I want the full experience" (1 day+)
1. Set up the **full Docker stack** (Ollama + Open WebUI + Chroma + n8n)
2. Configure **MCP servers** for live data access
3. Build **automated sync** for your documents
4. Create **custom knowledge bases** from your data

---

## Cloud vs Self-Hosted Comparison

| Aspect | Cloud Services | Self-Hosted |
|--------|---------------|-------------|
| **Setup time** | Instant | Hours to days |
| **Technical skill** | None | Medium to high |
| **Privacy** | Data on provider servers | Complete control |
| **Cost** | Subscription | Hardware + electricity |
| **Model quality** | GPT-4, Claude 3.5 | Llama 3.2, Mistral |
| **Maintenance** | Provider handles | You handle |
| **Customization** | Limited | Unlimited |

---

[← Back to README](../README.md)
