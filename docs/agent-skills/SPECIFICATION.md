# Agent Skills Specification Reference

This document provides a comprehensive technical reference for the Agent Skills format.

**Official Specification**: [agentskills.io/specification](https://agentskills.io/specification)

## Format Version

This reference is based on the Agent Skills specification as of January 2026.

## Core Concepts

### What is an Agent Skill?

An Agent Skill is a **folder** containing at minimum a `SKILL.md` file with:
- **YAML frontmatter**: Configuration metadata
- **Markdown content**: Instructions for the AI agent

### Key Characteristics

1. **Model-Invoked**: The AI agent automatically decides when to use a skill based on matching the request to the skill's description
2. **Lazy Loading**: Only skill name and description are loaded at startup; full content loads when activated
3. **Shared Context**: Skills share the agent's context window with conversation history
4. **Portable**: Works across any AI platform that supports the Agent Skills standard

## Directory Structure

### Minimal Structure
```
skill-name/
└── SKILL.md          # Required
```

### Full Structure
```
skill-name/
├── SKILL.md          # Required: Metadata and instructions
├── scripts/          # Optional: Executable scripts
│   ├── helper.py
│   └── validate.sh
├── references/       # Optional: Detailed documentation
│   ├── API.md
│   └── EXAMPLES.md
└── assets/          # Optional: Templates and resources
    ├── template.json
    └── config.yaml
```

### Nested Discovery

Agent Skills are automatically discovered from nested `.claude/skills/` directories, supporting monorepo structures:

```
project/
├── .claude/
│   └── skills/
│       └── project-wide-skill/
│           └── SKILL.md
└── packages/
    ├── backend/
    │   └── .claude/
    │       └── skills/
    │           └── backend-skill/
    │               └── SKILL.md
    └── frontend/
        └── .claude/
            └── skills/
                └── frontend-skill/
                    └── SKILL.md
```

## SKILL.md Format

### Structure

```markdown
---
# YAML frontmatter
field: value
---

# Markdown content
Instructions for the AI agent
```

### Required YAML Fields

#### name
- **Type**: String
- **Format**: Lowercase letters, numbers, hyphens only
- **Max Length**: 64 characters
- **Purpose**: Unique identifier for the skill
- **Examples**:
  - `code-reviewer`
  - `pdf-processor`
  - `api-tester-v2`

#### description
- **Type**: String
- **Max Length**: 1024 characters
- **Purpose**: Tells the AI when to use this skill
- **Should Include**:
  - What the skill does (specific capabilities)
  - When to use it (trigger keywords)
  - Relevant use cases
- **Example**:
  ```yaml
  description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
  ```

### Optional YAML Fields

#### allowed-tools
- **Type**: String (CSV) or Array (YAML)
- **Purpose**: Restrict which tools the agent can use when this skill is active
- **Format Options**:
  ```yaml
  # CSV format
  allowed-tools: Read, Grep, Glob

  # YAML array format
  allowed-tools:
    - Read
    - Grep
    - Glob
  ```
- **Use Cases**:
  - Read-only skills
  - Security-sensitive operations
  - Limited-scope workflows
- **Tool Names**: Read, Write, Edit, Grep, Glob, Bash, etc.

#### model
- **Type**: String
- **Purpose**: Specify which Claude model to use for this skill
- **Format**: Model ID string
- **Examples**:
  - `claude-sonnet-4-20250514`
  - `claude-opus-4-5-20251101`
  - `claude-haiku-4-20250514`
- **Use Case**: When a specific model's capabilities are required

#### context
- **Type**: String
- **Valid Values**: `fork`
- **Purpose**: Run skill in isolated sub-agent with separate conversation context
- **Use Cases**:
  - Research tasks that would clutter main conversation
  - Independent analysis
  - Background processing
- **Example**:
  ```yaml
  context: fork
  agent: Explore
  ```

#### agent
- **Type**: String
- **Purpose**: Specify agent type when `context: fork`
- **Valid Values**:
  - `Explore` - Fast exploration and searching
  - `Plan` - Planning and architecture
  - `general-purpose` - General tasks
  - `Bash` - Command execution
- **Required**: Only when `context: fork` is set

#### hooks
- **Type**: Object
- **Purpose**: Define lifecycle hooks for skill execution
- **Structure**:
  ```yaml
  hooks:
    <EventName>:
      - matcher: "<pattern>"
        hooks:
          - type: command
            command: "<script>"
            once: true/false
  ```
- **Events**:
  - `PreToolUse` - Before tool execution
  - `PostToolUse` - After tool execution
  - `Stop` - When conversation ends
- **Example**:
  ```yaml
  hooks:
    PreToolUse:
      - matcher: "Bash"
        hooks:
          - type: command
            command: "./scripts/security-check.sh $TOOL_INPUT"
            once: true
  ```

#### user-invocable
- **Type**: Boolean
- **Default**: `true`
- **Purpose**: Control whether skill appears in slash command menu
- **Values**:
  - `true` - Appears in menu, can be auto-invoked
  - `false` - Hidden from menu, only auto-invoked by Claude
- **Use Cases**:
  - Internal/background skills (`false`)
  - User-accessible skills (`true`)

#### disable-model-invocation
- **Type**: Boolean
- **Default**: `false`
- **Purpose**: Prevent Claude from automatically invoking the skill
- **Use Cases**:
  - Skills that should only be manually invoked
  - User-triggered workflows
- **Combination**:
  ```yaml
  # User-invoked only (appears in menu, not auto-invoked)
  user-invocable: true
  disable-model-invocation: true

  # Claude-invoked only (hidden from menu, auto-invoked)
  user-invocable: false
  # disable-model-invocation defaults to false
  ```

### Markdown Content

The markdown content below the YAML frontmatter contains instructions for the AI agent.

**Best Practices**:
- Keep main SKILL.md under 500 lines
- Use clear headings and structure
- Include examples
- Provide guidelines
- Use progressive disclosure (link to detailed references)

**Sections to Include**:
- Overview/Introduction
- When to use this skill
- Step-by-step instructions
- Examples
- Guidelines/Best practices
- Tips/Common pitfalls

## String Substitutions

The following variables can be used in skill content:

| Variable | Description | Example Use |
|----------|-------------|-------------|
| `$ARGUMENTS` | Arguments passed when invoking skill | Parsing user input |
| `${CLAUDE_SESSION_ID}` | Current session ID | Logging, tracking |

**Example**:
```markdown
Processing request for session ${CLAUDE_SESSION_ID}
Arguments: $ARGUMENTS
```

## Storage Locations

Skills are discovered from multiple locations in priority order:

| Priority | Type | Path | Scope |
|----------|------|------|-------|
| 1 | Managed | Platform-specific | Organization-wide |
| 2 | Personal | `~/.claude/skills/` | User-specific |
| 3 | Project | `.claude/skills/` | Repository-specific |
| 4 | Plugin | `skills/` in plugin directory | Plugin users |

**Resolution**: If skills with the same name exist in multiple locations, higher priority locations override lower priority ones.

## File Naming Conventions

### SKILL.md
- **Required**: Every skill must have exactly one `SKILL.md` file
- **Case Sensitive**: Must be uppercase
- **Location**: Root of skill directory

### Supporting Files
- **scripts/**: Executable scripts (`.sh`, `.py`, `.js`, etc.)
- **references/**: Documentation (`.md` files)
- **assets/**: Templates, configs (`.json`, `.yaml`, `.txt`, etc.)

## Progressive Disclosure

For complex skills, use a layered approach:

1. **SKILL.md**: Overview and common operations (< 500 lines)
2. **References**: Detailed documentation loaded on demand
3. **Scripts**: Utility code executed, not loaded into context

**Example SKILL.md**:
```markdown
---
name: complex-task
description: Handle complex operations efficiently
---

# Complex Task

## Quick Start
Common operations here...

## Detailed References
- [Complete API Guide](references/API.md)
- [Advanced Examples](references/EXAMPLES.md)
- [Troubleshooting](references/TROUBLESHOOTING.md)

## Utility Scripts
```bash
python scripts/helper.py --action process
```
```

**Benefits**:
- Keeps context window usage low
- Agent loads only needed information
- Better organization
- Easier maintenance

## Tool Restrictions

### Purpose
Control which tools the agent can use to:
- Enforce read-only operations
- Improve security
- Limit scope of actions
- Prevent unintended modifications

### Common Patterns

**Read-Only**:
```yaml
allowed-tools: Read, Grep, Glob
```

**Analysis Only**:
```yaml
allowed-tools: Read, Grep, Glob, Bash(python:*)
```

**Safe Writing**:
```yaml
allowed-tools: Read, Grep, Glob, Write
```

### Tool Reference

Common tool names:
- `Read` - Read files
- `Write` - Write new files
- `Edit` - Edit existing files
- `Grep` - Search file contents
- `Glob` - Find files by pattern
- `Bash` - Execute commands
- `Bash(python:*)` - Only Python commands
- `Bash(git:*)` - Only Git commands

## Forked Context

### Purpose
Run skills in isolated sub-agent with separate context:
- Prevents cluttering main conversation
- Allows parallel processing
- Independent history

### Configuration
```yaml
---
name: research-skill
description: Conduct deep research
context: fork
agent: general-purpose
---
```

### Agent Types
- `Explore` - Fast codebase exploration
- `Plan` - Architecture and planning
- `general-purpose` - General tasks
- `Bash` - Command execution specialist

### Limitations
- Built-in agents don't inherit skills from parent
- Only custom subagents can access skills (via explicit `skills` field)

## Hooks

### Event Types

#### PreToolUse
- **When**: Before tool execution
- **Use Cases**: Validation, security checks, logging
- **Variables**: `$TOOL_NAME`, `$TOOL_INPUT`

#### PostToolUse
- **When**: After tool execution
- **Use Cases**: Logging, cleanup, notifications
- **Variables**: `$TOOL_NAME`, `$TOOL_OUTPUT`

#### Stop
- **When**: Conversation ends
- **Use Cases**: Cleanup, final logging, resource release

### Hook Structure
```yaml
hooks:
  <EventName>:
    - matcher: "<pattern>"        # Tool name or glob pattern
      hooks:
        - type: command
          command: "<script>"      # Command to execute
          once: true              # Run only once (optional)
```

### Example: Security Validation
```yaml
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/validate-command.sh $TOOL_INPUT"
          once: false
```

### Example: Audit Logging
```yaml
hooks:
  PostToolUse:
    - matcher: "*"
      hooks:
        - type: command
          command: "echo '[${CLAUDE_SESSION_ID}] $TOOL_NAME' >> audit.log"
```

## Skill Visibility Matrix

| Setting | Slash Menu | Auto-Invoke | Use Case |
|---------|-----------|-------------|----------|
| Default (no settings) | ✓ | ✓ | Public skills |
| `user-invocable: false` | ✗ | ✓ | Background skills |
| `disable-model-invocation: true` | ✓ | ✗ | Manual-only |
| Both `false` + `true` | ✗ | ✗ | Disabled |

## Best Practices

### Description Writing
1. **Be Specific**: List exact capabilities
2. **Include Keywords**: Words users might say
3. **Explain When**: Situations for skill activation
4. **Avoid Vague Terms**: "Helps with" → "Extracts data from"

**Poor**: "Helps with documents"
**Good**: "Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction."

### Structure Guidelines
- One skill = one clear purpose
- Keep SKILL.md focused and concise
- Use progressive disclosure for complexity
- Include examples for clarity
- Document edge cases and limitations

### Performance Optimization
- Lazy load: Keep main SKILL.md small
- Link to references instead of embedding
- Execute scripts instead of loading code
- Use appropriate context (fork when needed)

### Security Considerations
- Use `allowed-tools` to restrict capabilities
- Implement hooks for validation
- Review scripts for security issues
- Document required permissions
- Use least privilege principle

## Validation

### YAML Validation
- Valid YAML syntax
- Required fields present
- Field types correct
- String lengths within limits

### Name Validation
- Lowercase letters, numbers, hyphens only
- No spaces, underscores, or special characters
- Max 64 characters
- Unique within scope

### Description Validation
- Max 1024 characters
- Clear and comprehensive
- Includes trigger keywords
- Explains when to use

## Troubleshooting

### Skill Not Activating
- **Issue**: Skill doesn't trigger when expected
- **Solutions**:
  - Improve description with specific keywords
  - Add more trigger phrases
  - Test with explicit skill invocation (`/skill-name`)
  - Check name uniqueness

### YAML Parse Error
- **Issue**: Skill fails to load
- **Solutions**:
  - Validate YAML syntax
  - Check for required fields
  - Verify field types
  - Use proper quoting for special characters

### Scripts Not Executing
- **Issue**: Helper scripts fail to run
- **Solutions**:
  - Check file permissions (`chmod +x script.sh`)
  - Verify script path (use forward slashes)
  - Test script independently
  - Check `allowed-tools` includes `Bash`

### Context Issues
- **Issue**: Skill clutters conversation
- **Solutions**:
  - Use `context: fork`
  - Implement progressive disclosure
  - Keep main SKILL.md concise
  - Link to references

## Platform Compatibility

The Agent Skills format is supported by:
- **Claude** (all platforms: Claude.ai, Claude Code, Claude API)
- **Cursor** (code editor)
- **VS Code Copilot**
- **OpenCode**
- **GitHub Copilot**
- **Letta**
- **goose**
- And growing...

Skills written to this specification work across all compatible platforms without modification.

## Version History

- **January 2026**: Current specification with full feature set
- **December 2025**: OpenAI adoption announced
- **2025**: Initial release and community adoption

## Additional Resources

- **Official Spec**: [agentskills.io/specification](https://agentskills.io/specification)
- **Documentation**: [agentskills.io](https://agentskills.io)
- **GitHub**: [agentskills/agentskills](https://github.com/agentskills/agentskills)
- **Anthropic Skills**: [anthropics/skills](https://github.com/anthropics/skills)
- **Claude Docs**: [platform.claude.com/docs/en/agents-and-tools/agent-skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview)
- **Support**: [support.claude.com/en/articles/12512176-what-are-skills](https://support.claude.com/en/articles/12512176-what-are-skills)

## Contributing

The Agent Skills specification is open to community contributions:
- Submit issues for clarifications
- Propose enhancements
- Share example skills
- Improve documentation

See the official repositories for contribution guidelines.

---

**Last Updated**: January 2026
**Specification Version**: 1.0 (current)
