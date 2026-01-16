---
name: git-workflow
description: Manage git operations following best practices. Create commits, branches, PRs, and handle merge conflicts. Use when working with git, version control, or when the user mentions commits, branches, pull requests, or git operations.
allowed-tools: Read, Grep, Glob, Bash(git:*)
---

# Git Workflow

This skill helps manage git operations following industry best practices and conventional commit standards.

## Core Capabilities

- Create well-formatted commits
- Manage branches effectively
- Handle merge conflicts
- Create pull requests
- Review git history
- Revert changes safely

## Commit Best Practices

### Conventional Commits Format

```
<type>(<scope>): <subject>

<body>

<footer>
```

### Commit Types

| Type | When to Use | Example |
|------|-------------|---------|
| `feat` | New feature | `feat(auth): add OAuth2 login` |
| `fix` | Bug fix | `fix(api): handle null responses` |
| `docs` | Documentation | `docs(readme): update setup instructions` |
| `style` | Code formatting | `style(components): fix indentation` |
| `refactor` | Code restructuring | `refactor(utils): simplify date helpers` |
| `perf` | Performance improvement | `perf(queries): add database indexing` |
| `test` | Add/update tests | `test(auth): add login edge cases` |
| `chore` | Maintenance | `chore(deps): update dependencies` |
| `ci` | CI/CD changes | `ci(actions): add test workflow` |
| `build` | Build system | `build(webpack): optimize bundle size` |

### Commit Workflow

**Step 1: Check Status**
```bash
git status
```

**Step 2: Review Changes**
```bash
git diff
git diff --staged
```

**Step 3: Stage Changes**
```bash
# Stage specific files
git add path/to/file.js

# Stage all changes
git add .

# Interactive staging
git add -p
```

**Step 4: Create Commit**
```bash
git commit -m "feat(auth): add OAuth2 login

Implement OAuth2 authentication flow with Google and GitHub providers.
Includes token refresh and session management.

Closes #123"
```

## Branch Management

### Branch Naming Convention

```
<type>/<description>

Examples:
- feature/user-authentication
- fix/payment-processing-bug
- refactor/api-client
- docs/api-documentation
- hotfix/critical-security-patch
```

### Branch Workflow

**Create Branch**
```bash
# Create and switch to new branch
git checkout -b feature/new-feature

# Create from specific branch
git checkout -b fix/bug main
```

**Switch Branches**
```bash
git checkout branch-name
```

**Update Branch**
```bash
# Get latest from remote
git fetch origin

# Rebase on main
git rebase origin/main
```

**Delete Branch**
```bash
# Delete local branch
git branch -d feature/old-feature

# Force delete
git branch -D feature/old-feature

# Delete remote branch
git push origin --delete feature/old-feature
```

## Pull Request Workflow

### Step 1: Prepare Branch

```bash
# Ensure branch is up to date
git fetch origin
git rebase origin/main

# Run tests
npm test

# Fix any issues
git add .
git commit -m "test: fix failing tests"
```

### Step 2: Push Branch

```bash
git push -u origin feature/new-feature
```

### Step 3: Create PR

**PR Title**: Same format as commit messages
```
feat(module): add new capability
```

**PR Description Template**:
```markdown
## Summary
Brief description of changes

## Changes
- Change 1
- Change 2
- Change 3

## Testing
- [ ] Unit tests added
- [ ] Integration tests pass
- [ ] Manual testing completed

## Screenshots
[If applicable]

## Related Issues
Closes #123
Related to #456

## Checklist
- [ ] Code follows style guidelines
- [ ] Self-review completed
- [ ] Documentation updated
- [ ] No breaking changes (or documented)
```

### Step 4: Address Review Comments

```bash
# Make changes
git add .
git commit -m "refactor: address PR feedback"
git push
```

## Merge Conflict Resolution

### Step 1: Identify Conflicts

```bash
git status
```

### Step 2: Review Conflict

```bash
# View conflicting file
cat path/to/conflicted-file.js
```

**Conflict Markers**:
```
<<<<<<< HEAD
Your changes
=======
Their changes
>>>>>>> branch-name
```

### Step 3: Resolve Conflict

1. Read both versions
2. Decide which to keep or merge both
3. Remove conflict markers
4. Test the resolution

### Step 4: Complete Resolution

```bash
# Stage resolved file
git add path/to/resolved-file.js

# Continue rebase/merge
git rebase --continue
# or
git merge --continue

# If needed, abort
git rebase --abort
# or
git merge --abort
```

## History Management

### View History

```bash
# Full log
git log

# One-line format
git log --oneline

# Graph view
git log --graph --oneline --all

# Specific file history
git log -- path/to/file.js

# Last N commits
git log -n 5
```

### Search History

```bash
# Search commit messages
git log --grep="search term"

# Search code changes
git log -S "function name"

# By author
git log --author="John Doe"

# By date
git log --since="2 weeks ago"
git log --until="2023-12-31"
```

### Undo Changes

**Unstage Files**
```bash
git restore --staged file.js
```

**Discard Working Changes**
```bash
git restore file.js
```

**Amend Last Commit**
```bash
# Add to last commit
git add forgotten-file.js
git commit --amend --no-edit

# Change commit message
git commit --amend -m "new message"
```

**Revert Commit**
```bash
# Create new commit that undoes changes
git revert commit-hash
```

**Reset to Previous State**
```bash
# Soft reset (keep changes)
git reset --soft HEAD~1

# Mixed reset (unstage changes)
git reset HEAD~1

# Hard reset (discard changes)
git reset --hard HEAD~1
```

## Advanced Workflows

### Interactive Rebase

```bash
# Rebase last 3 commits
git rebase -i HEAD~3
```

**Commands**:
- `pick` - keep commit
- `reword` - change commit message
- `edit` - modify commit
- `squash` - combine with previous
- `fixup` - combine, discard message
- `drop` - remove commit

### Cherry Pick

```bash
# Apply specific commit
git cherry-pick commit-hash

# Apply multiple commits
git cherry-pick commit1 commit2 commit3
```

### Stash Changes

```bash
# Stash current changes
git stash

# Stash with message
git stash save "work in progress"

# List stashes
git stash list

# Apply stash
git stash apply

# Apply and remove
git stash pop

# Drop stash
git stash drop
```

## Safety Checks

### Before Committing

- [ ] Review all changes with `git diff`
- [ ] Ensure tests pass
- [ ] Check for debug code/console logs
- [ ] Verify no sensitive data
- [ ] Follow commit message format

### Before Pushing

- [ ] Commits are logical and atomic
- [ ] Commit messages are clear
- [ ] Branch is up to date with main
- [ ] All tests pass
- [ ] No merge conflicts

### Before Creating PR

- [ ] Branch tested thoroughly
- [ ] Documentation updated
- [ ] Breaking changes documented
- [ ] PR description complete
- [ ] Related issues linked

## Git Aliases

Suggested aliases for `.gitconfig`:

```ini
[alias]
    st = status
    co = checkout
    br = branch
    ci = commit
    unstage = restore --staged
    last = log -1 HEAD
    visual = log --graph --oneline --all --decorate
    amend = commit --amend --no-edit
    undo = reset HEAD~1
```

## Common Scenarios

### Scenario 1: Feature Development

```bash
# 1. Create feature branch
git checkout -b feature/new-feature main

# 2. Make changes and commit
git add .
git commit -m "feat: implement new feature"

# 3. Keep updated with main
git fetch origin
git rebase origin/main

# 4. Push and create PR
git push -u origin feature/new-feature
```

### Scenario 2: Bug Fix

```bash
# 1. Create fix branch
git checkout -b fix/bug-description main

# 2. Fix and commit
git add .
git commit -m "fix: resolve bug description

Detailed explanation of the fix"

# 3. Push
git push -u origin fix/bug-description
```

### Scenario 3: Update PR

```bash
# 1. Make requested changes
git add .
git commit -m "refactor: address review feedback"

# 2. Update remote
git push
```

### Scenario 4: Sync Fork

```bash
# 1. Add upstream remote
git remote add upstream https://github.com/original/repo.git

# 2. Fetch upstream
git fetch upstream

# 3. Merge into main
git checkout main
git merge upstream/main

# 4. Push to fork
git push origin main
```

## Troubleshooting

### Issue: Accidentally Committed to Wrong Branch

```bash
# 1. Note commit hash
git log

# 2. Switch to correct branch
git checkout correct-branch

# 3. Cherry-pick commit
git cherry-pick commit-hash

# 4. Return to wrong branch
git checkout wrong-branch

# 5. Remove commit
git reset --hard HEAD~1
```

### Issue: Need to Split Large Commit

```bash
# 1. Reset to before commit
git reset HEAD~1

# 2. Stage and commit in parts
git add file1.js
git commit -m "feat: part 1"

git add file2.js
git commit -m "feat: part 2"
```

### Issue: Pushed Sensitive Data

```bash
# ⚠️  Contact security team first!

# 1. Remove from history
git filter-branch --force --index-filter \
  "git rm --cached --ignore-unmatch path/to/file" \
  --prune-empty --tag-name-filter cat -- --all

# 2. Force push (if allowed)
git push origin --force --all

# 3. Rotate credentials immediately
```

## Best Practices Summary

1. **Commit Often**: Small, focused commits
2. **Write Clear Messages**: Follow conventional commits
3. **Keep Branches Updated**: Regular rebase on main
4. **Test Before Pushing**: Ensure quality
5. **Review Your Changes**: Always diff before committing
6. **Use Branches**: Never commit directly to main
7. **Clean History**: Squash WIP commits before PR
8. **Document Breaking Changes**: In commit and PR
9. **Link Issues**: Reference issue numbers
10. **Stay Organized**: Delete merged branches

---

**Remember**: Git is a powerful tool. When in doubt, create a backup branch before destructive operations!
