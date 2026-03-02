---
type: cheatsheet
tags: [commands, vim, editor, cheatsheet]
created: 2026-03-02
---

# Vim Commands

---

## Modes

| Key | Mode |
|-----|------|
| `i` | Insert mode (before cursor) |
| `a` | Insert mode (after cursor) |
| `I` | Insert at beginning of line |
| `A` | Insert at end of line |
| `o` | New line below |
| `O` | New line above |
| `v` | Visual mode (character) |
| `V` | Visual mode (line) |
| `Ctrl+v` | Visual block mode |
| `Esc` | Normal mode |
| `:` | Command mode |

---

## Navigation

| Key | Action |
|-----|--------|
| `h j k l` | Left, Down, Up, Right |
| `w` | Next word start |
| `b` | Previous word start |
| `e` | End of word |
| `0` | Start of line |
| `^` | First non-blank character |
| `$` | End of line |
| `gg` | First line of file |
| `G` | Last line of file |
| `5G` or `:5` | Go to line 5 |
| `Ctrl+d` | Half page down |
| `Ctrl+u` | Half page up |
| `Ctrl+f` | Full page down |
| `Ctrl+b` | Full page up |
| `H` | Top of screen |
| `M` | Middle of screen |
| `L` | Bottom of screen |
| `%` | Matching bracket |
| `{` | Previous paragraph |
| `}` | Next paragraph |

---

## Editing

| Key | Action |
|-----|--------|
| `x` | Delete character |
| `dd` | Delete line |
| `dw` | Delete word |
| `d$` or `D` | Delete to end of line |
| `d0` | Delete to start of line |
| `cc` | Change line |
| `cw` | Change word |
| `c$` or `C` | Change to end of line |
| `yy` | Yank (copy) line |
| `yw` | Yank word |
| `y$` | Yank to end of line |
| `p` | Paste after |
| `P` | Paste before |
| `u` | Undo |
| `Ctrl+r` | Redo |
| `.` | Repeat last command |
| `~` | Toggle case |
| `>>` | Indent line |
| `<<` | Outdent line |
| `J` | Join lines |

---

## Search & Replace

```
/pattern
```
> Search forward

```
?pattern
```
> Search backward

| Key | Action |
|-----|--------|
| `n` | Next match |
| `N` | Previous match |
| `*` | Search word under cursor (forward) |
| `#` | Search word under cursor (backward) |

```
:%s/old/new/g
```
> Replace all in file

```
:%s/old/new/gc
```
> Replace all with confirmation

```
:5,15s/old/new/g
```
> Replace in lines 5-15

```
:%s/old/new/gi
```
> Case insensitive replace

```
:noh
```
> Clear search highlighting

---

## Save & Exit

```
:w
```
> Save

```
:q
```
> Quit

```
:wq
```
> Save and quit

```
:x
```
> Save and quit (same as :wq)

```
ZZ
```
> Save and quit (shortcut)

```
:q!
```
> Quit without saving

```
:wq!
```
> Force save and quit

```
:w filename
```
> Save as

---

## Visual Mode Operations

1. Enter visual mode (`v`, `V`, or `Ctrl+v`)
2. Select text using navigation keys
3. Apply operation:

| Key | Action |
|-----|--------|
| `d` | Delete selection |
| `y` | Yank selection |
| `c` | Change selection |
| `>` | Indent |
| `<` | Outdent |
| `~` | Toggle case |
| `u` | Lowercase |
| `U` | Uppercase |

---

## Windows & Tabs

```
:sp filename
```
> Horizontal split

```
:vsp filename
```
> Vertical split

| Key | Action |
|-----|--------|
| `Ctrl+w s` | Horizontal split |
| `Ctrl+w v` | Vertical split |
| `Ctrl+w w` | Switch window |
| `Ctrl+w h/j/k/l` | Navigate windows |
| `Ctrl+w q` | Close window |
| `Ctrl+w =` | Equal size windows |

```
:tabnew filename
```
> New tab

```
:tabn
```
> Next tab

```
:tabp
```
> Previous tab

---

## Marks & Macros

```
ma
```
> Set mark 'a' at cursor

```
'a
```
> Jump to mark 'a'

```
qa
```
> Start recording macro 'a'

```
q
```
> Stop recording

```
@a
```
> Play macro 'a'

```
10@a
```
> Play macro 'a' 10 times

---

## Useful Commands

```
:set number
```
> Show line numbers

```
:set relativenumber
```

```
:set paste
```
> Paste mode (no auto-indent)

```
:set nopaste
```

```
:!command
```
> Execute shell command

```
:r !command
```
> Insert command output

```
:r filename
```
> Insert file contents

---

*See also: [[VS Code Shortcuts]] | [[tmux Commands]]*
