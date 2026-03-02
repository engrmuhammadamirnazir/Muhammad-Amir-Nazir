---
type: cheatsheet
tags: [commands, vscode, editor, shortcuts, cheatsheet]
created: 2026-03-02
---

# VS Code Shortcuts

---

## General

| Shortcut | Action |
|----------|--------|
| `Ctrl+Shift+P` | Command Palette |
| `Ctrl+P` | Quick Open file |
| `Ctrl+,` | Settings |
| `Ctrl+K Ctrl+S` | Keyboard Shortcuts |
| `Ctrl+backtick` | Toggle Terminal |
| `Ctrl+B` | Toggle Sidebar |
| `Ctrl+Shift+E` | Explorer |
| `Ctrl+Shift+F` | Search across files |
| `Ctrl+Shift+G` | Source Control |
| `Ctrl+Shift+D` | Debug |
| `Ctrl+Shift+X` | Extensions |

---

## Editing

| Shortcut | Action |
|----------|--------|
| `Ctrl+C` | Copy line (no selection) |
| `Ctrl+X` | Cut line (no selection) |
| `Ctrl+Shift+K` | Delete line |
| `Alt+Up/Down` | Move line up/down |
| `Shift+Alt+Up/Down` | Copy line up/down |
| `Ctrl+Enter` | Insert line below |
| `Ctrl+Shift+Enter` | Insert line above |
| `Ctrl+D` | Select next occurrence |
| `Ctrl+Shift+L` | Select all occurrences |
| `Ctrl+H` | Find and replace |
| `Ctrl+/` | Toggle line comment |
| `Shift+Alt+A` | Toggle block comment |
| `Ctrl+]` | Indent line |
| `Ctrl+[` | Outdent line |
| `Ctrl+Z` | Undo |
| `Ctrl+Shift+Z` | Redo |
| `Ctrl+Shift+\` | Jump to matching bracket |
| `Tab` | Indent |
| `Shift+Tab` | Outdent |

---

## Multi-Cursor

| Shortcut | Action |
|----------|--------|
| `Alt+Click` | Add cursor |
| `Ctrl+Alt+Up/Down` | Add cursor above/below |
| `Ctrl+D` | Add next occurrence to selection |
| `Ctrl+Shift+L` | Add cursors to all occurrences |
| `Ctrl+U` | Undo last cursor |
| `Esc` | Cancel multi-cursor |

---

## Navigation

| Shortcut | Action |
|----------|--------|
| `Ctrl+G` | Go to line |
| `Ctrl+P` | Go to file |
| `Ctrl+Shift+O` | Go to symbol in file |
| `Ctrl+T` | Go to symbol in workspace |
| `F12` | Go to definition |
| `Alt+F12` | Peek definition |
| `Shift+F12` | Show references |
| `Ctrl+Shift+\` | Go to matching bracket |
| `Alt+Left/Right` | Navigate back/forward |
| `Ctrl+Tab` | Switch between open files |
| `Ctrl+Shift+M` | Problems panel |

---

## Selection

| Shortcut | Action |
|----------|--------|
| `Ctrl+L` | Select entire line |
| `Shift+Alt+Right` | Expand selection |
| `Shift+Alt+Left` | Shrink selection |
| `Ctrl+Shift+[` | Fold region |
| `Ctrl+Shift+]` | Unfold region |
| `Ctrl+K Ctrl+0` | Fold all |
| `Ctrl+K Ctrl+J` | Unfold all |

---

## Terminal

| Shortcut | Action |
|----------|--------|
| `` Ctrl+` `` | Toggle terminal |
| `` Ctrl+Shift+` `` | New terminal |
| `Ctrl+Shift+5` | Split terminal |
| `Ctrl+PgUp/PgDn` | Switch terminal tabs |

---

## Split Editor

| Shortcut | Action |
|----------|--------|
| `Ctrl+\` | Split editor |
| `Ctrl+1/2/3` | Focus editor group |
| `Ctrl+K Ctrl+Left/Right` | Move editor to group |

---

## VS Code CLI

```bash
code .
```
> Open current directory

```bash
code file.txt
```
> Open file

```bash
code --diff file1.txt file2.txt
```
> Diff two files

```bash
code --goto file.txt:10:5
```
> Open file at line 10, column 5

```bash
code --install-extension ms-python.python
```

```bash
code --uninstall-extension extension-id
```

```bash
code --list-extensions
```

```bash
code --disable-extensions
```
> Open with all extensions disabled

```bash
code --reuse-window .
```

```bash
code --new-window .
```

---

*See also: [[Vim Commands]] | [[tmux Commands]]*
