---
type: cheatsheet
tags: [commands, nginx, webserver, reverse-proxy, cheatsheet]
created: 2026-03-02
---

# Nginx Commands

---

## Service Management

```bash
sudo systemctl start nginx
```

```bash
sudo systemctl stop nginx
```

```bash
sudo systemctl restart nginx
```

```bash
sudo systemctl reload nginx
```
> Reload config without downtime

```bash
sudo systemctl enable nginx
```

```bash
sudo systemctl status nginx
```

```bash
sudo nginx -t
```
> Test configuration syntax

```bash
sudo nginx -T
```
> Test and dump full config

```bash
sudo nginx -s reload
```

```bash
sudo nginx -s stop
```

---

## Key File Locations

```
/etc/nginx/nginx.conf              # Main config
/etc/nginx/sites-available/         # Available site configs
/etc/nginx/sites-enabled/           # Enabled site configs (symlinks)
/etc/nginx/conf.d/                  # Additional config files
/var/log/nginx/access.log           # Access log
/var/log/nginx/error.log            # Error log
/var/www/html/                      # Default web root
```

---

## Basic Server Block

```nginx
server {
    listen 80;
    server_name example.com www.example.com;
    root /var/www/example;
    index index.html;

    location / {
        try_files $uri $uri/ =404;
    }
}
```

---

## Reverse Proxy

```nginx
server {
    listen 80;
    server_name app.example.com;

    location / {
        proxy_pass http://127.0.0.1:8069;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

### Reverse Proxy for Odoo
```nginx
server {
    listen 80;
    server_name erp.example.com;

    location / {
        proxy_pass http://127.0.0.1:8069;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
        proxy_redirect off;
    }

    location /longpolling {
        proxy_pass http://127.0.0.1:8072;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    }

    client_max_body_size 200M;
}
```

---

## SSL / HTTPS

```nginx
server {
    listen 443 ssl http2;
    server_name example.com;

    ssl_certificate /etc/letsencrypt/live/example.com/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/example.com/privkey.pem;
    ssl_protocols TLSv1.2 TLSv1.3;
    ssl_ciphers HIGH:!aNULL:!MD5;

    root /var/www/example;
    index index.html;
}

# Redirect HTTP to HTTPS
server {
    listen 80;
    server_name example.com;
    return 301 https://$host$request_uri;
}
```

---

## Load Balancing

```nginx
upstream backend {
    server 127.0.0.1:8001;
    server 127.0.0.1:8002;
    server 127.0.0.1:8003;
}

server {
    listen 80;
    location / {
        proxy_pass http://backend;
    }
}
```

### With Weights
```nginx
upstream backend {
    server 127.0.0.1:8001 weight=3;
    server 127.0.0.1:8002 weight=1;
    server 127.0.0.1:8003 backup;
}
```

---

## WebSocket Support

```nginx
location /ws {
    proxy_pass http://127.0.0.1:8080;
    proxy_http_version 1.1;
    proxy_set_header Upgrade $http_upgrade;
    proxy_set_header Connection "upgrade";
    proxy_set_header Host $host;
}
```

---

## Static Files & Caching

```nginx
location /static/ {
    alias /var/www/static/;
    expires 30d;
    add_header Cache-Control "public, immutable";
}

location ~* \.(jpg|jpeg|png|gif|ico|css|js)$ {
    expires 7d;
    add_header Cache-Control "public";
}
```

---

## Gzip Compression

```nginx
gzip on;
gzip_vary on;
gzip_min_length 1024;
gzip_types text/plain text/css application/json application/javascript text/xml application/xml text/javascript;
```

---

## Rate Limiting

```nginx
limit_req_zone $binary_remote_addr zone=api:10m rate=10r/s;

server {
    location /api/ {
        limit_req zone=api burst=20 nodelay;
        proxy_pass http://backend;
    }
}
```

---

## Security Headers

```nginx
add_header X-Frame-Options "SAMEORIGIN" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Content-Security-Policy "default-src 'self';" always;
```

---

## Redirects

```nginx
# Permanent redirect
return 301 https://new.example.com$request_uri;

# Temporary redirect
return 302 https://temp.example.com$request_uri;

# www to non-www
server {
    server_name www.example.com;
    return 301 https://example.com$request_uri;
}
```

---

## Upload Size

```nginx
client_max_body_size 100M;
```

---

## Enable Site

```bash
sudo ln -s /etc/nginx/sites-available/mysite /etc/nginx/sites-enabled/
```

```bash
sudo rm /etc/nginx/sites-enabled/mysite
```
> Disable site

```bash
sudo nginx -t && sudo systemctl reload nginx
```
> Test and reload

---

*See also: [[systemctl & systemd Commands]] | [[SSH & SCP Commands]]*
