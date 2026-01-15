# Personal Data & Preferences

> Teaching AI agents about your individual preferences, work style, and behavioral patterns.

## Overview

Personal data encompasses the unique information about you that helps AI agents provide tailored responses. This includes your communication style, technical preferences, domain expertise, and behavioral patterns. When AI understands these aspects, it can anticipate your needs and adapt its responses accordingly.

## Types of Personal Context

### 1. Communication Preferences
How you prefer to receive information:

| Preference | Examples |
|------------|----------|
| **Tone** | Formal, casual, technical, friendly |
| **Length** | Concise bullet points vs. detailed explanations |
| **Format** | Code-first, prose, tables, diagrams |
| **Language** | Native language, technical jargon level |

### 2. Technical Profile
Your technical background and preferences:

| Category | Examples |
|----------|----------|
| **Languages** | Python, TypeScript, Rust, Go |
| **Frameworks** | React, FastAPI, Django, Next.js |
| **Tools** | VSCode, Vim, Docker, Kubernetes |
| **Coding Style** | Functional, OOP, formatting preferences |

### 3. Domain Knowledge
Your areas of expertise:

| Category | Examples |
|----------|----------|
| **Industry** | Finance, healthcare, e-commerce |
| **Role** | Backend developer, data scientist, PM |
| **Specialization** | ML engineering, distributed systems |
| **Experience Level** | Junior, senior, lead, expert |

### 4. Work Context
Your current work environment:

| Category | Examples |
|----------|----------|
| **Company** | Size, industry, tech stack |
| **Team** | Structure, processes, tools |
| **Projects** | Current focus, goals, constraints |
| **Timezone** | Working hours, availability |

## Capturing Personal Preferences

### Method 1: Direct Declaration
Explicitly tell the AI about yourself:

```markdown
# My Preferences

## Technical
- Primary language: Python 3.11+
- Prefer type hints and docstrings
- Use black for formatting, ruff for linting
- Prefer functional programming patterns
- Always include error handling

## Communication
- Be concise, use bullet points
- Skip basic explanations (I'm a senior developer)
- Include code examples
- Explain trade-offs for design decisions

## Domain
- Working on fintech applications
- Strong focus on security and compliance
- Performance is critical (sub-100ms latency)
```

### Method 2: Context Interview
Use a structured conversation to extract preferences:

```markdown
# Context Interview Template

## Background
1. What is your current role and experience level?
2. What technologies do you work with daily?
3. What industry or domain do you work in?

## Preferences
4. How do you prefer explanations: detailed or concise?
5. Do you want code examples in responses?
6. Any specific coding conventions you follow?

## Current Work
7. What project are you currently focused on?
8. What are your main technical challenges?
9. Are there any constraints I should know about?
```

### Method 3: Behavioral Learning
AI learns from your interactions over time:

```
Observed Patterns:
├── Always asks for TypeScript examples → Prefers TS
├── Often requests shorter responses → Values conciseness
├── Frequently asks about performance → Cares about optimization
├── Uses functional patterns → Prefers functional style
└── Mentions AWS often → Uses AWS infrastructure
```

## Platform Implementations

### ChatGPT Memory
ChatGPT can learn and remember preferences:

**How to use:**
- "Remember that I prefer Python code examples"
- "Remember I'm a senior backend developer"
- "I always use TypeScript, remember that"

**Managing memory:**
- Settings → Personalization → Memory
- View all memories
- Delete specific memories
- Clear all memories

**Reference:** [ChatGPT Memory FAQ](https://help.openai.com/en/articles/8590148-memory-faq)

### Claude Custom Instructions
Use Claude's custom instructions feature:

**Example Custom Instructions:**
```markdown
## About Me
- Senior software engineer with 8+ years experience
- Expert in Python, TypeScript, and distributed systems
- Working at a fintech startup

## My Preferences
- Use type hints in Python
- Prefer functional programming patterns
- Give concise answers, skip basics
- Include error handling in code examples
- Consider security implications

## My Current Context
- Building a real-time payment processing system
- Using FastAPI, PostgreSQL, Redis
- Deploying on AWS with Kubernetes
```

**Reference:** [Claude Personalization](https://support.claude.com/en/articles/10185728-understanding-claude-s-personalization-features)

### Cursor Rules
Configure Cursor with `.cursor/rules` file:

```markdown
# .cursor/rules

## Code Style
- Use TypeScript for all new code
- Prefer functional components in React
- Use named exports over default exports
- Include JSDoc comments for public APIs

## Project Context
- This is a Next.js 14 app with App Router
- Using Prisma for database access
- Tailwind CSS for styling
- Deployed on Vercel

## My Preferences
- I prefer concise explanations
- Always suggest tests for new code
- Consider accessibility in UI components
```

## Structuring Personal Knowledge

### User Profile Schema
```json
{
  "profile": {
    "name": "Alex",
    "role": "Senior Software Engineer",
    "experience_years": 8,
    "timezone": "America/New_York"
  },
  "technical": {
    "primary_languages": ["Python", "TypeScript"],
    "frameworks": ["FastAPI", "React", "Next.js"],
    "tools": ["Docker", "Kubernetes", "AWS"],
    "coding_style": {
      "formatting": "black",
      "type_hints": true,
      "testing": "pytest"
    }
  },
  "preferences": {
    "response_style": "concise",
    "include_examples": true,
    "explain_tradeoffs": true,
    "skip_basics": true
  },
  "domain": {
    "industry": "fintech",
    "focus_areas": ["payments", "security", "performance"],
    "constraints": ["PCI compliance", "sub-100ms latency"]
  }
}
```

### Preference Categories

```
Personal Preferences
├── Communication
│   ├── Tone (formal/casual)
│   ├── Length (brief/detailed)
│   ├── Format (bullets/prose)
│   └── Examples (always/sometimes/never)
├── Technical
│   ├── Languages (Python, TS, etc.)
│   ├── Frameworks (React, FastAPI)
│   ├── Tools (IDE, CLI tools)
│   └── Style (conventions, formatting)
├── Learning Style
│   ├── Explanation depth
│   ├── Analogy usage
│   ├── Visual aids
│   └── Hands-on examples
└── Work Context
    ├── Current projects
    ├── Team practices
    ├── Constraints
    └── Goals
```

## Privacy Considerations

### What to Share
- **Safe**: Technical preferences, communication style, domain expertise
- **Caution**: Company-specific details, project names, internal tools
- **Avoid**: PII, credentials, confidential business information

### Privacy Best Practices
1. **Anonymize**: Remove identifying information when possible
2. **Generalize**: "fintech startup" vs. specific company name
3. **Review**: Periodically audit what preferences are stored
4. **Control**: Use platforms with memory management features
5. **Local First**: Prefer local storage for sensitive preferences

### Data Retention
- Understand each platform's data retention policy
- Know how to delete stored preferences
- Consider self-hosted options for sensitive contexts

## Tools for Preference Management

### Memory Services
| Service | Description | Link |
|---------|-------------|------|
| **ChatGPT Memory** | Built-in preference learning | [OpenAI](https://help.openai.com/en/articles/8590148-memory-faq) |
| **Mem0** | Personal AI memory layer | [GitHub](https://github.com/mem0ai/mem0) |
| **Zep** | Long-term memory for AI | [GitHub](https://github.com/getzep/zep) |

### Profile Management
| Tool | Description | Link |
|------|-------------|------|
| **Cursor Rules** | Per-project AI configuration | [Cursor Docs](https://docs.cursor.com/context/rules-for-ai) |
| **Claude Projects** | Persistent context & instructions | [Claude](https://support.claude.com/en/articles/9519177-how-can-i-create-and-manage-projects) |
| **Custom GPTs** | Persistent instructions for GPT | [OpenAI](https://help.openai.com/en/articles/8554397-creating-a-gpt) |

## Example: Complete Personal Context File

```markdown
# Personal AI Context

## About Me
I'm a senior software engineer with 8 years of experience, currently working at a fintech startup. I specialize in backend systems, particularly high-throughput payment processing and real-time data pipelines.

## Technical Profile
- **Primary Languages**: Python 3.11+, TypeScript
- **Backend**: FastAPI, SQLAlchemy, Celery
- **Frontend**: React, Next.js (when needed)
- **Infrastructure**: AWS (ECS, Lambda, RDS), Kubernetes, Terraform
- **Databases**: PostgreSQL, Redis, DynamoDB

## Coding Preferences
- Always use type hints in Python
- Prefer functional programming patterns
- Use dependency injection for testability
- Include comprehensive error handling
- Write docstrings for public APIs
- Format with black, lint with ruff

## Communication Preferences
- Be concise - I prefer bullet points over long paragraphs
- Skip basic explanations - assume senior-level knowledge
- Always include runnable code examples
- Explain trade-offs for architectural decisions
- Mention performance implications when relevant

## Current Context
- Building a real-time payment processing system
- Key constraints: PCI compliance, <100ms latency
- Team uses trunk-based development with short-lived branches
- Deploying to production multiple times per day

## Things to Remember
- I use macOS with iTerm2 and Neovim
- My test framework is pytest with pytest-asyncio
- I prefer explicit over implicit (Python Zen)
- Security is always a priority - mention vulnerabilities
```

## Further Reading

- [Building an AI Agent with Personal Context](https://www.reddit.com/r/LLMDevs/comments/1j6q49b/building_an_ai_agent_with_a_bank_of_personal/)
- [Context Personalization Guide](https://cookbook.openai.com/examples/agents_sdk/context_personalization)
- [AI That Adapts to Preferences](https://www.arsturn.com/blog/creating-an-ai-agent-that-adapts-to-user-preferences-over-time)

---

[← Back to README](../../README.md)
