# Git Commands Cheatsheet

## Setup
```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --list
```

## Create & Clone
```bash
git init                        # Initialize new repo
git clone https://github.com/user/repo.git  # Clone
git clone repo.git my-folder   # Clone into specific folder
```

## Basic Workflow
```bash
git status                      # Check status
git add .                       # Stage all changes
git add file.txt                # Stage specific file
git commit -m "Your message"   # Commit
git commit -am "message"        # Stage + commit tracked files
git push origin main            # Push to remote
git pull origin main            # Pull from remote
```

## Branches
```bash
git branch                      # List branches
git branch feature-x            # Create branch
git checkout feature-x          # Switch branch
git checkout -b feature-x       # Create + switch
git switch main                 # Switch (modern way)
git merge feature-x             # Merge into current
git branch -d feature-x        # Delete branch (safe)
git branch -D feature-x        # Force delete
git push origin feature-x       # Push branch to remote
git push origin --delete feature-x  # Delete remote branch
```

## Undo & Reset
```bash
git restore file.txt            # Discard unstaged changes
git restore --staged file.txt   # Unstage file
git revert HEAD                 # Undo last commit (safe)
git reset --soft HEAD~1        # Undo commit, keep changes staged
git reset --mixed HEAD~1       # Undo commit, keep changes unstaged
git reset --hard HEAD~1        # Undo commit, DISCARD changes ⚠️
```

## Stash
```bash
git stash                       # Save uncommitted changes
git stash pop                   # Apply stash and remove
git stash apply                 # Apply stash and keep
git stash list                  # List all stashes
git stash drop stash@{0}        # Remove specific stash
```

## View History
```bash
git log                         # Full log
git log --oneline               # One line per commit
git log --oneline --graph       # Graph view
git diff                        # Unstaged changes
git diff --staged               # Staged changes
git show HEAD                   # Show last commit
```

## Remote
```bash
git remote -v                   # List remotes
git remote add origin https://... # Add remote
git remote set-url origin https://... # Update remote URL
git fetch origin                # Fetch without merging
```

## Tags
```bash
git tag v1.0.0                  # Create tag
git tag -a v1.0.0 -m "Release" # Annotated tag
git push origin v1.0.0         # Push tag
git push origin --tags          # Push all tags
```

## .gitignore Common Patterns
```
node_modules/
.env
.env.local
dist/
build/
*.log
.DS_Store
.vscode/
*.pyc
__pycache__/
```
