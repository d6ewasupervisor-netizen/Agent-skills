---
name: read-only-analyzer
description: Analyze code and files without making changes. Reviews code quality, identifies patterns, and provides insights. Use when reviewing, auditing, or analyzing code without modifications.
allowed-tools: Read, Grep, Glob
---

# Read-Only Code Analyzer

This skill provides comprehensive code analysis without modifying any files. Perfect for code reviews, audits, and understanding unfamiliar codebases.

## Capabilities

### ✅ What This Skill Can Do

- **Read files**: Access and analyze file contents
- **Search code**: Find patterns, functions, and usage
- **Find files**: Locate files by name or pattern
- **Generate reports**: Create analysis summaries
- **Identify patterns**: Detect code smells and anti-patterns
- **Suggest improvements**: Recommend optimizations (without implementing)

### ❌ What This Skill Cannot Do

- Write new files
- Edit existing files
- Execute commands
- Make any changes to the codebase

## Analysis Types

### 1. Code Quality Review

**Focus Areas**:
- Code organization and structure
- Naming conventions
- Code duplication
- Complexity metrics
- Documentation quality

**Process**:
```
1. Identify target files using Glob
2. Read each file using Read
3. Search for patterns using Grep
4. Analyze and report findings
```

### 2. Security Audit

**Focus Areas**:
- Hardcoded credentials
- SQL injection vulnerabilities
- XSS vulnerabilities
- Insecure dependencies
- Authentication/authorization issues

**Search Patterns**:
- API keys: `grep -i "api[_-]?key\s*="`
- Passwords: `grep -i "password\s*="`
- SQL queries: `grep "SELECT.*FROM"`
- Eval usage: `grep "eval("`

### 3. Architecture Analysis

**Focus Areas**:
- Module dependencies
- Component relationships
- Design patterns
- Separation of concerns
- Code organization

**Approach**:
1. Map file structure
2. Identify imports/exports
3. Trace data flow
4. Document architecture

### 4. Performance Review

**Focus Areas**:
- N+1 queries
- Inefficient loops
- Large payload sizes
- Unnecessary re-renders
- Memory leaks

**Patterns to Find**:
- Nested loops
- Synchronous operations in loops
- Large arrays/objects
- Unoptimized algorithms

## Analysis Workflow

### Step 1: Scope Definition

Determine what to analyze:
- Specific files
- Directory
- Entire codebase
- Specific patterns

### Step 2: Data Collection

```bash
# Find relevant files
Glob: "**/*.js"

# Search for patterns
Grep: "TODO|FIXME|XXX"

# Read specific files
Read: path/to/file.js
```

### Step 3: Pattern Analysis

Look for:
- Code smells
- Anti-patterns
- Best practice violations
- Potential bugs
- Security issues

### Step 4: Report Generation

**Report Structure**:
```markdown
# Code Analysis Report

## Summary
[High-level overview]

## Critical Issues
[Severity: High - requires immediate attention]

## Warnings
[Severity: Medium - should be addressed]

## Suggestions
[Severity: Low - nice to have improvements]

## Metrics
- Total files analyzed: X
- Lines of code: Y
- Issues found: Z

## Detailed Findings
[File-by-file breakdown]

## Recommendations
[Prioritized action items]
```

## Analysis Templates

### Template 1: Code Quality

```markdown
## File: [filename]

### Structure
- Functions: X
- Classes: Y
- Lines of code: Z

### Quality Indicators
- ✅ Good: [List positives]
- ⚠️  Issues: [List concerns]
- 🔴 Critical: [List serious problems]

### Recommendations
1. [Specific improvement]
2. [Specific improvement]
3. [Specific improvement]
```

### Template 2: Security Audit

```markdown
## Security Analysis

### High Risk
- [Issue 1]: Found in [file:line]
  - Impact: [description]
  - Recommendation: [fix]

### Medium Risk
- [Issue 1]: Found in [file:line]
  - Impact: [description]
  - Recommendation: [fix]

### Best Practices
- [Observation 1]
- [Observation 2]
```

### Template 3: Performance Review

```markdown
## Performance Analysis

### Hot Spots
1. [Location]: [Issue]
   - Current: [description]
   - Impact: [performance cost]
   - Suggested: [optimization]

### Optimization Opportunities
- [Opportunity 1]
- [Opportunity 2]

### Benchmarking Recommendations
- [What to measure]
- [How to measure]
```

## Common Patterns to Detect

### Anti-Patterns

**1. God Object**
```
Search: Classes with >500 lines or >20 methods
```

**2. Code Duplication**
```
Look for: Similar code blocks in multiple files
```

**3. Magic Numbers**
```
Search: Hardcoded numbers without constants
```

**4. Deep Nesting**
```
Look for: Indentation levels >4
```

### Best Practices Violations

**1. Missing Error Handling**
```javascript
// Search for try blocks without catch
grep "try\s*{" -A 20 | grep -v "catch"
```

**2. Console Logs in Production**
```javascript
grep "console\.(log|debug|warn)"
```

**3. Commented Code**
```javascript
grep "^[ \t]*//"
```

**4. TODO Comments**
```javascript
grep -i "todo|fixme|hack|xxx"
```

## Analysis Checklist

### Before Analysis
- [ ] Understand the scope
- [ ] Identify focus areas
- [ ] Prepare search patterns
- [ ] Plan report structure

### During Analysis
- [ ] Systematically review each area
- [ ] Document findings with file:line references
- [ ] Categorize by severity
- [ ] Note patterns and trends

### After Analysis
- [ ] Generate comprehensive report
- [ ] Prioritize recommendations
- [ ] Provide specific examples
- [ ] Include actionable steps

## Best Practices

1. **Be Specific**: Always include file paths and line numbers
2. **Categorize**: Group findings by type and severity
3. **Context**: Explain why something is an issue
4. **Actionable**: Provide clear recommendations
5. **Balanced**: Acknowledge good practices too

## Example Analysis Session

```markdown
User: "Analyze the authentication module for security issues"