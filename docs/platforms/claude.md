# Claude Personalization Guide

> Customizing Claude (Anthropic) to understand your preferences, work context, and communication style.

## Overview

[Claude](https://claude.ai/) by Anthropic offers several personalization features that allow you to create persistent context, define communication preferences, and organize work into projects. This guide covers all available customization options.

## Personalization Methods

### 1. Custom Instructions (Styles)

Custom instructions let you define how Claude responds to you across all conversations.

#### Accessing Custom Instructions
1. Click your profile icon → Settings
2. Navigate to "Custom Instructions" or "Styles"
3. Add your preferences

#### Example Custom Instructions

```markdown
## About Me
- Senior software engineer with 10+ years experience
- Expert in Python, TypeScript, and distributed systems
- Currently leading a backend team at a fintech company

## Communication Style
- Be direct and concise
- Skip basic explanations unless I ask
- Use technical terminology freely
- Include code examples when discussing programming

## Preferences
- Default to Python 3.11+ syntax with type hints
- Use async/await for I/O operations
- Prefer functional programming patterns
- Always consider error handling and edge cases

## Format
- Use bullet points for lists
- Include code blocks with syntax highlighting
- Structure longer responses with clear headings
- Provide sources/links when making factual claims
```

### 2. Claude Projects

Projects let you create dedicated workspaces with persistent context and custom instructions.

#### Creating a Project
1. Go to Claude.ai sidebar
2. Click "+ New Project"
3. Name your project
4. Add project knowledge and instructions

#### Project Knowledge
Upload files that Claude can reference throughout the project:

**Supported file types:**
- PDF documents
- Text files (.txt, .md)
- Code files (.py, .ts, .js, etc.)
- CSV data files

**Token limit:** Up to 200,000 tokens per project

#### Project Instructions
Set project-specific instructions:

```markdown
# Project: Payment Processing System

## Context
You are helping me build a high-throughput payment processing system.

## Technical Stack
- Language: Python 3.11
- Framework: FastAPI
- Database: PostgreSQL with SQLAlchemy
- Message Queue: Redis + Celery
- Infrastructure: AWS ECS

## Requirements
- PCI DSS compliance is mandatory
- Target latency: <100ms p99
- Must handle 10,000 TPS

## Code Conventions
- Use type hints everywhere
- Follow Google Python Style Guide
- Include docstrings for public APIs
- Write async code for I/O operations

## When Generating Code
- Always include error handling
- Add logging for important operations
- Consider idempotency for transactions
- Include unit test suggestions
```

#### Best Uses for Projects
- **Documentation projects**: Upload technical docs, reference materials
- **Codebase assistance**: Add key source files for code understanding
- **Research projects**: Upload papers, articles for synthesis
- **Writing projects**: Add style guides, previous writing samples

### 3. Artifacts

Claude can create and iterate on substantial content in Artifacts:

**Artifact types:**
- Code files (any language)
- Documents (Markdown)
- HTML/CSS/JS applications
- React components
- SVG graphics
- Mermaid diagrams

**Best practices:**
- Ask Claude to create artifacts for code you want to save
- Request iterations on existing artifacts
- Export artifacts for use in your projects

### 4. Memory (Claude's Memory Feature)

Claude can remember information across conversations:

#### How to Use Memory
- "Remember that I prefer Python over JavaScript"
- "Keep in mind I work at a healthcare company"
- "Remember my project deadline is March 15th"

#### Managing Memories
1. Settings → Memory
2. View all stored memories
3. Delete individual memories or clear all

#### Memory Best Practices
- Store facts, not temporary context
- Be explicit about what to remember
- Review memories periodically
- Don't store sensitive information

### 5. Conversation Starters

Begin conversations with context:

```
I'm working on [specific project]. Here's the context:
- Tech stack: [your stack]
- Current challenge: [what you're solving]
- Constraints: [any limitations]

[Your specific question]
```

**Example:**
```
I'm building a real-time notification system. Here's the context:
- Tech stack: Node.js, WebSocket, Redis
- Current challenge: Handling reconnection and message delivery guarantees
- Constraints: Must work on mobile with unstable connections

How should I implement reliable message delivery with acknowledgments?
```

## Configuration Templates

### Developer Profile

```markdown
## Technical Background
- 8 years of software engineering experience
- Specialize in backend systems and APIs
- Languages: Python (primary), TypeScript, Go
- Focus areas: Performance, scalability, security

## Current Context
- Building microservices for e-commerce platform
- Using FastAPI, PostgreSQL, Redis, Kubernetes
- Team follows trunk-based development

## Code Preferences
- Type hints in all Python code
- Pydantic for data validation
- pytest for testing
- Black + ruff for formatting/linting

## Communication
- Skip beginner explanations
- Be direct about trade-offs
- Include runnable code examples
- Mention performance implications
```

### Researcher Profile

```markdown
## Background
- PhD candidate in machine learning
- Focus: Natural language processing
- 5 years of research experience

## Communication Preferences
- Cite relevant papers when discussing techniques
- Use mathematical notation when appropriate
- Be precise about assumptions and limitations
- Distinguish between established results and conjectures

## Format
- LaTeX for equations when helpful
- Include references to foundational work
- Summarize key contributions of papers
- Highlight experimental methodology
```

### Writer/Content Creator Profile

```markdown
## Background
- Technical writer and content creator
- Focus: Developer documentation and tutorials
- Writing for intermediate-level developers

## Style Preferences
- Clear, accessible language
- Avoid jargon without explanation
- Use progressive disclosure (simple → complex)
- Include practical examples

## Format
- Short paragraphs (2-3 sentences)
- Use code examples liberally
- Include "Try it yourself" sections
- End with actionable takeaways
```

## API Personalization

When using Claude via API, include system prompts:

```python
import anthropic

client = anthropic.Anthropic()

system_prompt = """You are an expert Python developer assistant.

Guidelines:
- Always use Python 3.11+ features
- Include type hints in all code
- Prefer functional patterns
- Include error handling
- Write docstrings for functions

User context:
- Senior developer, skip basics
- Working on FastAPI microservices
- Prioritize performance and security
"""

message = client.messages.create(
    model="claude-sonnet-4-20250514",
    max_tokens=1024,
    system=system_prompt,
    messages=[
        {"role": "user", "content": "How should I implement rate limiting?"}
    ]
)
```

## Best Practices

### Custom Instructions
1. **Be specific**: Vague instructions lead to generic responses
2. **Prioritize**: Put most important preferences first
3. **Update regularly**: Preferences change over time
4. **Test and iterate**: Refine based on response quality

### Projects
1. **Organize by topic**: One project per major area of work
2. **Curate knowledge**: Quality over quantity in uploads
3. **Keep instructions focused**: Project-specific, not generic
4. **Update context**: Refresh as projects evolve

### General
1. **Start conversations with context**: Help Claude understand the situation
2. **Be explicit about format**: Request specific output structures
3. **Use follow-ups**: Refine responses iteratively
4. **Provide feedback**: Tell Claude what worked or didn't

## Limitations

### What Claude Can't Do
- Access external websites in real-time (without tools)
- Remember across separate conversations (unless using Memory)
- Execute code (unless using Artifacts/Analysis)
- Access your files outside of Projects

### Token Limits
- Projects: ~200,000 tokens of knowledge
- Conversations: Context window varies by model
- Memory: Reasonable amount of facts (not unlimited)

## Resources

### Official Documentation
- [Claude Projects Guide](https://support.anthropic.com/en/articles/9519177-how-can-i-create-and-manage-projects)
- [Custom Instructions](https://support.claude.com/en/articles/10185728-understanding-claude-s-personalization-features)
- [Claude API Documentation](https://docs.anthropic.com/)

### Community Resources
- [r/ClaudeAI](https://www.reddit.com/r/ClaudeAI/) - Reddit community
- [Anthropic Discord](https://discord.gg/anthropic)
- [Prompt Library](https://docs.anthropic.com/en/prompt-library/library)

### Best Practices
- [Effective Context Engineering](https://www.anthropic.com/engineering/effective-context-engineering-for-ai-agents)
- [Claude Model Card](https://www.anthropic.com/model-card)

---

[← Back to README](../../README.md)
