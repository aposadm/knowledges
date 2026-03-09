# Git Command Reference

A handy reference for the most essential Git commands.

---

## Setup

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global credential.helper store
```

---

## Start a Repo

```bash
git init          # new local repo
git clone <url>   # clone remote repo
```

---

## Daily Workflow

```bash
git status                  # check changes
git add <file>              # stage a file
git add .                   # stage all changes
git commit -m "message"     # commit staged changes
git push                    # push to remote
git pull                    # fetch + merge from remote
```

---

## Branches

```bash
git branch                  # list branches
git branch <name>           # create branch
git checkout <name>         # switch branch
git checkout -b <name>      # create + switch
git merge <branch>          # merge into current
git branch -d <name>        # delete branch
```

---

## History & Diffs

```bash
git log                     # commit history
git log --oneline           # compact history
git diff                    # unstaged changes
git diff --staged           # staged changes
```

---

## Undo

```bash
git restore <file>          # discard working dir changes
git reset HEAD <file>       # unstage a file
git revert <commit>         # undo a commit (safe)
git reset --hard <commit>   # reset to commit (destructive)
```

---

## Remote

```bash
git remote -v               # list remotes
git remote add origin <url> # add remote
git fetch                   # fetch without merging
git remote set-url origin https://github.com/user/repo.git
```

