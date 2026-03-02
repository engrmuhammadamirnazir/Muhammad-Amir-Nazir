---
type: resource
tags: [commands, windows, cmd, reference]
created: 2026-03-02
updated: 2026-03-02
---

# Windows Commands

> Essential Windows CMD commands for system administration.

---

## File Operations

```cmd
dir                          # List directory contents
dir /s /b *.txt              # Recursive search for .txt files
cd [path]                    # Change directory
mkdir [name]                 # Create directory
rmdir /s /q [name]           # Delete directory recursively
del [file]                   # Delete file
copy [src] [dst]             # Copy file
move [src] [dst]             # Move/rename
xcopy /E /I [src] [dst]     # Copy directory tree
robocopy [src] [dst] /MIR   # Mirror directory (better than xcopy)
type [file]                  # Display file contents
```

---

## System Information

```cmd
systeminfo                   # Full system information
hostname                     # Computer name
whoami                       # Current user
ver                          # Windows version
wmic os get caption          # OS name
```

---

## Process Management

```cmd
tasklist                     # List running processes
tasklist /fi "imagename eq node.exe"  # Filter by name
taskkill /PID [pid] /F       # Kill by PID
taskkill /IM node.exe /F     # Kill by name
```

---

## Networking

```cmd
ipconfig                     # Network configuration
ipconfig /all                # Detailed network info
ipconfig /flushdns           # Flush DNS cache
ping [host]                  # Test connectivity
tracert [host]               # Trace route
netstat -ano                 # Active connections with PIDs
netstat -ano | findstr :3000 # Find process on port 3000
nslookup [domain]            # DNS lookup
```

---

## Disk & Storage

```cmd
diskpart                     # Disk management utility
chkdsk C: /f                 # Check and fix disk errors
wmic diskdrive get size      # Disk sizes
```

---

## Services

```cmd
sc query                     # List all services
sc query [service]           # Status of specific service
net start [service]          # Start service
net stop [service]           # Stop service
sc config [service] start=auto  # Set to auto-start
```

---

## Environment Variables

```cmd
set                          # List all env vars
set PATH                     # Show PATH
setx VAR "value"             # Set persistent env var
echo %PATH%                  # Echo specific var
```

---

## Useful Shortcuts

```cmd
cls                          # Clear screen
exit                         # Close terminal
start [url/file]             # Open file or URL
findstr /r "pattern" file    # Regex search in file (like grep)
where node                   # Find executable path (like which)
```

---

## Related

- [[PowerShell Commands]] — Windows PowerShell (more powerful)
- [[Linux & Bash Commands]] — Unix/Linux equivalent
- [[MOC - Command Cheatsheets]] — All cheatsheets
