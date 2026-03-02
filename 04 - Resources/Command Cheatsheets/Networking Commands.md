---
type: cheatsheet
tags: [commands, networking, dns, firewall, cheatsheet]
created: 2026-03-02
---

# Networking Commands

---

## Connectivity Testing

```bash
ping google.com
```

```bash
ping -c 4 google.com
```
> Ping 4 times then stop

```bash
ping -c 4 192.168.1.1
```

```bash
traceroute google.com
```

```bash
tracert google.com
```
> Windows

```bash
mtr google.com
```
> Better traceroute (interactive)

---

## DNS

```bash
nslookup example.com
```

```bash
nslookup -type=MX example.com
```
> Mail server records

```bash
nslookup -type=TXT example.com
```

```bash
dig example.com
```

```bash
dig example.com +short
```
> Just the IP

```bash
dig example.com MX
```

```bash
dig example.com TXT
```

```bash
dig @8.8.8.8 example.com
```
> Query specific DNS server

```bash
dig example.com ANY +noall +answer
```
> All records, clean output

```bash
host example.com
```

---

## Network Interfaces

### Linux
```bash
ip addr show
```

```bash
ip addr show eth0
```

```bash
ifconfig
```

```bash
ip link show
```

```bash
ip route show
```
> Routing table

```bash
ip route get 8.8.8.8
```
> Route to specific IP

### Windows
```cmd
ipconfig
```

```cmd
ipconfig /all
```

```cmd
ipconfig /flushdns
```

```cmd
ipconfig /release
```

```cmd
ipconfig /renew
```

---

## Ports & Connections

```bash
netstat -tulnp
```
> All listening ports with PIDs (Linux)

```bash
netstat -an
```
> All connections

```bash
netstat -an | grep :8069
```
> Find specific port

```bash
ss -tulnp
```
> Modern replacement for netstat (Linux)

```bash
ss -s
```
> Connection summary

```bash
lsof -i :8080
```
> What's using port 8080

```bash
lsof -i -P -n
```
> All network connections

### Windows
```cmd
netstat -an
```

```cmd
netstat -an | findstr :8080
```

```cmd
netstat -ab
```
> Show process names (requires admin)

---

## HTTP Testing

### curl
```bash
curl https://example.com
```

```bash
curl -I https://example.com
```
> Headers only

```bash
curl -o file.zip https://example.com/file.zip
```
> Download file

```bash
curl -O https://example.com/file.zip
```
> Download with original filename

```bash
curl -L https://example.com
```
> Follow redirects

```bash
curl -s https://api.example.com | jq .
```
> Silent + JSON pretty-print

```bash
curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' https://api.example.com
```

```bash
curl -X PUT -H "Authorization: Bearer TOKEN" -d '{"data":"value"}' https://api.example.com/resource
```

```bash
curl -u user:pass https://api.example.com
```
> Basic auth

```bash
curl -k https://self-signed.example.com
```
> Skip SSL verification

```bash
curl -w "\nHTTP Code: %{http_code}\nTime: %{time_total}s\n" https://example.com
```
> Show response code and timing

### wget
```bash
wget https://example.com/file.zip
```

```bash
wget -c https://example.com/large-file.zip
```
> Resume download

```bash
wget -q -O - https://example.com
```
> Output to stdout quietly

```bash
wget --mirror --convert-links --page-requisites https://example.com
```
> Download entire website

---

## Netcat (nc)

```bash
nc -zv hostname 80
```
> Test if port is open

```bash
nc -zv hostname 20-100
```
> Scan port range

```bash
nc -l 1234
```
> Listen on port 1234

```bash
echo "test" | nc hostname 1234
```
> Send data to port

---

## Firewall

### iptables (Linux)
```bash
sudo iptables -L
```
> List rules

```bash
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
```
> Allow HTTP

```bash
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```
> Allow HTTPS

```bash
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```
> Allow SSH

```bash
sudo iptables -A INPUT -j DROP
```
> Drop all other input

```bash
sudo iptables-save > /etc/iptables.rules
```

### ufw (Ubuntu)
```bash
sudo ufw enable
```

```bash
sudo ufw status verbose
```

```bash
sudo ufw allow 22/tcp
```

```bash
sudo ufw allow 80/tcp
```

```bash
sudo ufw allow 443/tcp
```

```bash
sudo ufw allow 8069/tcp
```
> Odoo

```bash
sudo ufw deny 3306/tcp
```

```bash
sudo ufw delete allow 80/tcp
```

```bash
sudo ufw allow from 192.168.1.0/24
```
> Allow subnet

---

## ARP & MAC

```bash
arp -a
```

```bash
ip neigh show
```

---

## Bandwidth & Transfer

```bash
scp file.txt user@host:/path/
```

```bash
rsync -avz source/ user@host:/dest/
```
> Sync with compression

```bash
rsync -avz --progress source/ user@host:/dest/
```

```bash
rsync -avz --delete source/ user@host:/dest/
```
> Sync and delete extra files on dest

---

*See also: [[SSH & SCP Commands]] | [[Linux & Bash Commands]] | [[Nginx Commands]]*
