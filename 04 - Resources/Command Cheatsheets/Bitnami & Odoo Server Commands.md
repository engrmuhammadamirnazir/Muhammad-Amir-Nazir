---
type: cheatsheet
tags: [commands, bitnami, odoo, server, ecosire, cheatsheet]
created: 2026-03-02
---

# Bitnami & Odoo Server Commands

> Commands for managing Bitnami Odoo stacks and standalone Odoo servers.

---

## Bitnami Service Control

```bash
sudo /opt/bitnami/ctlscript.sh status
```

```bash
sudo /opt/bitnami/ctlscript.sh start
```

```bash
sudo /opt/bitnami/ctlscript.sh stop
```

```bash
sudo /opt/bitnami/ctlscript.sh restart
```

```bash
sudo /opt/bitnami/ctlscript.sh restart odoo
```

```bash
sudo /opt/bitnami/ctlscript.sh restart postgresql
```

---

## Odoo systemd Services

```bash
sudo systemctl restart odoo17
```

```bash
sudo systemctl stop odoo16
```

```bash
sudo systemctl start odoo16
```

```bash
sudo systemctl status odoo17
```

```bash
sudo systemctl enable odoo17
```

```bash
systemctl restart apache2
```

---

## Odoo Configuration

```bash
sudo nano /opt/bitnami/odoo/conf/odoo.conf
```

### Key Config Parameters
```ini
db_name = False
dbfilter = ^%d$
proxy_mode = True
```

### Enterprise Addons Path
```
/opt/bitnami/odoo/addons/enterprise
```

### Web Enterprise Home Menu
```
/opt/bitnami/odoo/addons/web_enterprise/static/src/webclient/home_menu
```

---

## SSL Certificate (Bitnami)

```bash
sudo /opt/bitnami/bncert-tool
```
> Interactive SSL certificate setup

---

## WKHTMLTOPDF Installation

### Debian Bookworm
```bash
sudo apt update
sudo apt -y install wget
sudo wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6.1-3/wkhtmltox_0.12.6.1-3.bookworm_amd64.deb
sudo apt install ./wkhtmltox_0.12.6.1-3.bookworm_amd64.deb
sudo /opt/bitnami/ctlscript.sh restart
```

### Debian Buster
```bash
sudo apt update
sudo apt -y install wget
sudo wget https://github.com/wkhtmltopdf/packaging/releases/download/0.12.6-1/wkhtmltox_0.12.6-1.buster_amd64.deb
sudo apt install ./wkhtmltox_0.12.6-1.buster_amd64.deb
sudo /opt/bitnami/ctlscript.sh restart
```

---

## Enterprise Installation

```bash
sudo apt-get install git
```

```bash
git clone https://github.com/odoo/enterprise --depth 1 --branch 18.0 --single-branch .
```

```bash
git clone https://github.com/odoo/enterprise --depth 1 --branch 17.0 --single-branch .
```

---

## Python Dependencies

```bash
sudo pip3 install google_auth
```

```bash
sudo pip3 install qifparse
```

```bash
sudo apt-get install python3-pandas
```

---

## Module Publishing (Git)

```bash
git switch -c 19.0
git add .
git commit -m "Module Update"
git push -u origin 19.0
```

### Quick Push Workflow
```bash
git add .
git commit -m "Updated Module"
git push origin 18.0
```

---

*See also: [[Odoo Development Commands]] | [[PostgreSQL Commands]] | [[systemctl & systemd Commands]]*
