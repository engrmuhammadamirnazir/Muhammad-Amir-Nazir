---
type: cheatsheet
tags: [commands, regex, patterns, validation, cheatsheet]
created: 2026-03-02
---

# Regex Patterns

---

## Syntax Reference

| Pattern | Meaning |
|---------|---------|
| `.` | Any character (except newline) |
| `\d` | Digit [0-9] |
| `\D` | Non-digit |
| `\w` | Word char [a-zA-Z0-9_] |
| `\W` | Non-word char |
| `\s` | Whitespace |
| `\S` | Non-whitespace |
| `\b` | Word boundary |

---

## Quantifiers

| Pattern | Meaning |
|---------|---------|
| `*` | 0 or more |
| `+` | 1 or more |
| `?` | 0 or 1 |
| `{3}` | Exactly 3 |
| `{3,}` | 3 or more |
| `{3,5}` | 3 to 5 |
| `*?` | 0 or more (lazy) |
| `+?` | 1 or more (lazy) |

---

## Anchors

| Pattern | Meaning |
|---------|---------|
| `^` | Start of string/line |
| `$` | End of string/line |
| `\b` | Word boundary |
| `\B` | Non-word boundary |

---

## Groups & Lookarounds

| Pattern | Meaning |
|---------|---------|
| `(abc)` | Capture group |
| `(?:abc)` | Non-capture group |
| `(?=abc)` | Lookahead |
| `(?!abc)` | Negative lookahead |
| `(?<=abc)` | Lookbehind |
| `(?<!abc)` | Negative lookbehind |
| `\1` | Backreference to group 1 |
| `(a\|b)` | Alternation (a or b) |

---

## Character Classes

| Pattern | Meaning |
|---------|---------|
| `[abc]` | a, b, or c |
| `[^abc]` | NOT a, b, or c |
| `[a-z]` | Lowercase letter |
| `[A-Z]` | Uppercase letter |
| `[0-9]` | Digit |
| `[a-zA-Z]` | Any letter |
| `[a-zA-Z0-9]` | Alphanumeric |

---

## Ready-to-Use Patterns

### Email
```regex
^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$
```

### URL
```regex
https?://(?:www\.)?[-a-zA-Z0-9@:%._+~#=]{1,256}\.[a-zA-Z0-9()]{1,6}\b[-a-zA-Z0-9()@:%_+.~#?&/=]*
```

### IPv4 Address
```regex
\b(?:\d{1,3}\.){3}\d{1,3}\b
```

### IPv6 Address
```regex
(?:[0-9a-fA-F]{1,4}:){7}[0-9a-fA-F]{1,4}
```

### Phone Number (International)
```regex
\+?[\d\s\-().]{7,15}
```

### Phone Number (Pakistan)
```regex
\+?92\d{10}|0\d{10}
```

### Date (YYYY-MM-DD)
```regex
\d{4}-(?:0[1-9]|1[0-2])-(?:0[1-9]|[12]\d|3[01])
```

### Date (DD/MM/YYYY)
```regex
(?:0[1-9]|[12]\d|3[01])/(?:0[1-9]|1[0-2])/\d{4}
```

### Time (HH:MM:SS)
```regex
(?:[01]\d|2[0-3]):[0-5]\d(?::[0-5]\d)?
```

### Username (3-16 chars, alphanumeric + underscore)
```regex
^[a-zA-Z0-9_]{3,16}$
```

### Strong Password (8+ chars, upper, lower, digit, special)
```regex
^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$
```

### Hex Color
```regex
#(?:[0-9a-fA-F]{3}){1,2}\b
```

### HTML Tag
```regex
<\/?[a-z][\s\S]*?>
```

### Zip Code (US)
```regex
\b\d{5}(?:-\d{4})?\b
```

### Credit Card Number
```regex
\b(?:\d{4}[-\s]?){3}\d{4}\b
```

### IBAN
```regex
[A-Z]{2}\d{2}[A-Z0-9]{4}\d{7}(?:[A-Z0-9]?){0,16}
```

### Domain Name
```regex
^(?:[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?\.)+[a-zA-Z]{2,}$
```

### Slug (URL-friendly)
```regex
^[a-z0-9]+(?:-[a-z0-9]+)*$
```

### File Path (Unix)
```regex
^\/(?:[^/\0]+\/)*[^/\0]*$
```

### File Path (Windows)
```regex
^[a-zA-Z]:\\(?:[^\\/:*?"<>|\r\n]+\\)*[^\\/:*?"<>|\r\n]*$
```

### UUID
```regex
[0-9a-f]{8}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{4}-[0-9a-f]{12}
```

### Semantic Version
```regex
\bv?\d+\.\d+\.\d+(?:-[\w.]+)?(?:\+[\w.]+)?\b
```

### JSON Key-Value
```regex
"(\w+)"\s*:\s*"([^"]*)"
```

### Python Import
```regex
^(?:from\s+\S+\s+)?import\s+\S+
```

### Empty Lines
```regex
^\s*$
```

### Duplicate Words
```regex
\b(\w+)\s+\1\b
```

### Whitespace Trim
```regex
^\s+|\s+$
```

### Between Quotes
```regex
(?<=['"])(.*?)(?=['"])
```

---

## Regex in Common Tools

### grep
```bash
grep -E "pattern" file.txt
```

### sed
```bash
sed 's/pattern/replacement/g' file.txt
```

### Python
```python
import re
re.findall(r'pattern', text)
re.sub(r'old', 'new', text)
re.match(r'pattern', text)
```

### JavaScript
```javascript
const regex = /pattern/gi;
text.match(regex);
text.replace(regex, 'new');
```

---

*See also: [[Linux & Bash Commands]] | [[Python Commands]]*
