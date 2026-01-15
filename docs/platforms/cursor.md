# Cursor Personalization Guide

> Customizing Cursor IDE's AI assistant to understand your codebase, coding style, and preferences.

## Overview

[Cursor](https://cursor.com/) is an AI-first code editor built on VSCode that deeply integrates AI assistance into the development workflow. It offers multiple ways to personalize AI behavior through rules, context management, and project-specific configurations.

## Personalization Methods

### 1. Cursor Rules

Cursor Rules are instructions that guide how the AI behaves in your projects. They can be configured at different levels.

#### Project-Level Rules (`.cursor/rules`)

Create a `.cursor/rules` file in your project root:

```markdown
# Project Rules for MyApp

## Code Style
- Use TypeScript for all new files
- Follow Airbnb ESLint configuration
- Use named exports over default exports
- Prefer functional components with hooks

## Project Architecture
- This is a Next.js 14 app using App Router
- API routes are in `/app/api`
- Components are in `/components` with co-located styles
- Use Prisma for database access

## Conventions
- Use camelCase for variables and functions
- Use PascalCase for components and types
- Include JSDoc comments for public APIs
- Write unit tests for utility functions

## Preferences
- Prefer concise solutions
- Always consider error handling
- Suggest accessibility improvements for UI
- Mention performance implications when relevant
```

#### Global Rules

Set global rules in Cursor Settings:
1. Open Cursor Settings (`Cmd/Ctrl + ,`)
2. Navigate to "Rules for AI"
3. Add your global preferences

```markdown
## My Global Preferences
- I'm a senior developer, skip basic explanations
- Always use TypeScript
- Include type definitions
- Prefer functional programming patterns
- Use modern ES2022+ syntax
```

### 2. .cursorrules File (Legacy)

The `.cursorrules` file in project root is still supported:

```markdown
You are an expert TypeScript developer working on a Next.js application.

Key guidelines:
- Always use TypeScript with strict mode
- Prefer server components unless client interactivity is needed
- Use Tailwind CSS for styling
- Follow React Server Components best practices

Project structure:
- /app - Next.js app router pages
- /components - Reusable UI components
- /lib - Utility functions and shared code
- /prisma - Database schema and migrations
```

### 3. .cursorignore

Exclude files from AI context to reduce noise:

```gitignore
# .cursorignore

# Dependencies
node_modules/
.pnpm-store/

# Build outputs
.next/
dist/
build/

# Large files
*.min.js
*.bundle.js

# Sensitive files
.env*
*.pem
secrets/

# Generated files
generated/
__generated__/
```

### 4. Context Management

#### @-Mentions
Reference specific context in conversations:

| Mention | Description |
|---------|-------------|
| `@file` | Include specific file in context |
| `@folder` | Include folder contents |
| `@codebase` | Search entire codebase |
| `@docs` | Search documentation |
| `@web` | Search the web |
| `@git` | Git history and changes |

#### Examples:
```
@file:src/utils/auth.ts How does authentication work?
@folder:components Create a new Button component following our patterns
@codebase Where is user validation implemented?
@docs Next.js How do I implement middleware?
```

### 5. Docs Context

Add documentation for Cursor to reference:

1. **Add docs via Settings**:
   - Settings → Features → Docs
   - Add URLs to documentation sites
   - Cursor indexes and makes searchable

2. **Reference in conversations**:
   ```
   @docs Prisma How do I create a many-to-many relation?
   ```

**Good documentation to add:**
- Framework docs (Next.js, React, etc.)
- Library docs (Prisma, tRPC, etc.)
- Internal documentation
- API references

### 6. Notepads

Create reusable context snippets:

1. Open Command Palette (`Cmd/Ctrl + Shift + P`)
2. Search "Cursor: Open Notepad"
3. Create named notepads for different contexts

**Example Notepads:**

```markdown
# API Development Notepad
When building APIs:
- Use Zod for input validation
- Return consistent error formats
- Include pagination for list endpoints
- Add rate limiting headers
```

```markdown
# React Component Notepad
When creating components:
- Use forwardRef for DOM elements
- Include displayName for debugging
- Export types alongside component
- Add Storybook stories
```

## Configuration Files

### Project Structure
```
your-project/
├── .cursor/
│   └── rules           # Project-specific AI rules
├── .cursorrules        # Legacy rules file (still works)
├── .cursorignore       # Files to exclude from context
└── ...
```

### Full .cursor/rules Example

```markdown
# Project: E-Commerce Platform

## Tech Stack
- Framework: Next.js 14 (App Router)
- Language: TypeScript 5.0+
- Styling: Tailwind CSS + shadcn/ui
- Database: PostgreSQL with Prisma
- Auth: NextAuth.js
- State: Zustand for client state
- API: tRPC for type-safe APIs

## Code Standards

### TypeScript
- Enable strict mode
- Use `unknown` over `any`
- Define explicit return types for functions
- Use discriminated unions for complex state

### React
- Prefer Server Components by default
- Use 'use client' only when necessary
- Implement error boundaries for client components
- Use Suspense for data fetching

### Naming Conventions
- Files: kebab-case (user-profile.tsx)
- Components: PascalCase (UserProfile)
- Functions: camelCase (getUserProfile)
- Constants: SCREAMING_SNAKE_CASE (MAX_RETRIES)
- Types/Interfaces: PascalCase with descriptive names

### File Organization
/app
  /(auth)         # Auth-related routes
  /(dashboard)    # Dashboard routes
  /api            # API routes
/components
  /ui             # shadcn/ui components
  /features       # Feature-specific components
  /layouts        # Layout components
/lib
  /utils          # Utility functions
  /hooks          # Custom hooks
  /api            # API client code
/prisma
  schema.prisma   # Database schema

## Common Patterns

### Data Fetching
Use server components with direct database access:
```typescript
async function ProductPage({ params }: { params: { id: string } }) {
  const product = await prisma.product.findUnique({
    where: { id: params.id }
  });
  return <ProductDetails product={product} />;
}
```

### Form Handling
Use react-hook-form with Zod validation:
```typescript
const schema = z.object({
  email: z.string().email(),
  password: z.string().min(8)
});
```

### Error Handling
Always handle errors gracefully:
```typescript
try {
  // operation
} catch (error) {
  if (error instanceof PrismaClientKnownRequestError) {
    // handle specific error
  }
  throw error;
}
```

## AI Behavior Preferences
- Be concise but thorough
- Include TypeScript types in all code
- Suggest tests for new functionality
- Consider edge cases and error states
- Mention security implications when relevant
- Prefer composition over inheritance
```

## Best Practices

### Rules Organization
1. **Keep rules focused**: Separate technical rules from preferences
2. **Update regularly**: Rules should evolve with your project
3. **Be specific**: Vague rules lead to inconsistent behavior
4. **Include examples**: Show patterns you want followed

### Context Optimization
1. **Use .cursorignore**: Exclude irrelevant files
2. **Reference specific files**: Use @file for precise context
3. **Keep rules concise**: Long rules may be truncated
4. **Prioritize important info**: Put critical rules first

### Team Collaboration
1. **Commit .cursor/rules**: Share rules with your team
2. **Document rule changes**: PR reviews for rule updates
3. **Standardize patterns**: Agree on team conventions
4. **Review periodically**: Rules may become outdated

## Troubleshooting

### AI Not Following Rules?
- Check rules file location (`.cursor/rules` or `.cursorrules`)
- Ensure rules are specific and clear
- Try refreshing Cursor (`Cmd/Ctrl + Shift + P` → "Developer: Reload Window")
- Check for conflicting global vs. project rules

### Context Not Loading?
- Verify file isn't in `.cursorignore`
- Check file size (very large files may be excluded)
- Use explicit @file mentions
- Ensure codebase indexing is complete

### Slow Performance?
- Add large directories to `.cursorignore`
- Reduce indexed documentation
- Clear and rebuild index if needed

## Resources

### Official Documentation
- [Cursor Rules Documentation](https://docs.cursor.com/context/rules-for-ai)
- [Context Guide](https://docs.cursor.com/context/)
- [Cursor Features](https://docs.cursor.com/features/)

### Community Resources
- [Cursor Forum](https://forum.cursor.com/)
- [Cursor Discord](https://discord.gg/cursor)
- [cursor.directory](https://cursor.directory/) - Community rules library

### Examples
- [awesome-cursorrules](https://github.com/PatrickJS/awesome-cursorrules) - Collection of cursor rules
- [Cursor Rules Examples](https://cursor.directory/) - Community-contributed rules

---

[← Back to README](../../README.md)
