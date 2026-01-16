# Agent Skills Best Practices

This guide provides recommendations for creating effective, maintainable, and performant Agent Skills.

## Writing Effective Descriptions

The description is the most critical part of your skill - it determines when Claude activates it.

### ✅ Good Description Practices

**1. Be Specific About Capabilities**
```yaml
# ❌ Vague
description: Helps with documents

# ✅ Specific
description: Extract text and tables from PDF files, fill forms, merge documents
```

**2. Include Trigger Keywords**
```yaml
# ❌ Missing triggers
description: Processes PDF files

# ✅ With triggers
description: Extract text and tables from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, forms, or document extraction.
```

**3. List All Capabilities**
```yaml
# ❌ Incomplete
description: Code review skill

# ✅ Complete
description: Review pull requests for code quality, security issues, performance problems, and best practices violations. Check for proper error handling, test coverage, and documentation. Use when reviewing PRs, analyzing code changes, or ensuring code quality standards.
```

**4. Explain When to Use**
```yaml
# ❌ No context
description: Analyzes performance metrics

# ✅ With context
description: Analyze application performance metrics including response times, memory usage, and throughput. Generate optimization recommendations. Use when investigating performance issues, optimizing slow endpoints, or planning scaling strategies.
```

### Description Templates

**Generic Skill**:
```yaml
description: [Action verbs: what it does] [Objects: what it works with]. [Use cases: when to use it]. Use when [trigger scenarios] or when the user mentions [keywords].
```

**Example**:
```yaml
description: Create and manipulate spreadsheets with formulas, charts, and data analysis. Export to XLSX and CSV formats. Use when working with tabular data, creating reports, or when the user mentions spreadsheets, Excel, data tables, or analysis.
```

## Skill Structure Best Practices

### 1. Keep SKILL.md Focused

**Target**: Under 500 lines for main SKILL.md

**Why**: Performance, context window efficiency, readability

**How**: Use progressive disclosure

```markdown
---
name: complex-skill
description: ...
---

# Complex Skill

## Quick Start
[Essential info here]

## Common Operations
[Frequently used patterns]

## Detailed References
- For complete API docs, see [API.md](references/API.md)
- For advanced examples, see [EXAMPLES.md](references/EXAMPLES.md)
- For troubleshooting, see [TROUBLESHOOTING.md](references/TROUBLESHOOTING.md)
```

### 2. Organize Support Files

```
skill-name/
├── SKILL.md              # Main instructions (< 500 lines)
├── scripts/              # Executable utilities
│   ├── validate.py
│   └── helper.sh
├── references/           # Detailed docs (loaded on demand)
│   ├── API.md
│   ├── EXAMPLES.md
│   └── TROUBLESHOOTING.md
└── assets/              # Templates and resources
    ├── template.json
    └── config.yaml
```

### 3. Use Progressive Disclosure

**Instead of embedding everything**:
```markdown
# ❌ Don't do this
---
name: api-skill
---

# API Skill

## Complete API Reference
[3000 lines of API documentation]

## All Examples
[1000 lines of examples]
```

**Link to references**:
```markdown
# ✅ Do this
---
name: api-skill
---

# API Skill

## Quick Start
```bash
curl https://api.example.com/v1/resource
```

## Common Patterns
- GET request: [example]
- POST request: [example]

For complete API reference, see [API.md](references/API.md).
For 50+ examples, see [EXAMPLES.md](references/EXAMPLES.md).
```

## Content Quality

### 1. Include Examples

**Always include practical examples**:

```markdown
## Examples

### Example 1: Extract Text from PDF
```bash
python scripts/extract.py --input document.pdf --output text.txt
```

### Example 2: Fill PDF Form
```bash
python scripts/fill_form.py --template form.pdf --data data.json --output filled.pdf
```

### Example 3: Merge Multiple PDFs
```bash
python scripts/merge.py --inputs file1.pdf file2.pdf file3.pdf --output merged.pdf
```
```

### 2. Provide Clear Instructions

**Use step-by-step format**:

```markdown
## How to Use This Skill

### Step 1: Prepare Input Data
Create a JSON file with your data:
```json
{
  "name": "John Doe",
  "email": "john@example.com"
}
```

### Step 2: Run Validation
```bash
python scripts/validate.py data.json
```

### Step 3: Process
```bash
python scripts/process.py --input data.json --output result.json
```

### Step 4: Verify Output
Check the `result.json` file for processed data.
```

### 3. Document Edge Cases

```markdown
## Edge Cases and Limitations

### Large Files
- Files over 100MB may require streaming mode: `--stream`
- Memory usage: approximately 2x file size

### Special Characters
- File names with spaces: use quotes `"file name.pdf"`
- Unicode characters: ensure UTF-8 encoding

### Error Handling
- Missing files: returns error code 1
- Invalid format: returns error code 2
- Permission denied: returns error code 3
```

## Tool Restrictions

### When to Use allowed-tools

**Use tool restrictions for**:
1. Read-only operations
2. Security-sensitive tasks
3. Limited-scope operations
4. Educational/training scenarios

### Common Patterns

**Read-Only Analysis**:
```yaml
---
name: code-analyzer
description: Analyze code for patterns and issues
allowed-tools: Read, Grep, Glob
---
```

**Safe Data Processing**:
```yaml
---
name: data-transformer
description: Transform data files without side effects
allowed-tools: Read, Write
---
```

**Python-Only Execution**:
```yaml
---
name: python-runner
description: Run Python scripts for data analysis
allowed-tools: Read, Bash(python:*)
---
```

**Git Operations Only**:
```yaml
---
name: git-workflow
description: Manage git operations
allowed-tools: Read, Bash(git:*)
---
```

## Performance Optimization

### 1. Lazy Loading

**Don't load everything upfront**:
```markdown
# ❌ Avoid
Inline all documentation in SKILL.md (5000 lines)

# ✅ Better
Main SKILL.md (300 lines) + linked references
```

### 2. Execute vs. Load

**For scripts, execute rather than load**:

```markdown
# ❌ Don't do this
Read the Python script and understand it:
[Include 500 lines of Python code]

# ✅ Do this
Run the helper script:
```bash
python scripts/helper.py --action process
```
```

### 3. Context Management

**Use forked context for heavy tasks**:

```yaml
---
name: deep-analysis
description: Comprehensive codebase analysis
context: fork
agent: Explore
---
```

**When to fork**:
- Long-running research
- Tasks that generate lots of output
- Independent analysis that shouldn't clutter main conversation

## Security Best Practices

### 1. Validate Input

**Always validate before processing**:

```yaml
---
name: file-processor
description: Process user files
hooks:
  PreToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "./scripts/validate-input.sh $TOOL_INPUT"
---
```

### 2. Restrict Capabilities

**Use least privilege**:

```yaml
# ❌ Too permissive
allowed-tools: Bash  # Allows any command

# ✅ Restricted
allowed-tools: Bash(python:*), Read, Write  # Only Python, read, write
```

### 3. Audit Actions

**Log important operations**:

```yaml
hooks:
  PostToolUse:
    - matcher: "Bash"
      hooks:
        - type: command
          command: "echo '${CLAUDE_SESSION_ID}: $TOOL_NAME' >> audit.log"
```

### 4. Sanitize Data

```markdown
## Security Guidelines

- **Never execute untrusted code** without validation
- **Sanitize file paths** to prevent directory traversal
- **Validate data formats** before processing
- **Check file permissions** before reading/writing
- **Use absolute paths** to avoid ambiguity
```

## Naming Conventions

### Skill Names

**Format**: lowercase-with-hyphens

```yaml
# ✅ Good names
name: code-reviewer
name: pdf-processor
name: api-tester-v2
name: react-best-practices

# ❌ Bad names
name: Code_Reviewer      # uppercase, underscore
name: api tester         # spaces
name: APITester          # camelCase
name: api_tester_final_v3_NEW  # too complex
```

### File Organization

```
skill-name/
├── SKILL.md                    # Uppercase, required
├── README.md                   # Optional, for GitHub
├── scripts/
│   ├── validate_input.py      # snake_case for Python
│   ├── process-data.sh        # kebab-case for shell
│   └── helper.js              # camelCase for JavaScript
├── references/
│   ├── API.md                 # Uppercase for docs
│   ├── EXAMPLES.md
│   └── TROUBLESHOOTING.md
└── assets/
    ├── template.json          # lowercase for configs
    └── schema.yaml
```

## Testing Your Skills

### 1. Test Activation

**Verify the skill activates correctly**:

```markdown
Test Cases:
1. ✅ "Can you review this PR?" → Should activate code-reviewer
2. ✅ "Analyze this code for issues" → Should activate code-reviewer
3. ✅ "Check code quality" → Should activate code-reviewer
4. ❌ "Write new code" → Should NOT activate (not relevant)
```

### 2. Test Instructions

**Ensure Claude follows your guidance**:

1. Activate skill
2. Verify Claude uses specified tools
3. Check output format matches expectations
4. Confirm edge cases are handled

### 3. Test Tool Restrictions

**Verify allowed-tools works**:

```yaml
allowed-tools: Read, Grep, Glob
```

Test:
- ✅ Reading files should work
- ✅ Searching should work
- ❌ Writing files should be blocked
- ❌ Bash commands should be blocked

### 4. Test Progressive Disclosure

**Verify references load on demand**:

1. Activate skill
2. Check if main instructions appear
3. Reference a linked document
4. Verify it loads when needed

## Common Pitfalls to Avoid

### 1. Overly Broad Descriptions

```yaml
# ❌ Too broad
description: Helps with coding tasks

# ✅ Specific
description: Review pull requests for code quality, security, and best practices. Check error handling, test coverage, and documentation. Use when reviewing PRs or analyzing code changes.
```

### 2. Missing Trigger Keywords

```yaml
# ❌ Missing keywords
description: Processes documents

# ✅ With keywords
description: Extract text from PDF files, fill forms, merge documents. Use when working with PDF files or when the user mentions PDFs, documents, forms, or extraction.
```

### 3. Monolithic Skills

```yaml
# ❌ One skill does everything
name: developer-tools
description: Code, test, review, deploy, monitor, debug...

# ✅ Focused skills
name: code-reviewer
description: Review code for quality and security

name: test-runner
description: Run and analyze test suites

name: deployment-helper
description: Deploy applications to production
```

### 4. Inline Everything

```markdown
# ❌ 5000-line SKILL.md with everything embedded

# ✅ 300-line SKILL.md with references
See [API.md](references/API.md) for details
```

### 5. No Examples

```markdown
# ❌ Just instructions
Follow these abstract guidelines...

# ✅ With examples
## Example 1: Basic Usage
[concrete example]

## Example 2: Advanced Usage
[concrete example]
```

## Maintenance Best Practices

### 1. Version Your Skills

```yaml
# Add version in metadata
---
name: api-client
description: API client for REST services
# Custom metadata (not part of core spec, but useful)
version: 2.1.0
---

# API Client v2.1.0

## Changelog
- v2.1.0: Added OAuth2 support
- v2.0.0: Complete rewrite with async support
- v1.0.0: Initial release
```

### 2. Document Changes

**Keep a changelog**:

```markdown
## Version History

### v2.1.0 (2026-01-15)
- Added OAuth2 authentication
- Improved error messages
- Fixed bug with large responses

### v2.0.0 (2025-12-01)
- Breaking: Changed API from sync to async
- Added streaming support
- Performance improvements
```

### 3. Test After Updates

**Checklist after modifying a skill**:
- [ ] YAML syntax valid
- [ ] Description still accurate
- [ ] Examples still work
- [ ] Scripts have correct permissions
- [ ] References still load
- [ ] Tool restrictions still appropriate

### 4. Archive Old Versions

```
skills/
├── api-client/           # Current version
│   └── SKILL.md
└── archived/
    ├── api-client-v1/   # Old version
    │   └── SKILL.md
    └── deprecated-skill/
        └── SKILL.md
```

## Distribution Strategies

### 1. Project Skills

**When**: Skill is project-specific

**Where**: `.claude/skills/` in repository

**Benefits**:
- Version controlled
- Team collaboration
- Project context

```bash
my-project/
├── .claude/
│   └── skills/
│       └── project-workflow/
│           └── SKILL.md
└── src/
```

### 2. Personal Skills

**When**: Skill used across all your projects

**Where**: `~/.claude/skills/`

**Benefits**:
- Always available
- Personal customization
- Cross-project utility

```bash
~/.claude/
└── skills/
    ├── my-code-style/
    ├── my-git-workflow/
    └── my-templates/
```

### 3. Plugin Skills

**When**: Distributing to community

**Where**: Plugin repository with `skills/` directory

**Benefits**:
- Easy distribution
- Version management
- Discoverability

```bash
my-plugin/
├── skills/
│   ├── skill-one/
│   └── skill-two/
└── plugin.json
```

### 4. Managed Skills

**When**: Organization-wide deployment

**Where**: Enterprise management system

**Benefits**:
- Centralized control
- Compliance
- Consistency

## Documentation Standards

### README.md (Optional)

**For GitHub repositories**:

```markdown
# Skill Name

Brief description of what the skill does.

## Installation

```bash
claude plugin install username/skill-name
```

## Usage

Description of how to use the skill...

## Examples

[Examples]

## Requirements

- Claude Code 1.x or later
- Python 3.8+
- [Other dependencies]

## License

Apache 2.0
```

### Inline Documentation

```markdown
---
name: documented-skill
description: Well-documented example skill
---

# Documented Skill

## Overview
Clear explanation of purpose

## Prerequisites
- Requirement 1
- Requirement 2

## Quick Start
Minimal example to get started

## Usage
Detailed usage instructions

## Examples
Multiple concrete examples

## API Reference
Technical details (or link to references/API.md)

## Troubleshooting
Common issues and solutions

## Advanced
Advanced usage patterns

## Limitations
Known limitations and constraints
```

## Summary Checklist

Before publishing a skill, verify:

### Required Elements
- [ ] Valid SKILL.md with YAML frontmatter
- [ ] `name` field (lowercase, hyphens)
- [ ] `description` field (specific, with keywords)
- [ ] Clear markdown instructions

### Quality Checks
- [ ] Description includes trigger keywords
- [ ] Instructions are step-by-step
- [ ] Examples are included
- [ ] Edge cases documented
- [ ] SKILL.md under 500 lines (or uses progressive disclosure)

### Testing
- [ ] Skill activates with expected prompts
- [ ] Instructions produce desired behavior
- [ ] Tool restrictions work as intended
- [ ] Scripts execute correctly
- [ ] References load on demand

### Documentation
- [ ] Purpose clearly explained
- [ ] Prerequisites listed
- [ ] Usage examples provided
- [ ] Limitations documented
- [ ] Version history (if applicable)

### Security
- [ ] Input validation implemented
- [ ] Tool restrictions appropriate
- [ ] No hardcoded secrets
- [ ] Audit logging (if needed)
- [ ] Safe defaults

### Performance
- [ ] Main SKILL.md optimized
- [ ] References used for details
- [ ] Scripts executed, not loaded
- [ ] Forked context for heavy tasks

---

Following these best practices will help you create effective, maintainable, and professional Agent Skills that provide value to users across different platforms.
