---
type: cheatsheet
tags: [commands, systemctl, systemd, service, linux, cheatsheet]
created: 2026-03-02
---

# systemctl & systemd Commands

---

## Service Management

```bash
sudo systemctl start service-name
```

```bash
sudo systemctl stop service-name
```

```bash
sudo systemctl restart service-name
```

```bash
sudo systemctl reload service-name
```
> Reload config without restarting

```bash
sudo systemctl enable service-name
```
> Start on boot

```bash
sudo systemctl disable service-name
```
> Don't start on boot

```bash
sudo systemctl status service-name
```

```bash
sudo systemctl is-active service-name
```

```bash
sudo systemctl is-enabled service-name
```

```bash
sudo systemctl mask service-name
```
> Completely prevent service from starting

```bash
sudo systemctl unmask service-name
```

```bash
sudo systemctl daemon-reload
```
> Reload systemd after editing unit files

---

## Common Services

```bash
sudo systemctl restart nginx
```

```bash
sudo systemctl restart postgresql
```

```bash
sudo systemctl restart apache2
```

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
sudo systemctl restart docker
```

```bash
sudo systemctl restart sshd
```

---

## Listing

```bash
systemctl list-units --type=service
```
> List running services

```bash
systemctl list-units --type=service --state=running
```

```bash
systemctl list-units --type=service --state=failed
```

```bash
systemctl list-unit-files --type=service
```
> List all service files (enabled/disabled)

```bash
systemctl list-timers
```
> List scheduled timers

```bash
systemctl list-dependencies service-name
```

---

## System Targets

```bash
systemctl get-default
```

```bash
sudo systemctl set-default multi-user.target
```
> Boot to CLI

```bash
sudo systemctl set-default graphical.target
```
> Boot to GUI

```bash
sudo systemctl isolate multi-user.target
```
> Switch to CLI now

```bash
sudo systemctl reboot
```

```bash
sudo systemctl poweroff
```

```bash
sudo systemctl suspend
```

---

## journalctl (Logs)

```bash
journalctl -u service-name
```
> Logs for specific service

```bash
journalctl -u service-name -f
```
> Follow logs in real-time

```bash
journalctl -u service-name --since today
```

```bash
journalctl -u service-name --since "2025-01-01" --until "2025-01-31"
```

```bash
journalctl -u service-name -n 100
```
> Last 100 lines

```bash
journalctl -u service-name -p err
```
> Only error priority and above

```bash
journalctl -b
```
> Logs since last boot

```bash
journalctl -b -1
```
> Logs from previous boot

```bash
journalctl --disk-usage
```

```bash
sudo journalctl --vacuum-size=500M
```
> Reduce log storage to 500MB

```bash
sudo journalctl --vacuum-time=7d
```
> Remove logs older than 7 days

---

## Custom Service File

Location: `/etc/systemd/system/myapp.service`

```ini
[Unit]
Description=My Application
After=network.target postgresql.service
Wants=postgresql.service

[Service]
Type=simple
User=myuser
Group=mygroup
WorkingDirectory=/opt/myapp
ExecStart=/usr/bin/python3 /opt/myapp/app.py
ExecReload=/bin/kill -HUP $MAINPID
Restart=always
RestartSec=5
StandardOutput=journal
StandardError=journal
Environment=APP_ENV=production

[Install]
WantedBy=multi-user.target
```

### After Creating/Editing:
```bash
sudo systemctl daemon-reload
```

```bash
sudo systemctl enable myapp
```

```bash
sudo systemctl start myapp
```

---

## Odoo Service File Example

```ini
[Unit]
Description=Odoo 18
After=network.target postgresql.service

[Service]
Type=simple
User=odoo
Group=odoo
ExecStart=/opt/odoo/odoo-bin -c /etc/odoo/odoo.conf
Restart=always
RestartSec=3

[Install]
WantedBy=multi-user.target
```

---

*See also: [[Bitnami & Odoo Server Commands]] | [[Nginx Commands]] | [[Linux & Bash Commands]]*
