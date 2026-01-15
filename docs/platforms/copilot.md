# GitHub Copilot Personalization Guide

> Customizing GitHub Copilot and VS Code's AI features to match your coding style and project requirements.

## Overview

[GitHub Copilot](https://github.com/features/copilot) is an AI pair programmer that integrates with VS Code, JetBrains IDEs, and other editors. While it learns from context automatically, there are several ways to guide its behavior for better, more personalized suggestions.

## Personalization Methods

### 1. Repository-Level Instructions

Create instruction files that Copilot can reference.

#### `.github/copilot-instructions.md`

```markdown
# Copilot Instructions for This Repository

## Project Overview
This is a Next.js 14 e-commerce application using App Router.

## Technology Stack
- Framework: Next.js 14 with App Router
- Language: TypeScript (strict mode)
- Styling: Tailwind CSS + shadcn/ui
- Database: PostgreSQL with Prisma
- Auth: NextAuth.js v5
- State: Zustand for client state

## Code Conventions

### TypeScript
- Always use strict TypeScript
- Prefer `unknown` over `any`
- Use explicit return types for functions
- Use discriminated unions for state

### React
- Prefer Server Components by default
- Use 'use client' only when necessary
- Extract complex logic into custom hooks
- Use React.forwardRef for component refs

### Naming
- Files: kebab-case (user-profile.tsx)
- Components: PascalCase (UserProfile)
- Functions: camelCase (getUserProfile)
- Types: PascalCase (UserProfileProps)
- Constants: SCREAMING_SNAKE_CASE

### File Structure
```
/app
  /(auth)       - Auth routes
  /(dashboard)  - Dashboard routes
/components
  /ui           - shadcn/ui components
  /features     - Feature components
/lib
  /utils        - Utility functions
  /hooks        - Custom hooks
```

## Patterns to Follow

### API Routes
```typescript
// Always use this pattern for API routes
export async function POST(request: Request) {
  try {
    const body = await request.json();
    const validated = schema.parse(body);
    // ... implementation
    return Response.json({ data });
  } catch (error) {
    if (error instanceof ZodError) {
      return Response.json({ error: 'Validation failed' }, { status: 400 });
    }
    return Response.json({ error: 'Internal error' }, { status: 500 });
  }
}
```

### Components
```typescript
// Always use this pattern for components
interface Props {
  // props
}

export function ComponentName({ prop1, prop2 }: Props) {
  // implementation
}
```

## Things to Avoid
- Don't use `any` type
- Don't use default exports for components
- Don't mix CSS-in-JS with Tailwind
- Don't use class components
```

### 2. VS Code Settings

Configure Copilot behavior in VS Code settings:

```json
{
  // Copilot settings
  "github.copilot.enable": {
    "*": true,
    "plaintext": false,
    "markdown": true,
    "yaml": true
  },
  
  // Language-specific settings
  "github.copilot.advanced": {
    "length": 500,
    "temperature": 0.1
  },
  
  // Editor settings that affect Copilot
  "editor.inlineSuggest.enabled": true,
  "editor.suggest.preview": true
}
```

### 3. VS Code Copilot Chat

Use Copilot Chat with context-aware prompts:

#### Workspace Context
```
@workspace What patterns do we use for API routes?
@workspace How is authentication implemented?
```

#### File Context
```
#file:src/lib/auth.ts Explain how this auth system works
#file:package.json What testing framework do we use?
```

#### Selection Context
```
# Select code, then:
/explain Explain this code
/fix Fix any issues
/tests Generate tests
```

### 4. Custom Instructions in Copilot Chat

Set persistent instructions in VS Code settings:

```json
{
  "github.copilot.chat.codeGeneration.instructions": [
    {
      "text": "Always use TypeScript with strict mode enabled."
    },
    {
      "text": "Use async/await instead of promises."
    },
    {
      "text": "Include JSDoc comments for exported functions."
    },
    {
      "text": "Prefer functional programming patterns."
    }
  ]
}
```

### 5. Code Comments for Guidance

Guide Copilot with strategic comments:

```typescript
// Generate a React component that:
// - Uses TypeScript with Props interface
// - Implements loading and error states
// - Uses Tailwind for styling
// - Includes accessibility attributes

// Copilot will generate based on these instructions
```

```python
# Create a FastAPI endpoint that:
# - Validates input with Pydantic
# - Handles errors gracefully
# - Returns consistent JSON responses
# - Includes type hints

# Copilot generates contextual code
```

### 6. Prompt Files

Create `.prompt` files for complex generations:

```markdown
<!-- .github/prompts/api-endpoint.prompt -->

Generate a FastAPI endpoint with:
- Pydantic input validation
- SQLAlchemy database operations
- Proper error handling
- OpenAPI documentation
- Async/await patterns

Example structure:
```python
@router.post("/resource")
async def create_resource(
    data: ResourceCreate,
    db: AsyncSession = Depends(get_db)
) -> ResourceResponse:
    ...
```
```

## VS Code Copilot Features

### Inline Suggestions
- `Tab` - Accept suggestion
- `Esc` - Dismiss suggestion
- `Alt + ]` - Next suggestion
- `Alt + [` - Previous suggestion
- `Ctrl + Enter` - Open Copilot panel

### Copilot Chat Commands
| Command | Description |
|---------|-------------|
| `/explain` | Explain selected code |
| `/fix` | Fix issues in code |
| `/tests` | Generate tests |
| `/doc` | Generate documentation |
| `/new` | Create new file/project |
| `/simplify` | Simplify complex code |

### Chat Participants
| Participant | Use Case |
|-------------|----------|
| `@workspace` | Project-wide questions |
| `@vscode` | VS Code settings/features |
| `@terminal` | Terminal commands |

### Context Variables
| Variable | Description |
|----------|-------------|
| `#file` | Reference specific file |
| `#selection` | Use selected code |
| `#editor` | Current editor content |
| `#terminalLastCommand` | Last terminal command |

## JetBrains Integration

### Settings Location
- Settings → Tools → GitHub Copilot

### Configuration Options
```
Enable GitHub Copilot: ✓
Enable for:
  - Python ✓
  - JavaScript ✓
  - TypeScript ✓
  - Markdown ✓
```

### Keyboard Shortcuts
- `Tab` - Accept suggestion
- `Alt + \` - Manual trigger
- `Alt + [` / `Alt + ]` - Cycle suggestions

## Neovim Integration

### Setup with copilot.vim
```lua
-- In your init.lua or .vimrc
vim.g.copilot_filetypes = {
  ["*"] = true,
  ["markdown"] = true,
  ["yaml"] = true,
}

vim.g.copilot_no_tab_map = true
vim.api.nvim_set_keymap("i", "<C-J>", 'copilot#Accept("<CR>")', { silent = true, expr = true })
```

### copilot.lua Configuration
```lua
require("copilot").setup({
  suggestion = {
    enabled = true,
    auto_trigger = true,
    debounce = 75,
    keymap = {
      accept = "<M-l>",
      accept_word = false,
      accept_line = false,
      next = "<M-]>",
      prev = "<M-[>",
      dismiss = "<C-]>",
    },
  },
  filetypes = {
    yaml = true,
    markdown = true,
    help = false,
    gitcommit = false,
  },
})
```

## Best Practices

### Writing Effective Comments
```typescript
// Good: Specific and actionable
// Create a user authentication hook that:
// - Manages login/logout state
// - Persists session to localStorage
// - Provides loading and error states

// Bad: Too vague
// Create auth hook
```

### Providing Context
```typescript
// Good: Show the pattern you want
interface User {
  id: string;
  name: string;
}

// Copilot will follow this pattern
interface Product {
  // ... generates similar structure
}
```

### Using Type Information
```typescript
// TypeScript types guide Copilot
function processOrder(order: Order): ProcessedOrder {
  // Copilot understands the transformation needed
}
```

### Strategic File Organization
```
project/
├── .github/
│   └── copilot-instructions.md  # Project instructions
├── src/
│   ├── types/
│   │   └── index.ts             # Central types (Copilot references)
│   ├── utils/
│   │   └── helpers.ts           # Utility patterns
│   └── components/
│       └── Button.tsx           # Component patterns
```

## Troubleshooting

### Poor Suggestions?
1. Add more context in comments
2. Create example files with patterns
3. Use type annotations
4. Break complex tasks into steps

### Copilot Not Activating?
1. Check authentication status
2. Verify file type is enabled
3. Check network connectivity
4. Restart VS Code/IDE

### Slow Suggestions?
1. Check network latency
2. Reduce project complexity
3. Close unnecessary files
4. Check extension conflicts

## Enterprise Features

### Copilot Business/Enterprise
- Organization-wide settings
- Usage analytics
- Content exclusions
- Audit logs
- IP indemnification

### Content Exclusions
```yaml
# In GitHub organization settings
copilot:
  content_exclusions:
    - "**/*.env"
    - "**/secrets/**"
    - "**/private/**"
```

## Resources

### Official Documentation
- [GitHub Copilot Documentation](https://docs.github.com/en/copilot)
- [VS Code Copilot Guide](https://code.visualstudio.com/docs/copilot/overview)
- [Copilot Chat Guide](https://docs.github.com/en/copilot/github-copilot-chat)

### Community Resources
- [Copilot Tips & Tricks](https://github.blog/developer-skills/github/how-to-use-github-copilot-in-your-ide-tips-tricks-and-best-practices/)
- [r/GitHubCopilot](https://www.reddit.com/r/GitHubCopilot/)

### Learning Resources
- [Copilot Fundamentals](https://resources.github.com/learn/pathways/copilot/essentials/)
- [Prompt Engineering for Copilot](https://docs.github.com/en/copilot/using-github-copilot/prompt-engineering-for-github-copilot)

---

[← Back to README](../../README.md)
