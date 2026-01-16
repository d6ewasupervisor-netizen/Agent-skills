# Agent Skills Documentation

Comprehensive documentation for understanding and creating Agent Skills.

## What's in This Directory

This documentation collection provides everything you need to understand and create Agent Skills - the open format for giving AI agents new capabilities.

### 📚 Documentation Files

| Document | Purpose | For |
|----------|---------|-----|
| [OVERVIEW.md](OVERVIEW.md) | Introduction to Agent Skills | Everyone |
| [SPECIFICATION.md](SPECIFICATION.md) | Technical format reference | Developers |
| [TEMPLATE.md](TEMPLATE.md) | Skill creation templates | Skill authors |
| [BEST_PRACTICES.md](BEST_PRACTICES.md) | Authoring guidelines | Skill authors |
| [examples/](examples/) | Working skill examples | Everyone |

## Quick Start

### New to Agent Skills?

1. **Start here**: Read [OVERVIEW.md](OVERVIEW.md)
   - Understand what Agent Skills are
   - Learn how they work
   - See the benefits

2. **Explore examples**: Browse [examples/](examples/)
   - See real working skills
   - Understand different patterns
   - Get inspired

3. **Create your first skill**: Use [TEMPLATE.md](TEMPLATE.md)
   - Copy a template
   - Customize for your needs
   - Deploy and test

### Ready to Create Skills?

1. **Choose a template**: See [TEMPLATE.md](TEMPLATE.md)
2. **Follow best practices**: Read [BEST_PRACTICES.md](BEST_PRACTICES.md)
3. **Check the spec**: Reference [SPECIFICATION.md](SPECIFICATION.md)
4. **Study examples**: Learn from [examples/](examples/)

### Building Advanced Skills?

1. **Master the specification**: Deep dive into [SPECIFICATION.md](SPECIFICATION.md)
2. **Optimize performance**: Follow [BEST_PRACTICES.md](BEST_PRACTICES.md)
3. **Study patterns**: Analyze [examples/](examples/)
4. **Experiment**: Try different configurations and approaches

## Documentation Overview

### OVERVIEW.md
**What it covers**:
- Agent Skills introduction
- How skills work
- Platform support
- Getting started
- Use cases and benefits

**Read this if**:
- You're new to Agent Skills
- You want to understand the concept
- You need to explain skills to others

### SPECIFICATION.md
**What it covers**:
- Complete technical specification
- YAML frontmatter fields
- Directory structure
- File formats
- Tool restrictions
- Hooks and advanced features

**Read this if**:
- You need technical details
- You're implementing a skill
- You're troubleshooting issues
- You want to use advanced features

### TEMPLATE.md
**What it covers**:
- Ready-to-use templates
- Common patterns
- Field reference
- String substitutions
- Quick start checklists

**Read this if**:
- You're creating a new skill
- You need a starting point
- You want copy-paste templates
- You're exploring patterns

### BEST_PRACTICES.md
**What it covers**:
- Writing effective descriptions
- Skill structure guidelines
- Performance optimization
- Security considerations
- Testing strategies
- Common pitfalls

**Read this if**:
- You want to create quality skills
- You're optimizing existing skills
- You need authoring guidance
- You want to avoid common mistakes

### examples/
**What it includes**:
- **code-explainer**: Educational skill example
- **read-only-analyzer**: Tool restriction example
- **git-workflow**: Workflow automation example

**Explore this if**:
- You learn by example
- You want working skills to study
- You need inspiration
- You want to see different patterns

## Learning Paths

### Path 1: Quick Start (30 minutes)

1. Read [OVERVIEW.md](OVERVIEW.md) (10 min)
2. Browse [examples/code-explainer](examples/code-explainer) (5 min)
3. Copy [TEMPLATE.md](TEMPLATE.md) basic template (5 min)
4. Create and test your first skill (10 min)

### Path 2: Comprehensive (2 hours)

1. Read [OVERVIEW.md](OVERVIEW.md) (15 min)
2. Study all [examples/](examples/) (30 min)
3. Read [BEST_PRACTICES.md](BEST_PRACTICES.md) (30 min)
4. Review [SPECIFICATION.md](SPECIFICATION.md) (30 min)
5. Create a custom skill (15 min)

### Path 3: Expert (4+ hours)

1. Complete Path 2 (2 hours)
2. Deep dive [SPECIFICATION.md](SPECIFICATION.md) (1 hour)
3. Study all example patterns (30 min)
4. Experiment with advanced features (1+ hour)
5. Create multiple skills with different patterns

## Common Use Cases

### Educational Skills
**Goal**: Teach concepts or explain code

**Resources**:
- Example: [code-explainer](examples/code-explainer)
- Pattern: Educational in [TEMPLATE.md](TEMPLATE.md)
- Tips: Description writing in [BEST_PRACTICES.md](BEST_PRACTICES.md)

### Analysis Skills
**Goal**: Analyze code without modifications

**Resources**:
- Example: [read-only-analyzer](examples/read-only-analyzer)
- Pattern: Read-only in [TEMPLATE.md](TEMPLATE.md)
- Reference: Tool restrictions in [SPECIFICATION.md](SPECIFICATION.md)

### Workflow Skills
**Goal**: Automate domain-specific tasks

**Resources**:
- Example: [git-workflow](examples/git-workflow)
- Pattern: Workflow in [TEMPLATE.md](TEMPLATE.md)
- Guide: Testing in [BEST_PRACTICES.md](BEST_PRACTICES.md)

### Documentation Skills
**Goal**: Generate or maintain documentation

**Resources**:
- Pattern: Progressive disclosure in [BEST_PRACTICES.md](BEST_PRACTICES.md)
- Template: Advanced template in [TEMPLATE.md](TEMPLATE.md)
- Reference: String substitutions in [SPECIFICATION.md](SPECIFICATION.md)

### Security Skills
**Goal**: Security analysis or validation

**Resources**:
- Example: Security patterns in [read-only-analyzer](examples/read-only-analyzer)
- Guide: Security in [BEST_PRACTICES.md](BEST_PRACTICES.md)
- Reference: Hooks in [SPECIFICATION.md](SPECIFICATION.md)

## Quick Reference

### Creating a Skill

**Minimum requirements**:
```yaml
---
name: skill-name
description: What it does and when to use it
---

# Skill Title
Instructions here
```

**Location options**:
- Personal: `~/.claude/skills/skill-name/SKILL.md`
- Project: `.claude/skills/skill-name/SKILL.md`

### Testing a Skill

1. **Create the skill** in appropriate location
2. **Restart Claude** (if needed)
3. **Test activation** with relevant prompts
4. **Verify behavior** matches expectations
5. **Iterate** on description and instructions

### Common Fields

```yaml
---
name: skill-name                    # Required
description: Detailed description   # Required
allowed-tools: Read, Grep, Glob    # Optional
model: claude-sonnet-4             # Optional
context: fork                      # Optional
agent: Explore                     # Optional (with context)
user-invocable: true              # Optional (default: true)
---
```

## Troubleshooting

### Skill Not Activating

**Check**:
1. Description in [BEST_PRACTICES.md](BEST_PRACTICES.md)
2. Trigger keywords in [SPECIFICATION.md](SPECIFICATION.md)
3. Naming in [TEMPLATE.md](TEMPLATE.md)

### YAML Errors

**Reference**:
1. Format in [SPECIFICATION.md](SPECIFICATION.md)
2. Templates in [TEMPLATE.md](TEMPLATE.md)
3. Examples in [examples/](examples/)

### Tool Restrictions Not Working

**See**:
1. allowed-tools in [SPECIFICATION.md](SPECIFICATION.md)
2. Patterns in [BEST_PRACTICES.md](BEST_PRACTICES.md)
3. Example: [read-only-analyzer](examples/read-only-analyzer)

## External Resources

### Official Documentation
- **Specification**: [agentskills.io/specification](https://agentskills.io/specification)
- **Homepage**: [agentskills.io](https://agentskills.io)
- **Claude Docs**: [platform.claude.com/docs/en/agents-and-tools/agent-skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)

### Repositories
- **Specification Repo**: [github.com/agentskills/agentskills](https://github.com/agentskills/agentskills)
- **Anthropic Skills**: [github.com/anthropics/skills](https://github.com/anthropics/skills)
- **Awesome Agent Skills**: [github.com/skillmatic-ai/awesome-agent-skills](https://github.com/skillmatic-ai/awesome-agent-skills)

### Platform Guides
- **VS Code**: [code.visualstudio.com/docs/copilot/customization/agent-skills](https://code.visualstudio.com/docs/copilot/customization/agent-skills)
- **Claude Support**: [support.claude.com/en/articles/12512176-what-are-skills](https://support.claude.com/en/articles/12512176-what-are-skills)

## Contributing

Want to improve this documentation?

**You can**:
- Fix errors or typos
- Add missing information
- Create new examples
- Improve explanations
- Add use cases

**How to contribute**:
1. Edit the relevant file
2. Test your changes
3. Submit a pull request
4. Describe your improvements

## License

This documentation is part of the Agent-skills repository. See the main repository LICENSE for details.

The Agent Skills specification is maintained by Anthropic as an open standard.

## About This Repository

This repository contains:
- **Documentation**: This comprehensive guide
- **Examples**: Working skill implementations
- **Skills**: Production-ready skills from vercel-labs

All resources are designed to help you understand and create effective Agent Skills.

## Need Help?

1. **Search the docs**: Use the table above to find relevant sections
2. **Check examples**: See [examples/](examples/) for working code
3. **Review best practices**: Read [BEST_PRACTICES.md](BEST_PRACTICES.md)
4. **Read the spec**: Reference [SPECIFICATION.md](SPECIFICATION.md)

---

**Ready to get started?** Choose your learning path above and begin creating Agent Skills!
