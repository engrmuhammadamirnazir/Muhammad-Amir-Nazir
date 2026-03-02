---
type: cheatsheet
tags: [commands, database, postgres, administration, cheatsheet]
created: 2026-03-02
---

# Database Administration Commands

> Quick reference for database operations commonly needed for Odoo/Bitnami environments.

---

## Connect to PostgreSQL

```bash
sudo su postgres
```

```bash
psql
```

```bash
psql -U username -d database_name
```

---

## List Databases

```sql
\l
```

---

## Connect to Specific Database

```sql
\c database_name
```

---

## Drop Database (Safe Method)

### Step 1: Kill active connections
```sql
SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname = 'database_name' AND pid != pg_backend_pid();
```

### Step 2: Drop the database
```sql
DROP DATABASE "database_name";
```

### Step 3: Exit
```sql
\q
```

```bash
exit
```

### Step 4: Restart Odoo
```bash
sudo /opt/bitnami/ctlscript.sh restart odoo
```

---

## Backup Database

```bash
pg_dump -U odoo database_name > backup.sql
```

```bash
pg_dump -Fc -U odoo database_name > backup.dump
```
> Compressed custom format

---

## Restore Database

```bash
createdb -U odoo new_database
```

```bash
psql -U odoo new_database < backup.sql
```

Or with custom format:
```bash
pg_restore -d new_database backup.dump
```

---

## Duplicate Database

```bash
createdb -U odoo -T original_db copy_db
```

---

## Rename Database

```sql
ALTER DATABASE old_name RENAME TO new_name;
```
> Must disconnect all users first

---

## Check Database Size

```sql
SELECT pg_size_pretty(pg_database_size('database_name'));
```

---

## List All Tables

```sql
\dt
```

---

## Quick Troubleshooting

### Cannot drop database — active connections
```sql
SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname = 'database_name' AND pid != pg_backend_pid();
```

### For PostgreSQL 8.4-9.1
```sql
SELECT pg_terminate_backend(procpid) FROM pg_stat_activity WHERE procpid <> pg_backend_pid() AND datname = 'database_name';
```

### Check who is connected
```sql
SELECT datname, usename, client_addr, state FROM pg_stat_activity;
```

---

*See also: [[PostgreSQL Commands]] | [[Bitnami & Odoo Server Commands]] | [[Odoo Development Commands]]*
