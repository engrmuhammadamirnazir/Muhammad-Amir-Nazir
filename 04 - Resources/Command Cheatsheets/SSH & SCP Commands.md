---
type: cheatsheet
tags: [commands, ssh, scp, remote, cheatsheet]
created: 2026-03-02
---

# SSH & SCP Commands

---

## Basic Connection

```bash
ssh user@hostname
```

```bash
ssh user@192.168.1.100
```

```bash
ssh -p 2222 user@hostname
```
> Connect on custom port

```bash
ssh -i ~/.ssh/mykey.pem user@hostname
```
> Connect with specific key file

```bash
ssh -i "C:\Users\Amir Nazir\OneDrive\Documents\xrero.pem" root@134.209.250.80
```
> Windows path with PEM key

```bash
ssh root@hostname
```

---

## SSH Key Management

```bash
ssh-keygen -t ed25519 -C "your@email.com"
```
> Generate Ed25519 key (recommended)

```bash
ssh-keygen -t rsa -b 4096 -C "your@email.com"
```
> Generate RSA 4096-bit key

```bash
ssh-keygen -t ed25519 -f ~/.ssh/custom_key
```
> Generate with custom filename

```bash
ssh-copy-id user@hostname
```
> Copy public key to remote server

```bash
ssh-copy-id -i ~/.ssh/custom_key.pub user@hostname
```
> Copy specific key

```bash
cat ~/.ssh/id_ed25519.pub
```
> View your public key

---

## SSH Agent

```bash
eval "$(ssh-agent -s)"
```
> Start SSH agent

```bash
ssh-add ~/.ssh/id_ed25519
```
> Add key to agent

```bash
ssh-add -l
```
> List keys in agent

```bash
ssh-add -D
```
> Remove all keys from agent

---

## SCP (Secure Copy)

```bash
scp file.txt user@hostname:/remote/path/
```
> Upload file to remote

```bash
scp user@hostname:/remote/file.txt ./local/path/
```
> Download file from remote

```bash
scp -r directory/ user@hostname:/remote/path/
```
> Upload directory recursively

```bash
scp -r user@hostname:/remote/dir/ ./local/path/
```
> Download directory recursively

```bash
scp -P 2222 file.txt user@hostname:/remote/path/
```
> SCP on custom port

```bash
scp -i ~/.ssh/mykey.pem file.txt user@hostname:/path/
```
> SCP with specific key

```bash
scp user1@host1:/path/file user2@host2:/path/
```
> Copy between two remote servers

---

## SSH Tunneling / Port Forwarding

```bash
ssh -L 8080:localhost:80 user@hostname
```
> Local port forwarding: access remote port 80 via localhost:8080

```bash
ssh -L 5432:localhost:5432 user@hostname
```
> Forward remote PostgreSQL to local

```bash
ssh -R 8080:localhost:3000 user@hostname
```
> Remote port forwarding: expose local port 3000 on remote as 8080

```bash
ssh -D 1080 user@hostname
```
> Dynamic SOCKS proxy on port 1080

```bash
ssh -N -L 8080:localhost:80 user@hostname
```
> Port forward only, no shell

```bash
ssh -fN -L 8080:localhost:80 user@hostname
```
> Port forward in background

---

## Jump Host / Bastion

```bash
ssh -J jumphost user@target
```
> Connect through jump host

```bash
ssh -J user1@jump1,user2@jump2 user@target
```
> Multi-hop through multiple jump hosts

---

## SSH Config File

Location: `~/.ssh/config`

```
Host myserver
    HostName 192.168.1.100
    User root
    Port 22
    IdentityFile ~/.ssh/mykey.pem

Host production
    HostName prod.example.com
    User deploy
    IdentityFile ~/.ssh/prod_key
    ForwardAgent yes

Host bastion
    HostName bastion.example.com
    User admin

Host internal
    HostName 10.0.0.5
    User app
    ProxyJump bastion

Host *
    ServerAliveInterval 60
    ServerAliveCountMax 3
    AddKeysToAgent yes
```

Then connect with just:
```bash
ssh myserver
```

---

## Useful SSH Options

```bash
ssh -v user@hostname
```
> Verbose output (debug connection issues)

```bash
ssh -vvv user@hostname
```
> Extra verbose

```bash
ssh -o StrictHostKeyChecking=no user@hostname
```
> Skip host key verification (not recommended for production)

```bash
ssh -o ConnectTimeout=10 user@hostname
```
> Timeout after 10 seconds

```bash
ssh -A user@hostname
```
> Forward SSH agent (use keys from local machine on remote)

```bash
ssh -X user@hostname
```
> Enable X11 forwarding (GUI apps)

---

## SFTP

```bash
sftp user@hostname
```

### SFTP Commands (inside sftp session):
```
ls                  # List remote files
lls                 # List local files
cd /remote/path     # Change remote directory
lcd /local/path     # Change local directory
get file.txt        # Download file
put file.txt        # Upload file
mget *.txt          # Download multiple files
mput *.txt          # Upload multiple files
mkdir dirname       # Create remote directory
rm file.txt         # Delete remote file
exit                # Close session
```

---

## PuTTY / PPK Keys (Windows)

```bash
puttygen mykey.pem -O private -o mykey.ppk
```
> Convert PEM to PPK

```bash
puttygen mykey.ppk -O private-openssh -o mykey.pem
```
> Convert PPK to PEM

```bash
plink -i mykey.ppk user@hostname
```
> Connect using PuTTY CLI

```bash
pscp -i mykey.ppk file.txt user@hostname:/path/
```
> SCP using PPK key (PuTTY)

---

*See also: [[Networking Commands]] | [[Linux & Bash Commands]]*
