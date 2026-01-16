# Agent Skills Template

Use this template to create new Agent Skills. Copy this structure and customize for your specific use case.

## Basic Template

```yaml
---
name: your-skill-name
description: A clear, comprehensive description of what this skill does and when to use it. Include specific capabilities and trigger keywords.
---

# Your Skill Name

Brief overview of what this skill helps accomplish.

## When to Use This Skill

- Situation 1
- Situation 2
- Situation 3

## Instructions

### Step 1: [First Action]

Detailed instructions for the first step...

### Step 2: [Second Action]

Detailed instructions for the second step...

### Step 3: [Final Action]

Detailed instructions for completion...

## Examples

### Example 1: [Use Case Name]

```
Input: ...
Output: ...
```

### Example 2: [Use Case Name]

```
Input: ...
Output: ...
```

## Guidelines

- Guideline 1: Important consideration
- Guideline 2: Best practice
- Guideline 3: Common pitfall to avoid

## Tips

- Tip 1
- Tip 2
- Tip 3
```

## Advanced Template with Tool Restrictions

```yaml
---
name: read-only-analyzer
description: Analyze code and files without making changes. Use when reviewing, auditing, or analyzing code for insights.
allowed-tools: Read, Grep, Glob
---

# Read-Only Code Analyzer

This skill analyzes code and provides insights without modifying files.

## Allowed Operations

- Reading files
- Searching code
- Finding files by pattern

## Prohibited Operations

- Writing files
- Editing code
- Running commands

## Analysis Process

1. **Identify target files** using Glob
2. **Search for patterns** using Grep
3. **Read relevant files** using Read
4. **Generate analysis report**

## Output Format

Provide analysis in this structure:
- Summary
- Key findings
- Recommendations
- Supporting evidence
```

## Template with Forked Context

```yaml
---
name: deep-research
description: Conduct thorough research on complex topics with isolated context. Use for in-depth investigations.
context: fork
agent: general-purpose
---

# Deep Research Skill

This skill conducts comprehensive research in an isolated context to avoid cluttering the main conversation.

## Research Process

1. Define research questions
2. Gather information from multiple sources
3. Synthesize findings
4. Present structured report

## Deliverables

- Executive summary
- Detailed findings
- References
- Recommendations
```

## Template with Scripts

**Directory structure:**
```
my-skill/
├── SKILL.md
├── scripts/
│   ├── helper.py
│   └── validate.sh
├── references/
│   └── api-docs.md
└── assets/
    └── template.json
```

**SKILL.md:**
```yaml
---
name: automated-workflow
description: Execute automated workflows with validation and helper scripts. Use when running multi-step processes.
allowed-tools: Read, Bash
---

# Automated Workflow

This skill combines instructions with utility scripts for complex workflows.

## Quick Start

Run the validation script:
```bash
bash scripts/validate.sh input-file.json
```

## Helper Scripts

### validate.sh
Validates input files before processing.

### helper.py
Provides utility functions for data transformation.

## Using References

For detailed API documentation, see [api-docs.md](references/api-docs.md).

For templates, use:
```bash
cp assets/template.json my-config.json
```

## Workflow Steps

1. Validate input using `scripts/validate.sh`
2. Process data with helper functions
3. Generate output report
```

## Template with Hooks

```yaml
---
name: secure-operations
description: Perform operations with pre-execution security checks. Use when security validation is required.
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/security-check.sh $TOOL_INPUT"
          once: true
---

# Secure Operations

This skill runs security checks before executing bash commands.

## Security Checks

Before any bash command execution:
1. Validate command safety
2. Check for dangerous patterns
3. Log command for audit trail

## Allowed Commands

- File operations (non-destructive)
- Read-only system queries
- Safe utility commands

## Blocked Commands

- System modification commands
- Network operations without approval
- File deletion without confirmation
```

## Naming Conventions

### Skill Name (YAML field)
- **Format**: lowercase with hyphens
- **Max length**: 64 characters
- **Allowed**: letters, numbers, hyphens
- **Examples**:
  - ✅ `code-reviewer`
  - ✅ `pdf-processor`
  - ✅ `api-tester-v2`
  - ❌ `Code_Reviewer` (uppercase, underscore)
  - ❌ `api tester` (spaces)

### Description (YAML field)
- **Max length**: 1024 characters
- **Should include**:
  - What the skill does (capabilities)
  - When to use it (trigger keywords)
  - Specific use cases
- **Example**: "Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction."

## Progressive Disclosure Pattern

For complex skills, use a main SKILL.md with links to detailed references:

**SKILL.md (keep under 500 lines):**
```yaml
---
name: complex-skill
description: Handle complex operations with multiple capabilities
---

# Complex Skill

## Quick Reference

Common operations:
- Operation 1: `command example`
- Operation 2: `command example`

## Detailed Guides

- [Complete API Reference](REFERENCE.md)
- [Advanced Examples](EXAMPLES.md)
- [Troubleshooting Guide](TROUBLESHOOTING.md)

## Getting Started

Basic usage:
```bash
python scripts/main.py --input data.json
```

For advanced usage, see [ADVANCED.md](ADVANCED.md).
```

## Field Reference

| Field | Required | Type | Description |
|-------|----------|------|-------------|
| `name` | Yes | string | Unique identifier (lowercase, hyphens) |
| `description` | Yes | string | What the skill does and when to use it |
| `allowed-tools` | No | string/array | CSV or YAML list of permitted tools |
| `model` | No | string | Specific Claude model ID |
| `context` | No | string | Set to `fork` for isolated context |
| `agent` | No | string | Agent type when context is forked |
| `hooks` | No | object | Lifecycle hooks definition |
| `user-invocable` | No | boolean | Show in slash menu (default: true) |
| `disable-model-invocation` | No | boolean | Prevent auto-invocation by Claude |

## String Substitutions

Use these variables in your skill content:

- `$ARGUMENTS` - Arguments passed when invoking the skill
- `${CLAUDE_SESSION_ID}` - Current session ID for logging/tracking

Example:
```markdown
Processing request with arguments: $ARGUMENTS
Session ID: ${CLAUDE_SESSION_ID}
```

## Common Patterns

### Read-Only Skills
```yaml
allowed-tools: Read, Grep, Glob
```

### Security-Focused Skills
```yaml
allowed-tools: Read, Grep
hooks:
  PreToolUse:
    - matcher: "*"
      hooks:
        - type: command
          command: "./security-audit.sh"
```

### User-Invoked Only
```yaml
user-invocable: true
disable-model-invocation: true
```

### Claude-Invoked Only
```yaml
user-invocable: false
```

## Quick Start Checklist

- [ ] Create skill directory in `.claude/skills/`
- [ ] Create SKILL.md with required frontmatter
- [ ] Write clear, comprehensive description
- [ ] Add detailed instructions in markdown
- [ ] Include examples and guidelines
- [ ] Test skill activation with relevant prompts
- [ ] Add optional scripts/references if needed
- [ ] Document in project README

## Testing Your Skill

1. **Check activation**: Use keywords from your description
2. **Verify instructions**: Ensure Claude follows your guidance
3. **Test edge cases**: Try boundary conditions
4. **Review output**: Confirm expected behavior
5. **Iterate**: Refine description and instructions based on results

## Distribution Options

1. **Project Skills**: Add to `.claude/skills/` and commit to version control
2. **Personal Skills**: Add to `~/.claude/skills/` for all your projects
3. **Plugin Skills**: Create plugin with `skills/` directory
4. **Managed Skills**: Organization-wide deployment (enterprise)

---

**Ready to create your skill?** Use this template as a starting point and customize for your specific use case!
