---
type: cheatsheet
tags: [commands, git, version-control, cheatsheet]
created: 2026-03-02
---

# Git Commands

---

## Setup & Config

```bash
git config --global user.name "Your Name"
```

```bash
git config --global user.email "you@example.com"
```

```bash
git config --global init.defaultBranch main
```

```bash
git config --global core.editor "code --wait"
```

```bash
git config --list
```

---

## Create & Clone

```bash
git init
```

```bash
git clone https://github.com/user/repo.git
```

```bash
git clone --depth 1 --branch 18.0 --single-branch https://github.com/user/repo.git
```

```bash
git clone --bare https://github.com/user/repo.git
```

---

## Stage & Commit

```bash
git status
```

```bash
git add .
```

```bash
git add file.py
```

```bash
git add -p
```
> Interactive staging — choose which hunks to stage

```bash
git commit -m "Your commit message"
```

```bash
git commit -am "Stage tracked files and commit"
```

```bash
git commit --amend -m "Updated commit message"
```

```bash
git commit --amend --no-edit
```
> Add staged changes to last commit without changing message

---

## Branches

```bash
git branch
```

```bash
git branch -a
```
> List all branches (local + remote)

```bash
git branch new-branch
```

```bash
git checkout branch-name
```

```bash
git checkout -b new-branch
```
> Create and switch to new branch

```bash
git switch branch-name
```

```bash
git switch -c new-branch
```

```bash
git branch -d branch-name
```
> Delete branch (safe — won't delete unmerged)

```bash
git branch -D branch-name
```
> Force delete branch

```bash
git push origin --delete branch-name
```
> Delete remote branch

```bash
git branch -m old-name new-name
```
> Rename branch

---

## Merge & Rebase

```bash
git merge branch-name
```

```bash
git merge --no-ff branch-name
```
> Merge with explicit merge commit

```bash
git merge --squash branch-name
```
> Squash all commits into one before merging

```bash
git rebase main
```

```bash
git rebase -i HEAD~3
```
> Interactive rebase last 3 commits (squash, edit, reorder)

```bash
git rebase --abort
```

```bash
git rebase --continue
```

```bash
git cherry-pick commit-hash
```

```bash
git cherry-pick commit1 commit2 commit3
```

---

## Remote

```bash
git remote -v
```

```bash
git remote add origin https://github.com/user/repo.git
```

```bash
git remote set-url origin https://github.com/user/new-repo.git
```

```bash
git remote remove origin
```

```bash
git fetch origin
```

```bash
git fetch --all
```

```bash
git pull origin main
```

```bash
git pull --rebase origin main
```

```bash
git push origin main
```

```bash
git push origin 18.0
```

```bash
git push -u origin new-branch
```
> Push and set upstream tracking

```bash
git push --force-with-lease
```
> Safer force push — fails if remote has new commits

```bash
git push origin --tags
```

---

## Stash

```bash
git stash
```

```bash
git stash push -m "description"
```

```bash
git stash list
```

```bash
git stash pop
```
> Apply latest stash and remove it

```bash
git stash apply
```
> Apply latest stash but keep it

```bash
git stash apply stash@{2}
```

```bash
git stash drop stash@{0}
```

```bash
git stash clear
```

```bash
git stash show -p stash@{0}
```
> Show stash diff

```bash
git stash branch new-branch stash@{0}
```
> Create branch from stash

---

## Log & History

```bash
git log --oneline
```

```bash
git log --oneline --graph --all
```

```bash
git log --author="name"
```

```bash
git log --since="2025-01-01" --until="2025-12-31"
```

```bash
git log --stat
```

```bash
git log -p -3
```
> Show last 3 commits with full diff

```bash
git log --pretty=format:"%h %an %ar - %s"
```

```bash
git shortlog -sn
```
> Commit count per author

```bash
git blame file.py
```

```bash
git show commit-hash
```

---

## Diff

```bash
git diff
```
> Unstaged changes

```bash
git diff --staged
```
> Staged changes

```bash
git diff branch1..branch2
```

```bash
git diff HEAD~3..HEAD
```

```bash
git diff --stat
```
> Summary only

```bash
git diff --name-only
```

---

## Undo & Reset

```bash
git restore file.py
```
> Discard unstaged changes

```bash
git restore --staged file.py
```
> Unstage file

```bash
git reset HEAD~1
```
> Undo last commit, keep changes unstaged

```bash
git reset --soft HEAD~1
```
> Undo last commit, keep changes staged

```bash
git reset --hard HEAD~1
```
> Undo last commit and discard all changes (DANGEROUS)

```bash
git revert commit-hash
```
> Create new commit that undoes a specific commit (safe)

```bash
git checkout -- file.py
```
> Discard changes in file

---

## Tags

```bash
git tag v1.0.0
```

```bash
git tag -a v1.0.0 -m "Release 1.0.0"
```

```bash
git tag -l "v1.*"
```

```bash
git push origin v1.0.0
```

```bash
git push origin --tags
```

```bash
git tag -d v1.0.0
```

```bash
git push origin --delete v1.0.0
```

---

## Submodules

```bash
git submodule add https://github.com/user/repo.git path/to/submodule
```

```bash
git submodule update --init --recursive
```

```bash
git submodule foreach git pull origin main
```

```bash
git submodule deinit path/to/submodule
```

---

## Worktree

```bash
git worktree add ../feature-branch feature-branch
```

```bash
git worktree list
```

```bash
git worktree remove ../feature-branch
```

---

## Advanced

```bash
git bisect start
```
> Binary search for bug-introducing commit

```bash
git bisect bad
```

```bash
git bisect good commit-hash
```

```bash
git bisect reset
```

```bash
git reflog
```
> View all HEAD movements (recover lost commits)

```bash
git clean -fd
```
> Remove untracked files and directories

```bash
git clean -fdn
```
> Dry run — show what would be removed

```bash
git gc --prune=now
```
> Garbage collect and optimize repo

```bash
git archive --format=zip HEAD -o archive.zip
```

---

## Common Workflows

### Feature Branch
```bash
git checkout -b feature/my-feature
# ... make changes ...
git add .
git commit -m "Add my feature"
git push -u origin feature/my-feature
```

### Sync Fork with Upstream
```bash
git remote add upstream https://github.com/original/repo.git
git fetch upstream
git merge upstream/main
git push origin main
```

### Undo a Pushed Commit
```bash
git revert commit-hash
git push origin main
```

---

*See also: [[GitHub CLI Commands]] | [[Odoo Development Commands]]*
