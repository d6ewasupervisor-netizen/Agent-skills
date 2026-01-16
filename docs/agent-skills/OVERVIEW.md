# Agent Skills Overview

## What Are Agent Skills?

**Agent Skills** are an open format for giving AI agents new capabilities and expertise. They are folders of instructions, scripts, and resources that agents can discover and use to perform better at specific tasks.

**Key Principle:** Write once, use everywhere. Skills you create work across different AI platforms and tools that adopt the Agent Skills standard.

## Why Use Agent Skills?

- **Modular Capabilities**: Package specialized knowledge into reusable components
- **Automatic Invocation**: Claude automatically decides when to use skills based on your request
- **Portable**: Works across AI platforms (Claude, Cursor, VS Code Copilot, OpenCode, etc.)
- **Open Standard**: Maintained by Anthropic as an open format with community contributions
- **Performance Optimized**: Only loads full skill content when needed

## How Agent Skills Work

1. **Discovery**: Claude automatically discovers skills from `.claude/skills/` directories
2. **Matching**: When you make a request, Claude checks skill descriptions to find relevant ones
3. **Loading**: Only the matched skill's full content is loaded (lazy loading for performance)
4. **Execution**: Claude follows the skill's instructions to complete your task

## File Structure

Every skill is a folder containing at minimum a `SKILL.md` file:

```
my-skill/
├── SKILL.md           # Required: Skill metadata and instructions
├── scripts/           # Optional: Executable scripts
├── references/        # Optional: Documentation files
└── assets/           # Optional: Templates, resources
```

## SKILL.md Format

```yaml
---
name: skill-name
description: What this skill does and when to use it
---

# Skill Title

## Instructions
Your markdown content with guidance for Claude
```

### Required Fields

- **name**: Lowercase identifier with hyphens (e.g., `code-reviewer`)
- **description**: Clear explanation of what the skill does and when to use it

### Optional Fields

- **allowed-tools**: Restrict which tools Claude can use
- **model**: Specify which Claude model to use
- **context**: Set to `fork` for isolated sub-agent context
- **agent**: Specify agent type when using forked context
- **hooks**: Define lifecycle hooks for the skill
- **user-invocable**: Control whether skill appears in slash command menu (default: true)

## Storage Locations (Priority Order)

| Type | Path | Scope |
|------|------|-------|
| Managed | Platform-specific | Organization-wide |
| Personal | `~/.claude/skills/` | All your projects |
| Project | `.claude/skills/` | Current repository |
| Plugin | `skills/` in plugin | Plugin users |

## Platform Adoption

Agent Skills are supported by:
- **Claude** (Claude.ai, Claude Code, Claude API)
- **Cursor**
- **VS Code Copilot**
- **OpenCode**
- **Letta**
- **goose**
- **GitHub Copilot**
- And growing...

## Getting Started

### 1. Create Your First Skill

```bash
mkdir -p ~/.claude/skills/my-first-skill
cd ~/.claude/skills/my-first-skill
```

### 2. Create SKILL.md

```yaml
---
name: code-explainer
description: Explains code with visual diagrams and analogies. Use when explaining how code works or teaching about a codebase.
---

# Code Explainer

When explaining code, always include:

1. **Start with an analogy**: Compare the code to something from everyday life
2. **Draw a diagram**: Use ASCII art to show flow or structure
3. **Walk through step-by-step**: Explain what happens in sequence
4. **Highlight gotchas**: Common mistakes or misconceptions

Keep explanations conversational and use multiple analogies for complex concepts.
```

### 3. Use Your Skill

Just start a conversation with Claude! The skill will automatically activate when you ask questions about code.

## Example Skills

See the `/skills` directory in this repository for production-ready examples:
- **react-best-practices**: React performance optimization rules
- **vercel-deploy-claimable**: Deployment workflows
- **document skills**: PDF, DOCX, PPTX, XLSX manipulation

## Resources

- **Official Specification**: [agentskills.io/specification](https://agentskills.io/specification)
- **Documentation**: [agentskills.io](https://agentskills.io)
- **GitHub Repository**: [agentskills/agentskills](https://github.com/agentskills/agentskills)
- **Anthropic Skills Repo**: [anthropics/skills](https://github.com/anthropics/skills)
- **Claude Documentation**: [platform.claude.com/docs/en/agents-and-tools/agent-skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- **VS Code Guide**: [code.visualstudio.com/docs/copilot/customization/agent-skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills)

## License

Agent Skills specification is open source and maintained by Anthropic. Individual skills may have different licenses - check each skill's metadata.

---

**Next Steps:**
- Read the [Specification Reference](SPECIFICATION.md) for detailed format requirements
- Check out [Best Practices](BEST_PRACTICES.md) for authoring guidelines
- Use the [Template](TEMPLATE.md) to create new skills
