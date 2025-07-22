# 2️⃣ Git & GitHub Interview Questions

## 🔀 Core Concepts

### Q1: Explain the difference between Git and GitHub
**Answer:**  
Git is a distributed version control system that tracks changes to source code. GitHub is a cloud-based hosting service for Git repositories with additional collaboration features like:
- Pull requests
- Issue tracking
- CI/CD integrations
- Project management tools

### Q2: What is a .gitignore file? Give examples of when you'd use it
**Answer:**  
A `.gitignore` file specifies intentionally untracked files that Git should ignore. Common use cases:
```gitignore
# Ignore OS files
.DS_Store
Thumbs.db

# Ignore IDE files
.idea/
.vscode/

# Ignore dependencies
node_modules/
.env
```

### Q3: Describe Git's three-stage architecture
**Answer:**  
1. **Working Directory**: Where you make changes to files
2. **Staging Area (Index)**: Where you prepare changes for commit (`git add`)
3. **Repository**: Where Git permanently stores changes as commits (`git commit`)

## 🌿 Branching Strategies

### Q4: Compare Git Flow vs. Trunk-Based Development
| Feature          | Git Flow                  | Trunk-Based Development       |
|------------------|---------------------------|-------------------------------|
| Branches         | Long-lived `dev`, `main`  | Short-lived feature branches  |
| Release Cycle    | Scheduled                 | Continuous                    |
| Complexity      | Higher                    | Lower                         |
| Best For        | Versioned products        | CI/CD environments            |

### Q5: How to rename a branch locally and remotely?
```bash
# Rename locally
git branch -m old-name new-name

# Push new branch and delete old
git push origin :old-name new-name

# Set upstream
git push origin -u new-name
```

### Q6: Explain fast-forward merge vs. three-way merge
**Answer:**  
- **Fast-forward**: When there's a linear path (no diverged history)
- **Three-way**: When branches have diverged (creates merge commit)

## 🛠️ Conflict Resolution

### Q7: Steps to resolve a merge conflict
1. Identify conflicts:
   ```bash
   git status
   ```
2. Open files and resolve `<<<<<<<`, `=======`, `>>>>>>>` markers
3. Mark as resolved:
   ```bash
   git add conflicted_file.js
   ```
4. Complete the merge:
   ```bash
   git commit
   ```

### Q8: How to undo the last commit without losing changes?
```bash
git reset --soft HEAD~1
# Changes remain staged
```

## ⏪ Undoing Changes Cheatsheet

| Command                          | Use Case                                  |
|----------------------------------|------------------------------------------|
| `git revert <commit>`            | Safe undo for public commits             |
| `git reset --hard HEAD~1`        | Completely discard last commit           |
| `git checkout -- <file>`         | Discard uncommitted file changes         |
| `git clean -df`                  | Remove untracked files/dirs              |

## 🔗 Advanced Scenarios

### Q9: Difference between `git pull --rebase` and regular pull
**Answer:**  
- Regular `pull`: Creates merge commit
- `pull --rebase`: Rewrites local commits on top of remote changes (cleaner history)

### Q10: How to squash multiple commits into one?
```bash
git rebase -i HEAD~3
# In interactive editor, mark commits with 'squash' or 'fixup'
```

## 🧪 Hands-On Lab

**Scenario**: Team member pushed to main branch by mistake  
**Task**: Revert their commit while preserving your changes
```bash
# Fetch latest changes
git fetch origin

# Create revert commit
git revert <bad-commit-hash>

# Push the fix
git push origin main
```

📌 **Pro Tip**: Always pull before pushing to avoid conflicts!
```bash
git pull --rebase origin main
```

[⬆ Back to Main README](../README.md) | [💡 Practice Exercises](./exercises/)
```
