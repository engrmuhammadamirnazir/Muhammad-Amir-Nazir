---
type: cheatsheet
tags: [commands, github, gh, cli, cheatsheet]
created: 2026-03-02
---

# GitHub CLI (gh) Commands

---

## Authentication

```bash
gh auth login
```

```bash
gh auth status
```

```bash
gh auth logout
```

```bash
gh auth refresh
```

---

## Repositories

```bash
gh repo clone user/repo
```

```bash
gh repo create myrepo --public
```

```bash
gh repo create myrepo --private
```

```bash
gh repo create myrepo --public --source=. --remote=origin --push
```
> Create from existing local repo

```bash
gh repo fork user/repo
```

```bash
gh repo view
```

```bash
gh repo view user/repo --web
```
> Open in browser

```bash
gh repo list
```

```bash
gh repo list --limit 50
```

```bash
gh repo delete user/repo --yes
```

```bash
gh repo sync
```
> Sync fork with upstream

---

## Pull Requests

```bash
gh pr create --title "My PR" --body "Description"
```

```bash
gh pr create --fill
```
> Auto-fill from commits

```bash
gh pr create --draft
```

```bash
gh pr create --base main --head feature-branch
```

```bash
gh pr list
```

```bash
gh pr list --state open
```

```bash
gh pr list --author @me
```

```bash
gh pr view 123
```

```bash
gh pr view 123 --web
```

```bash
gh pr checkout 123
```
> Checkout PR branch locally

```bash
gh pr merge 123
```

```bash
gh pr merge 123 --squash
```

```bash
gh pr merge 123 --rebase
```

```bash
gh pr merge 123 --auto --squash
```
> Auto-merge when checks pass

```bash
gh pr close 123
```

```bash
gh pr reopen 123
```

```bash
gh pr diff 123
```

```bash
gh pr review 123 --approve
```

```bash
gh pr review 123 --request-changes --body "Please fix X"
```

```bash
gh pr checks 123
```
> View CI status

```bash
gh pr comment 123 --body "LGTM!"
```

---

## Issues

```bash
gh issue create --title "Bug report" --body "Description"
```

```bash
gh issue create --label bug --assignee @me
```

```bash
gh issue list
```

```bash
gh issue list --state open --label bug
```

```bash
gh issue view 456
```

```bash
gh issue close 456
```

```bash
gh issue reopen 456
```

```bash
gh issue comment 456 --body "Working on this"
```

```bash
gh issue edit 456 --add-label "priority"
```

---

## GitHub Actions

```bash
gh workflow list
```

```bash
gh workflow run workflow-name.yml
```

```bash
gh workflow run workflow-name.yml --ref branch-name
```

```bash
gh workflow enable workflow-name.yml
```

```bash
gh workflow disable workflow-name.yml
```

```bash
gh run list
```

```bash
gh run view 12345
```

```bash
gh run watch 12345
```
> Watch run in real-time

```bash
gh run rerun 12345
```

```bash
gh run cancel 12345
```

```bash
gh run download 12345
```
> Download artifacts

---

## Releases

```bash
gh release create v1.0.0
```

```bash
gh release create v1.0.0 --title "Release 1.0.0" --notes "Release notes"
```

```bash
gh release create v1.0.0 ./dist/*.zip
```
> Create with assets

```bash
gh release list
```

```bash
gh release view v1.0.0
```

```bash
gh release download v1.0.0
```

---

## Gists

```bash
gh gist create file.py
```

```bash
gh gist create file.py --public
```

```bash
gh gist create file1.py file2.py --desc "My gist"
```

```bash
gh gist list
```

```bash
gh gist view gist-id
```

```bash
gh gist edit gist-id
```

---

## API

```bash
gh api repos/user/repo
```

```bash
gh api repos/user/repo/pulls/123/comments
```

```bash
gh api -X POST repos/user/repo/issues -f title="Bug" -f body="Description"
```

---

*See also: [[Git Commands]]*
