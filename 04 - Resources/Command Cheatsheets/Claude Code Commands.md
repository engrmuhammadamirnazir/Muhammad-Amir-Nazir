---
type: cheatsheet
tags: [commands, claude, ai, cli, cheatsheet]
created: 2026-03-02
---

# Claude Code Commands

---

## Installation & Setup

```bash
npm install -g @anthropic-ai/claude-code
```

```bash
claude
```
> Start interactive session

```bash
claude --version
```

```bash
claude update
```

---

## Basic Usage

```bash
claude
```
> Interactive mode

```bash
claude "explain this code"
```
> One-shot with prompt

```bash
claude -p "what does this function do"
```
> Print mode (no interactive)

```bash
claude --continue
```
> Continue last conversation

```bash
claude --resume
```
> Pick a conversation to resume

---

## Authentication

```bash
claude auth login
```

```bash
claude auth logout
```

```bash
claude auth status
```

---

## Model Selection

```bash
claude --model sonnet
```

```bash
claude --model opus
```

```bash
claude --model haiku
```

---

## System Prompts

```bash
claude --system-prompt "You are a Python expert"
```

```bash
claude --append-system-prompt "Always use TypeScript"
```

```bash
claude --system-prompt-file ./prompt.txt
```

---

## Input/Output

```bash
cat file.py | claude -p "review this code"
```
> Pipe input

```bash
claude -p "generate a README" > README.md
```
> Output to file

```bash
cat error.log | claude -p "explain these errors"
```

```bash
git diff | claude -p "write a commit message"
```

---

## Permissions & Safety

```bash
claude --dangerously-skip-permissions
```
> Skip all permission prompts (use with caution)

```bash
claude --allowedTools "Read,Write,Bash"
```

---

## Advanced

```bash
claude --verbose
```

```bash
claude --debug
```

```bash
claude --max-turns 10
```

```bash
claude remote-control --dangerously-skip-permissions
```
> Remote control mode

---

## Slash Commands (Inside Session)

| Command | Action |
|---------|--------|
| `/help` | Show help |
| `/clear` | Clear conversation |
| `/compact` | Compact conversation history |
| `/cost` | Show token usage and cost |
| `/doctor` | Check installation health |
| `/init` | Initialize CLAUDE.md for project |
| `/login` | Switch account |
| `/logout` | Sign out |
| `/model` | Switch model |
| `/permissions` | View/modify permissions |
| `/review` | Review code changes |
| `/status` | Show session info |
| `/fast` | Toggle fast mode |
| `/commit` | Create a git commit |

---

## Configuration

```bash
claude config
```

```bash
claude config set theme dark
```

### CLAUDE.md
Create a `CLAUDE.md` file in your project root for project-specific instructions:

```markdown
# Project Instructions

## Build & Test
- Run tests: `npm test`
- Build: `npm run build`

## Code Style
- Use TypeScript strict mode
- Prefer functional components

## Important
- Never modify database migrations directly
```

---

## MCP Servers

```bash
claude mcp add server-name -- command args
```

```bash
claude mcp list
```

```bash
claude mcp remove server-name
```

---

*See also: [[Git Commands]] | [[GitHub CLI Commands]]*
