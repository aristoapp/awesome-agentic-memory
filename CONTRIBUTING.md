# Contributing to Awesome Context Engineering

Thank you for your interest in contributing! Your contributions help make this resource more valuable for everyone looking to personalize their AI assistants.

## Repository Structure

```
awesome-context-engineering/
├── README.md                    # Main entry point
├── CONTRIBUTING.md              # This file
├── LICENSE                      # Apache 2.0
└── docs/
    ├── services.md              # Personal AI services directory
    ├── personalization-beyond-platforms.md
    ├── context-types/           # Types of context information
    │   ├── external-files.md
    │   ├── session-history.md
    │   ├── external-apps.md
    │   ├── knowledge-bases.md
    │   └── personal-data.md
    └── platforms/               # Platform-specific guides
        ├── cursor.md
        ├── claude.md
        ├── chatgpt.md
        └── copilot.md
```

## How to Contribute

### Adding a Resource

1. **Check relevance**: Ensure your submission relates to context engineering, AI personalization, or related technologies.

2. **Check for duplicates**: Search existing files to avoid duplicate entries.

3. **Choose the right location**:
   - New tool/service → `docs/services.md`
   - Platform-specific tip → `docs/platforms/<platform>.md`
   - Context type info → `docs/context-types/<type>.md`
   - General resource → `README.md` Resources section

4. **Follow the format**:
   ```markdown
   | **Name** | Description | [Link](URL) |
   ```
   or
   ```markdown
   - [Resource Name](URL) - Brief description.
   ```

5. **Quality standards**: Resources should be:
   - Actively maintained (for repositories)
   - Well-documented
   - Relevant to context engineering
   - Accessible (not entirely behind paywalls)

### Adding a New Platform Guide

Create a new file in `docs/platforms/` following this template:

```markdown
# [Platform] Personalization Guide

> Brief description of the platform.

## Overview

Introduction to the platform and its personalization features.

## Personalization Methods

### 1. [Method Name]
Description and examples...

### 2. [Method Name]
Description and examples...

## Configuration Examples

Code examples and templates...

## Best Practices

Tips for effective use...

## Resources

- [Official Docs](URL)
- [Community Resources](URL)

---

[← Back to README](../../README.md)
```

### Adding a New Context Type Guide

Create a new file in `docs/context-types/` following this template:

```markdown
# [Context Type]

> Brief description of this context type.

## Overview

What this context type is and why it matters.

## Key Concepts

Important concepts and terminology.

## Implementation

How to use this context type:
- Code examples
- Platform-specific instructions
- Tools and services

## Best Practices

Tips and recommendations.

## Resources

Related links and further reading.

---

[← Back to README](../../README.md)
```

## Pull Request Guidelines

### Before Submitting

1. **One topic per PR**: Easier to review and merge
2. **Test links**: Ensure all URLs work
3. **Spell check**: Run a spell check on your content
4. **Consistent formatting**: Match existing style

### PR Description Template

```markdown
## What does this PR add/change?
Brief description of the changes.

## Type of Change
- [ ] New resource/tool
- [ ] Platform guide update
- [ ] Bug fix (broken link, typo)
- [ ] Documentation improvement
- [ ] New section/category

## Checklist
- [ ] Links have been tested
- [ ] Content follows existing format
- [ ] No duplicate entries
- [ ] Spelling and grammar checked
```

### Review Process

1. Maintainers will review your PR
2. Feedback may be provided for revisions
3. Once approved, it will be merged
4. You'll be credited in the commit

## What Makes a Resource Awesome?

### Must Have
- **Relevant**: Directly related to AI personalization
- **Quality**: Well-maintained and documented
- **Accessible**: Available for individual use
- **Working**: Links must be functional

### Nice to Have
- Free tier or open source
- Active community
- Regular updates
- Good documentation

### Avoid
- Abandoned projects (>2 years inactive)
- Paywalled without free tier
- Promotional/marketing content
- Duplicate functionality without differentiation

## Content Categories

### README.md
- High-level overview
- Quick-start resources
- Major categories and links

### docs/services.md
- Comprehensive tool listings
- Categorized by function
- Include pricing info when available

### docs/platforms/
- Platform-specific guides
- Step-by-step instructions
- Configuration examples
- Best practices

### docs/context-types/
- Conceptual explanations
- Implementation guides
- Tool recommendations
- Code examples

## Code of Conduct

- Be respectful and constructive
- Focus on resource quality, not promotion
- Help improve existing content
- Respect maintainer decisions

## Questions?

- Open an issue for discussion
- Ask in PR comments
- Reach out to maintainers

## License

By contributing, you agree that your contributions will be licensed under the Apache License 2.0.

---

Thank you for contributing! 🎉
