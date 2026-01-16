# Agent Skills Examples

This directory contains example Agent Skills demonstrating different patterns and use cases.

## Available Examples

### 1. Code Explainer
**Path**: `code-explainer/SKILL.md`

**What it demonstrates**:
- Basic skill structure
- Clear, educational instructions
- Use of analogies and examples
- Comprehensive guidance format

**Key Features**:
- Teaches Claude to explain code with analogies
- Includes diagram creation guidance
- Provides explanation templates
- Shows how to make technical concepts accessible

**Use this pattern when**:
- Creating educational skills
- Building explanation/teaching tools
- Developing user-friendly documentation skills

### 2. Read-Only Analyzer
**Path**: `read-only-analyzer/SKILL.md`

**What it demonstrates**:
- Tool restrictions with `allowed-tools`
- Security-focused design
- Analysis and reporting workflows
- Template-based outputs

**Key Features**:
- Restricted to Read, Grep, Glob only
- Cannot modify files (read-only)
- Comprehensive analysis capabilities
- Structured reporting templates

**Use this pattern when**:
- Creating audit/review skills
- Building read-only analysis tools
- Implementing security-sensitive workflows
- Need to prevent modifications

### 3. Git Workflow
**Path**: `git-workflow/SKILL.md`

**What it demonstrates**:
- Domain-specific expertise
- Command-focused workflows
- Best practices integration
- Practical, real-world usage

**Key Features**:
- Restricted to git commands only (`Bash(git:*)`)
- Conventional commit standards
- Branch management strategies
- Conflict resolution guidance

**Use this pattern when**:
- Creating workflow automation skills
- Building domain-specific tools
- Need to restrict commands to specific category
- Implementing best practice guides

## Comparison Matrix

| Feature | Code Explainer | Read-Only Analyzer | Git Workflow |
|---------|----------------|-------------------|--------------|
| **Tool Restrictions** | None | Read, Grep, Glob | Bash(git:*), Read, Grep, Glob |
| **Primary Use** | Education | Analysis | Automation |
| **Complexity** | Simple | Medium | Advanced |
| **Output Type** | Explanations | Reports | Commands |
| **Modification Level** | None | None | Repository only |
| **Best For** | Teaching | Auditing | Version control |

## How to Use These Examples

### Option 1: Direct Use

Copy an example to your skills directory:

```bash
# For personal use
cp -r code-explainer ~/.claude/skills/

# For project use
cp -r read-only-analyzer .claude/skills/

# For git workflow
cp -r git-workflow ~/.claude/skills/
```

### Option 2: Customize

1. Copy the example
2. Modify the `name` and `description`
3. Adjust the instructions to fit your needs
4. Add/remove tool restrictions
5. Test with relevant prompts

### Option 3: Learn and Create

Study these examples to understand:
- How to structure SKILL.md
- How to write effective descriptions
- How to use tool restrictions
- How to organize instructions
- How to provide examples

Then create your own custom skill!

## Testing the Examples

### Test Code Explainer

**Prompts that should activate it**:
- "Explain how this function works"
- "Can you help me understand this code?"
- "What does this React hook do?"

### Test Read-Only Analyzer

**Prompts that should activate it**:
- "Analyze this codebase for issues"
- "Review the code quality"
- "Audit the authentication module"

### Test Git Workflow

**Prompts that should activate it**:
- "Create a commit for these changes"
- "Help me create a pull request"
- "I have a merge conflict"

## Skill Patterns Demonstrated

### Pattern 1: Educational Skills (Code Explainer)

**Structure**:
```yaml
---
name: teaching-skill
description: Teaches concepts with analogies...
---

# Framework for teaching
1. Analogy
2. Visual aid
3. Step-by-step
4. Common pitfalls
```

**When to use**: Skills that teach or explain concepts

### Pattern 2: Analysis Skills (Read-Only Analyzer)

**Structure**:
```yaml
---
name: analyzer-skill
description: Analyzes without modifying...
allowed-tools: Read, Grep, Glob
---

# Analysis workflow
1. Collect data
2. Analyze patterns
3. Generate report
```

**When to use**: Skills that review or audit without changes

### Pattern 3: Workflow Skills (Git Workflow)

**Structure**:
```yaml
---
name: workflow-skill
description: Automates specific workflow...
allowed-tools: Bash(category:*), Read
---

# Workflow steps
1. Prepare
2. Execute
3. Verify
```

**When to use**: Skills that automate domain-specific tasks

## Customization Ideas

### From Code Explainer

Create variations for:
- **API Explainer**: Explain API endpoints and usage
- **Database Schema Explainer**: Explain table structures
- **Algorithm Explainer**: Explain algorithms with complexity analysis
- **Architecture Explainer**: Explain system architecture

### From Read-Only Analyzer

Create variations for:
- **Security Auditor**: Focus on security vulnerabilities
- **Performance Analyzer**: Focus on performance issues
- **Dependency Analyzer**: Analyze package dependencies
- **Documentation Analyzer**: Check documentation quality

### From Git Workflow

Create variations for:
- **GitHub Workflow**: Add GitHub-specific operations
- **GitLab Workflow**: GitLab CI/CD integration
- **Release Workflow**: Version bumping and changelog
- **Monorepo Workflow**: Multi-package management

## Learning Path

**Beginner**: Start with Code Explainer
- Simple structure
- No tool restrictions
- Clear use case
- Easy to understand

**Intermediate**: Move to Read-Only Analyzer
- Introduces tool restrictions
- More complex workflows
- Template-based outputs
- Security considerations

**Advanced**: Study Git Workflow
- Domain-specific expertise
- Command-specific restrictions
- Multiple workflows
- Best practices integration

## Additional Resources

- [OVERVIEW.md](../OVERVIEW.md) - Introduction to Agent Skills
- [TEMPLATE.md](../TEMPLATE.md) - Templates for creating skills
- [SPECIFICATION.md](../SPECIFICATION.md) - Technical specification
- [BEST_PRACTICES.md](../BEST_PRACTICES.md) - Best practices guide

## Contributing Examples

Want to add your own example?

1. Create a new directory in `examples/`
2. Add a complete SKILL.md file
3. Demonstrate a unique pattern or use case
4. Update this README with your example
5. Submit a pull request

## Questions?

- Check the [BEST_PRACTICES.md](../BEST_PRACTICES.md) guide
- Review the [SPECIFICATION.md](../SPECIFICATION.md) reference
- Read the main [OVERVIEW.md](../OVERVIEW.md)

---

**Remember**: These are examples for learning. Customize them for your specific needs!
