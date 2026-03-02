---
type: cheatsheet
tags: [commands, tmux, terminal, multiplexer, cheatsheet]
created: 2026-03-02
---

# tmux Commands

> Prefix key: `Ctrl+b` (press prefix, release, then press the next key)

---

## Sessions

```bash
tmux
```
> New session

```bash
tmux new -s mysession
```
> New named session

```bash
tmux ls
```
> List sessions

```bash
tmux attach -t mysession
```
> Attach to session

```bash
tmux kill-session -t mysession
```

```bash
tmux kill-server
```
> Kill all sessions

| Shortcut | Action |
|----------|--------|
| `Prefix d` | Detach from session |
| `Prefix s` | List/switch sessions |
| `Prefix $` | Rename session |
| `Prefix (` | Previous session |
| `Prefix )` | Next session |

---

## Windows

| Shortcut | Action |
|----------|--------|
| `Prefix c` | New window |
| `Prefix ,` | Rename window |
| `Prefix n` | Next window |
| `Prefix p` | Previous window |
| `Prefix 0-9` | Switch to window # |
| `Prefix w` | List windows |
| `Prefix &` | Kill window |
| `Prefix l` | Last active window |

---

## Panes

| Shortcut | Action |
|----------|--------|
| `Prefix %` | Split vertical |
| `Prefix "` | Split horizontal |
| `Prefix arrow` | Navigate panes |
| `Prefix o` | Next pane |
| `Prefix z` | Toggle pane zoom (fullscreen) |
| `Prefix x` | Kill pane |
| `Prefix q` | Show pane numbers |
| `Prefix q 0-9` | Switch to pane # |
| `Prefix {` | Swap pane left |
| `Prefix }` | Swap pane right |
| `Prefix Space` | Toggle pane layouts |

### Resize Panes
| Shortcut | Action |
|----------|--------|
| `Prefix Ctrl+arrow` | Resize by 1 |
| `Prefix Alt+arrow` | Resize by 5 |

---

## Copy Mode

| Shortcut | Action |
|----------|--------|
| `Prefix [` | Enter copy mode |
| `q` | Exit copy mode |
| `Space` | Start selection |
| `Enter` | Copy selection |
| `Prefix ]` | Paste buffer |

### In Copy Mode (vi keys)
| Key | Action |
|-----|--------|
| `h j k l` | Navigate |
| `/` | Search forward |
| `?` | Search backward |
| `n` | Next match |
| `N` | Previous match |

---

## Commands

```bash
tmux source-file ~/.tmux.conf
```
> Reload config

```bash
tmux list-keys
```
> List all keybindings

```bash
tmux display-message -p "#{session_name}"
```

---

## Useful .tmux.conf

```bash
# Mouse support
set -g mouse on

# Start window numbering at 1
set -g base-index 1
setw -g pane-base-index 1

# Vi keys in copy mode
setw -g mode-keys vi

# Better split keys
bind | split-window -h
bind - split-window -v

# Reload config shortcut
bind r source-file ~/.tmux.conf \; display "Reloaded!"
```

---

*See also: [[Linux & Bash Commands]] | [[Vim Commands]]*
