# External Application Data

> Connecting AI agents to your productivity tools, communication platforms, and data sources.

## Overview

Modern AI agents become significantly more powerful when connected to the applications where your actual work happens. By integrating with tools like Notion, Slack, Google Workspace, and others, AI can access real-time information, understand your workflows, and take actions on your behalf.

## Categories of External Apps

### Productivity & Knowledge Management
| App | Data Types | Use Cases |
|-----|------------|-----------|
| **Notion** | Pages, databases, wikis | Personal knowledge base, project docs |
| **Obsidian** | Markdown notes, links | Personal PKM, research notes |
| **Evernote** | Notes, web clips | Personal notes, bookmarks |
| **Roam Research** | Networked notes | Research, daily notes |
| **Coda** | Docs, tables | Team documentation |

### Communication & Collaboration
| App | Data Types | Use Cases |
|-----|------------|-----------|
| **Slack** | Messages, channels, threads | Team communication history |
| **Discord** | Messages, servers | Community discussions |
| **Microsoft Teams** | Chats, meetings, files | Enterprise communication |
| **Email (Gmail/Outlook)** | Emails, contacts, calendar | Correspondence, scheduling |

### Project Management
| App | Data Types | Use Cases |
|-----|------------|-----------|
| **Linear** | Issues, projects, roadmaps | Development tracking |
| **Jira** | Tickets, sprints, boards | Enterprise project management |
| **Asana** | Tasks, projects, timelines | Team task management |
| **Trello** | Cards, boards, lists | Visual project tracking |
| **GitHub/GitLab** | Issues, PRs, code | Development workflow |

### Cloud Storage
| App | Data Types | Use Cases |
|-----|------------|-----------|
| **Google Drive** | Docs, sheets, files | Cloud document storage |
| **Dropbox** | Files, folders | File synchronization |
| **OneDrive** | Office docs, files | Microsoft ecosystem storage |
| **Box** | Enterprise files | Secure file sharing |

### CRM & Business Tools
| App | Data Types | Use Cases |
|-----|------------|-----------|
| **Salesforce** | Contacts, deals, accounts | Sales data |
| **HubSpot** | CRM data, marketing | Customer information |
| **Intercom** | Conversations, users | Customer support |
| **Zendesk** | Tickets, knowledge base | Support history |

## Integration Methods

### 1. Model Context Protocol (MCP)
The emerging standard for connecting LLMs to external data:

```json
{
  "mcpServers": {
    "notion": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-notion"],
      "env": {
        "NOTION_API_KEY": "your-api-key"
      }
    },
    "slack": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-slack"],
      "env": {
        "SLACK_BOT_TOKEN": "xoxb-your-token"
      }
    }
  }
}
```

**Available MCP Servers:**
- [Official MCP Servers](https://github.com/modelcontextprotocol/servers) - Reference implementations
- [Notion MCP](https://github.com/modelcontextprotocol/servers/tree/main/src/notion) - Notion integration
- [Slack MCP](https://github.com/modelcontextprotocol/servers/tree/main/src/slack) - Slack integration
- [Google Drive MCP](https://github.com/modelcontextprotocol/servers/tree/main/src/gdrive) - Drive integration
- [GitHub MCP](https://github.com/modelcontextprotocol/servers/tree/main/src/github) - Repository access

### 2. Direct API Integration
Build custom integrations using official APIs:

```python
# Example: Fetching Notion pages
from notion_client import Client

notion = Client(auth="your-integration-token")

# Search all pages
results = notion.search(
    query="project planning",
    filter={"property": "object", "value": "page"}
)

# Get page content
page = notion.blocks.children.list(block_id="page-id")
```

### 3. Zapier/Make Integration
No-code automation connecting apps to AI:
- **Zapier AI Actions**: Connect 6000+ apps to AI workflows
- **Make (Integromat)**: Visual automation builder
- **n8n**: Self-hosted automation platform

### 4. LangChain Integrations
Pre-built document loaders and tools:

```python
from langchain_community.document_loaders import NotionDBLoader
from langchain_community.tools import SlackGetChannel

# Load Notion database
loader = NotionDBLoader(
    integration_token="secret_...",
    database_id="...",
)
docs = loader.load()

# Slack tool
slack_tool = SlackGetChannel()
```

## Platform-Specific Setup

### Notion Integration
1. **Create Integration**: Go to [Notion Developers](https://www.notion.so/my-integrations)
2. **Get API Key**: Copy the Internal Integration Token
3. **Share Pages**: Share specific pages with your integration
4. **Use via MCP or API**: Connect to your AI agent

**Resources:**
- [Notion API Documentation](https://developers.notion.com/)
- [Notion MCP Server](https://github.com/modelcontextprotocol/servers/tree/main/src/notion)

### Slack Integration
1. **Create Slack App**: Visit [Slack API](https://api.slack.com/apps)
2. **Add Bot Scopes**: `channels:history`, `channels:read`, `chat:write`
3. **Install to Workspace**: Get Bot User OAuth Token
4. **Configure MCP**: Add to your MCP configuration

**Resources:**
- [Slack API Documentation](https://api.slack.com/docs)
- [Slack MCP Server](https://github.com/modelcontextprotocol/servers/tree/main/src/slack)

### Google Workspace Integration
1. **Create GCP Project**: Go to [Google Cloud Console](https://console.cloud.google.com/)
2. **Enable APIs**: Drive API, Gmail API, Calendar API
3. **Create Credentials**: OAuth 2.0 or Service Account
4. **Authenticate**: Complete OAuth flow

**Resources:**
- [Google Drive API](https://developers.google.com/drive)
- [Gmail API](https://developers.google.com/gmail/api)
- [Google Drive MCP](https://github.com/modelcontextprotocol/servers/tree/main/src/gdrive)

### GitHub Integration
1. **Create Personal Access Token**: Settings → Developer settings → PAT
2. **Select Scopes**: `repo`, `read:org` as needed
3. **Configure MCP**: Add GitHub server configuration

**Resources:**
- [GitHub REST API](https://docs.github.com/en/rest)
- [GitHub MCP Server](https://github.com/modelcontextprotocol/servers/tree/main/src/github)

## Best Practices

### Data Access
1. **Principle of Least Privilege**: Only request necessary permissions
2. **Selective Sync**: Don't pull entire databases, filter relevantly
3. **Caching**: Cache frequently accessed data to reduce API calls
4. **Rate Limiting**: Respect API limits, implement backoff

### Privacy & Security
1. **Credential Management**: Use environment variables, never hardcode
2. **Data Minimization**: Only fetch what you need
3. **Audit Access**: Regularly review what's connected
4. **Token Rotation**: Periodically rotate API keys

### Performance
1. **Incremental Sync**: Only fetch changes since last sync
2. **Batch Requests**: Combine multiple requests when possible
3. **Async Operations**: Don't block on slow API calls
4. **Smart Retrieval**: Use search/filter APIs vs fetching everything

## Use Cases

### Personal Knowledge Assistant
```
User Query: "What did we decide about the Q4 roadmap?"

AI Agent Flow:
1. Search Notion for "Q4 roadmap" pages
2. Check Slack #product channel for related discussions
3. Review Linear project for timeline
4. Synthesize and respond with citations
```

### Email Summarization
```
User Query: "Summarize important emails from this week"

AI Agent Flow:
1. Connect to Gmail API
2. Filter emails from past 7 days
3. Identify high-priority senders
4. Extract key action items
5. Present organized summary
```

### Project Status Update
```
User Query: "Give me a status update on Project X"

AI Agent Flow:
1. Pull Linear issues for Project X
2. Check recent GitHub PRs and commits
3. Review Slack discussions in project channel
4. Aggregate into cohesive status report
```

## Tools & Services

### Integration Platforms
| Platform | Description | Link |
|----------|-------------|------|
| **Composio** | 150+ tool integrations for AI agents | [Website](https://composio.dev/) |
| **Zapier** | Connect 6000+ apps with AI | [Website](https://zapier.com/) |
| **Make** | Visual automation platform | [Website](https://www.make.com/) |
| **n8n** | Self-hosted workflow automation | [GitHub](https://github.com/n8n-io/n8n) |

### AI-Native Tools
| Tool | Description | Link |
|------|-------------|------|
| **Dust** | Connect company data to AI | [Website](https://dust.tt/) |
| **Glean** | Enterprise AI search | [Website](https://www.glean.com/) |
| **Kapa.ai** | AI for developer docs | [Website](https://www.kapa.ai/) |

## Security Considerations

### OAuth vs API Keys
- **OAuth**: Better for user-specific access, revocable
- **API Keys**: Simpler but less granular control

### Data Handling
- Store tokens securely (environment variables, secret managers)
- Encrypt sensitive data at rest
- Implement proper session management
- Log access for audit purposes

### Compliance
- Review data processing agreements
- Ensure GDPR/CCPA compliance for personal data
- Check enterprise security requirements

---

[← Back to README](../../README.md)
