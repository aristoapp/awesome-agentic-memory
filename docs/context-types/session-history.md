# Session & Conversation History

> Leveraging past interactions to create coherent, personalized AI conversations.

## Overview

Session history refers to the record of previous interactions between a user and an AI agent within a conversation or across sessions. Effective management of conversation history is crucial for maintaining context, ensuring coherent responses, and providing personalized experiences.

## Types of Conversation Memory

### 1. Short-Term Memory (Within Session)
- **Buffer Memory**: Stores recent messages in the current conversation
- **Window Memory**: Keeps the last N messages or tokens
- **Summary Memory**: Maintains a running summary of the conversation

### 2. Long-Term Memory (Across Sessions)
- **Episodic Memory**: Records specific past interactions
- **Semantic Memory**: Extracts and stores facts/preferences
- **Procedural Memory**: Learned patterns and behaviors

## Memory Architectures

```
Conversation Memory Types
├── Buffer Memory
│   └── Stores all messages (simple but limited)
├── Window Memory
│   └── Last K messages (efficient but loses context)
├── Summary Memory
│   └── Compressed summaries (efficient but lossy)
├── Entity Memory
│   └── Tracks entities mentioned (good for relationships)
└── Knowledge Graph Memory
    └── Structured relationships (complex but powerful)
```

## Implementation Strategies

### 1. Sliding Window
Keep the most recent N messages:

```python
class SlidingWindowMemory:
    def __init__(self, window_size=10):
        self.window_size = window_size
        self.messages = []
    
    def add_message(self, role, content):
        self.messages.append({"role": role, "content": content})
        if len(self.messages) > self.window_size:
            self.messages.pop(0)
    
    def get_context(self):
        return self.messages
```

### 2. Token-Based Truncation
Manage context by token count:

```python
import tiktoken

def truncate_by_tokens(messages, max_tokens=4000):
    encoder = tiktoken.get_encoding("cl100k_base")
    truncated = []
    total_tokens = 0
    
    # Start from most recent
    for msg in reversed(messages):
        msg_tokens = len(encoder.encode(msg["content"]))
        if total_tokens + msg_tokens <= max_tokens:
            truncated.insert(0, msg)
            total_tokens += msg_tokens
        else:
            break
    
    return truncated
```

### 3. Summarization
Compress older context:

```python
def summarize_conversation(messages, llm):
    """Summarize older messages to save context space."""
    old_messages = messages[:-5]  # Keep last 5 intact
    recent_messages = messages[-5:]
    
    if old_messages:
        summary = llm.summarize(old_messages)
        return [{"role": "system", "content": f"Previous context: {summary}"}] + recent_messages
    return messages
```

## Platform Implementations

### ChatGPT Memory
- **Automatic Memory**: ChatGPT learns from conversations automatically
- **Manual Memory**: Users can tell ChatGPT to remember specific facts
- **Memory Management**: View and delete stored memories in settings
- [ChatGPT Memory Guide](https://help.openai.com/en/articles/8590148-memory-faq)

### Claude
- **Project Context**: Persistent instructions across conversations
- **Conversation History**: Maintained within single conversation
- **No Cross-Session Memory**: Each new chat starts fresh (by design)
- [Claude Projects](https://support.anthropic.com/en/articles/9519177-how-can-i-create-and-manage-projects)

### Cursor
- **Session Context**: Maintains context within composer/chat sessions
- **Codebase Awareness**: Indexes and remembers project structure
- **@-mentions**: Reference previous context explicitly
- [Cursor Chat Features](https://docs.cursor.com/chat/)

## Memory Frameworks & Tools

### Open Source
| Framework | Description | Link |
|-----------|-------------|------|
| **MemGPT/Letta** | OS-like memory management for LLMs | [GitHub](https://github.com/cpacker/MemGPT) |
| **Zep** | Long-term memory store for AI apps | [GitHub](https://github.com/getzep/zep) |
| **Mem0** | Memory layer for AI applications | [GitHub](https://github.com/mem0ai/mem0) |
| **LangChain Memory** | Multiple memory implementations | [Docs](https://python.langchain.com/docs/modules/memory/) |

### Commercial Services
| Service | Description | Link |
|---------|-------------|------|
| **Zep Cloud** | Managed memory infrastructure | [Website](https://www.getzep.com/) |
| **Pinecone** | Vector DB for conversation memory | [Website](https://www.pinecone.io/) |
| **MongoDB Atlas** | Document store with vector search | [Website](https://www.mongodb.com/atlas) |

## Best Practices

### Context Window Management
1. **Prioritize recent messages**: Most relevant for current task
2. **Preserve system prompts**: Always include instructions
3. **Summarize strategically**: Compress older context, not recent
4. **Track token usage**: Monitor and optimize context size

### Memory Quality
1. **Filter noise**: Don't store every message equally
2. **Extract key facts**: Store structured information
3. **Update over time**: Refresh stale information
4. **Handle contradictions**: More recent info usually wins

### User Experience
1. **Transparency**: Let users know what's remembered
2. **Control**: Allow users to view/edit/delete memories
3. **Privacy**: Be explicit about data retention
4. **Graceful degradation**: Handle missing context well

## Advanced Techniques

### Hierarchical Memory
```
Long-Term Storage (Persistent)
├── User Profile (preferences, facts)
├── Key Decisions (important choices made)
└── Project Context (ongoing work)

Mid-Term Storage (Session-Level)
├── Current Goals
├── Active Topics
└── Recent Findings

Short-Term Storage (Immediate)
├── Last few messages
├── Current task context
└── Immediate references
```

### Memory Retrieval Patterns
1. **Recency-Based**: Weight recent memories higher
2. **Relevance-Based**: Use semantic similarity
3. **Importance-Based**: Score and prioritize memories
4. **Hybrid**: Combine multiple signals

## Example: Building Conversation Memory

```python
from datetime import datetime
from typing import List, Dict
import numpy as np

class ConversationMemory:
    def __init__(self, embedding_model):
        self.messages: List[Dict] = []
        self.summaries: List[str] = []
        self.embeddings = []
        self.embed_model = embedding_model
    
    def add_turn(self, user_msg: str, assistant_msg: str):
        turn = {
            "user": user_msg,
            "assistant": assistant_msg,
            "timestamp": datetime.now().isoformat()
        }
        self.messages.append(turn)
        
        # Create embedding for retrieval
        combined = f"User: {user_msg}\nAssistant: {assistant_msg}"
        embedding = self.embed_model.embed(combined)
        self.embeddings.append(embedding)
    
    def get_relevant_context(self, query: str, top_k: int = 3) -> List[Dict]:
        query_embedding = self.embed_model.embed(query)
        
        # Calculate similarities
        similarities = [
            np.dot(query_embedding, emb) 
            for emb in self.embeddings
        ]
        
        # Get top-k most relevant
        top_indices = np.argsort(similarities)[-top_k:]
        return [self.messages[i] for i in top_indices]
    
    def get_recent_context(self, n: int = 5) -> List[Dict]:
        return self.messages[-n:]
```

## Research & Further Reading

- [Memory in Agents](https://www.philschmid.de/memory-in-agents) - Engineering long-term memory
- [MemGPT Paper](https://arxiv.org/abs/2310.08560) - Virtual Context Management
- [Conversational Memory Survey](https://arxiv.org/abs/2401.02777) - Academic overview
- [LangChain Memory Guide](https://python.langchain.com/docs/how_to/chatbots_memory/) - Practical implementation

---

[← Back to README](../../README.md)
