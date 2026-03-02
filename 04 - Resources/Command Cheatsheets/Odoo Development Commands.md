---
type: cheatsheet
tags: [commands, odoo, erp, development, cheatsheet]
created: 2026-03-02
---

# Odoo Development Commands

---

## Starting the Server

```bash
./odoo-bin -c /etc/odoo/odoo.conf
```

```bash
./odoo-bin --addons-path=addons,enterprise,custom-addons -d mydb
```

```bash
./odoo-bin -d mydb -r dbuser -w dbpassword --db_host=localhost
```

```bash
./odoo-bin -d mydb --dev=reload,qweb,xml
```
> Dev mode with auto-reload

```bash
./odoo-bin -d mydb --workers=4
```
> Multi-worker mode

```bash
./odoo-bin -d mydb --proxy-mode
```
> Behind reverse proxy

```bash
./odoo-bin -d mydb --db-filter=^%d$
```
> Database filter by domain

```bash
./odoo-bin -d mydb --limit-memory-hard=2684354560 --limit-memory-soft=2147483648
```

---

## Module Operations

### Install Module
```bash
./odoo-bin -d mydb -i module_name --stop-after-init
```

### Install Multiple Modules
```bash
./odoo-bin -d mydb -i module1,module2,module3 --stop-after-init
```

### Update Module
```bash
./odoo-bin -d mydb -u module_name --stop-after-init
```

### Update All Modules
```bash
./odoo-bin -d mydb -u all --stop-after-init
```

### Update Multiple Modules
```bash
./odoo-bin -d mydb -u module1,module2 --stop-after-init
```

---

## Scaffolding (New Module)

```bash
./odoo-bin scaffold module_name /path/to/addons/
```
> Creates module structure with boilerplate

Generated structure:
```
module_name/
├── __init__.py
├── __manifest__.py
├── controllers/
│   ├── __init__.py
│   └── controllers.py
├── demo/
│   └── demo.xml
├── models/
│   ├── __init__.py
│   └── models.py
├── security/
│   └── ir.model.access.csv
└── views/
    ├── templates.xml
    └── views.xml
```

---

## Odoo Shell (Interactive)

```bash
./odoo-bin shell -d mydb
```

### Common Shell Commands
```python
# Search records
env['res.partner'].search([('name', 'like', 'Amir')])
```

```python
# Search and read
partners = env['res.partner'].search_read([('customer_rank', '>', 0)], ['name', 'email'])
```

```python
# Create record
env['res.partner'].create({'name': 'New Partner', 'email': 'new@partner.com'})
```

```python
# Write (update)
partner = env['res.partner'].browse(1)
partner.write({'phone': '+1234567890'})
```

```python
# Delete record
partner = env['res.partner'].browse(1)
partner.unlink()
```

```python
# Commit changes
env.cr.commit()
```

```python
# Search count
env['sale.order'].search_count([('state', '=', 'sale')])
```

```python
# Execute SQL
env.cr.execute("SELECT id, name FROM res_partner LIMIT 10")
env.cr.fetchall()
```

```python
# Install module via shell
env['ir.module.module'].search([('name', '=', 'sale')]).button_immediate_install()
env.cr.commit()
```

```python
# Update module via shell
env['ir.module.module'].search([('name', '=', 'sale')]).button_immediate_upgrade()
env.cr.commit()
```

```python
# Reset admin password
admin = env['res.users'].browse(2)
admin.write({'password': 'newpassword'})
env.cr.commit()
```

---

## Database Operations

### Dump Database
```bash
pg_dump -U odoo mydb > mydb_backup.sql
```

```bash
pg_dump -Fc -U odoo mydb > mydb_backup.dump
```

### Restore Database
```bash
createdb -U odoo newdb
psql -U odoo newdb < mydb_backup.sql
```

### Duplicate Database
```bash
createdb -U odoo -T mydb mydb_copy
```

### Drop Database
```bash
dropdb -U odoo mydb
```

### Rename Database
```sql
ALTER DATABASE mydb RENAME TO newname;
```

### Via psql
```bash
sudo su postgres
psql
\l
```
> List databases

```sql
SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname = 'mydb' AND pid != pg_backend_pid();
```
> Kill active connections before drop

```sql
DROP DATABASE mydb;
```

---

## Testing

```bash
./odoo-bin -d testdb -i module_name --test-enable --stop-after-init
```

```bash
./odoo-bin -d testdb -i module_name --test-enable --test-tags=/module_name --stop-after-init
```

```bash
./odoo-bin -d testdb --test-enable --test-tags=:TestClassName --stop-after-init
```

---

## Utility Commands

```bash
./odoo-bin cloc -d mydb --addons-path=addons
```
> Count lines of code

```bash
./odoo-bin populate -d mydb --models=res.partner,sale.order
```
> Generate demo data

```bash
./odoo-bin neutralize -d mydb
```
> Neutralize database (disable emails, crons, etc.)

---

## Configuration File Reference

Location: `/etc/odoo/odoo.conf`

```ini
[options]
admin_passwd = master_password
db_host = localhost
db_port = 5432
db_user = odoo
db_password = odoo
db_name = False
dbfilter = ^%d$
addons_path = /opt/odoo/addons,/opt/odoo/enterprise,/opt/odoo/custom-addons
data_dir = /opt/odoo/.local/share/Odoo
logfile = /var/log/odoo/odoo.log
log_level = info
proxy_mode = True
workers = 4
max_cron_threads = 2
limit_memory_hard = 2684354560
limit_memory_soft = 2147483648
limit_time_cpu = 600
limit_time_real = 1200
```

---

## Git Workflow for Odoo Modules

```bash
git status
git add .
git commit -m "Updated Odoo Module"
git push origin 18.0
```

### Branch per Odoo version
```bash
git checkout -b 18.0
git checkout -b 17.0
git checkout -b 16.0
```

### Merge to another version
```bash
git checkout 17.0
git merge 18.0
git push origin 17.0
```

---

*See also: [[Bitnami & Odoo Server Commands]] | [[PostgreSQL Commands]] | [[Git Commands]]*
