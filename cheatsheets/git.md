# Git Commands Cheatsheet

## Initial Setup
```bash
git config --global user.name  "Your Name"
git config --global user.email "you@example.com"
git config --global core.editor "code --wait"  # VS Code as editor
git config --global init.defaultBranch main
git config --list
```

## Create & Clone
```bash
git init                                     # New repo in current folder
git init my-project                          # New repo in new folder
git clone https://github.com/user/repo.git   # Clone
git clone repo.git my-folder                 # Clone into custom folder
git clone --depth 1 url                      # Shallow clone (faster)
```

## Basic Workflow
```bash
git status                   # Check staged / unstaged / untracked
git status -s                # Short format

git add file.txt             # Stage specific file
git add .                    # Stage all changes
git add -p                   # Stage interactively (chunk by chunk)

git commit -m "message"      # Commit staged
git commit -am "message"     # Stage tracked files + commit
git commit --amend           # Edit last commit message or add missed file

git push origin main         # Push to remote
git push -u origin main      # Set upstream (first push)
git push --force-with-lease  # Force push safely (checks remote)

git pull origin main         # Fetch + merge
git pull --rebase            # Fetch + rebase (cleaner history)
```

## Branches
```bash
git branch                   # List local branches
git branch -a                # List all (local + remote)
git branch feature-x         # Create branch
git switch feature-x         # Switch (modern)
git switch -c feature-x      # Create + switch (modern)
git checkout -b feature-x    # Create + switch (classic)

git merge feature-x          # Merge into current branch
git merge --no-ff feature-x  # Merge with commit (no fast-forward)
git rebase main              # Rebase current branch onto main

git branch -d feature-x      # Delete (safe — only if merged)
git branch -D feature-x      # Force delete
git push origin --delete feature-x  # Delete remote branch
git branch -m old new        # Rename branch
```

## Undo & Reset
```bash
# Unstaged changes
git restore file.txt          # Discard changes in working tree
git restore .                 # Discard all unstaged changes ⚠️

# Staged changes
git restore --staged file.txt # Unstage (keep working changes)

# Commits
git revert HEAD               # New commit that undoes last commit (safe)
git revert HEAD~3..HEAD       # Revert last 3 commits

git reset --soft  HEAD~1      # Undo commit → keep changes staged
git reset --mixed HEAD~1      # Undo commit → keep changes unstaged (default)
git reset --hard  HEAD~1      # Undo commit → DISCARD changes ⚠️

# Specific file to previous state
git checkout HEAD~1 -- file.txt
```

## Stash
```bash
git stash                     # Stash uncommitted changes
git stash push -m "WIP: auth" # Stash with message
git stash -u                  # Include untracked files
git stash list                # List all stashes
git stash pop                 # Apply last stash + remove it
git stash apply stash@{1}     # Apply specific stash, keep it
git stash drop stash@{0}      # Delete specific stash
git stash clear               # Delete all stashes
git stash show -p             # Show stash diff
```

## View History
```bash
git log                       # Full history
git log --oneline             # One line per commit
git log --oneline --graph --all  # Visual branch graph
git log --author="Ahmad"      # Filter by author
git log --since="2 weeks ago"
git log -- file.txt           # History of specific file

git show HEAD                 # Show last commit details
git show abc1234              # Show specific commit

git diff                      # Unstaged changes
git diff --staged             # Staged changes
git diff main..feature-x      # Diff between branches

git blame file.txt            # Who changed each line
git log -S "search_term"      # Find commits that added/removed text
```

## Remote
```bash
git remote -v                          # List remotes
git remote add origin https://...      # Add remote
git remote set-url origin https://...  # Update remote URL
git remote remove origin               # Remove remote
git fetch origin                       # Fetch without merging
git fetch --all                        # Fetch all remotes
```

## Tags
```bash
git tag                        # List tags
git tag v1.0.0                 # Lightweight tag
git tag -a v1.0.0 -m "Release v1.0.0"  # Annotated tag
git push origin v1.0.0         # Push specific tag
git push origin --tags          # Push all tags
git tag -d v1.0.0               # Delete local tag
git push origin --delete v1.0.0 # Delete remote tag
```

## .gitignore Examples
```
# Dependencies
node_modules/
vendor/

# Environment
.env
.env.local
.env.production

# Build output
dist/
build/
*.min.js

# Logs
*.log
npm-debug.log*

# OS
.DS_Store
Thumbs.db
.vscode/
.idea/

# Language specific
*.pyc
__pycache__/
*.class
.gradle/
```
