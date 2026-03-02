---
type: cheatsheet
tags: [commands, postgresql, database, sql, cheatsheet]
created: 2026-03-02
---

# PostgreSQL Commands

---

## Connection

```bash
sudo su postgres
```

```bash
psql
```

```bash
psql -U username -d database_name
```

```bash
psql -h hostname -p 5432 -U username -d database_name
```

```bash
psql "postgresql://user:password@host:5432/dbname"
```

---

## psql Meta-Commands

```sql
\l
```
> List all databases

```sql
\c database_name
```
> Connect to database

```sql
\dt
```
> List tables in current database

```sql
\dt+
```
> List tables with sizes

```sql
\d table_name
```
> Describe table structure

```sql
\d+ table_name
```
> Detailed table info with comments

```sql
\dn
```
> List schemas

```sql
\du
```
> List users/roles

```sql
\df
```
> List functions

```sql
\di
```
> List indexes

```sql
\dv
```
> List views

```sql
\dp
```
> List privileges

```sql
\x
```
> Toggle expanded display (vertical output)

```sql
\timing
```
> Toggle query timing

```sql
\i /path/to/file.sql
```
> Execute SQL file

```sql
\o output.txt
```
> Redirect output to file

```sql
\! ls
```
> Execute shell command

```sql
\q
```
> Quit psql

---

## Database Management

```sql
CREATE DATABASE mydb;
```

```sql
CREATE DATABASE mydb OWNER myuser ENCODING 'UTF8';
```

```sql
DROP DATABASE mydb;
```

```sql
DROP DATABASE IF EXISTS mydb;
```

### Force Drop (Terminate Connections First)
```sql
SELECT pg_terminate_backend(pid) FROM pg_stat_activity WHERE datname = 'mydb' AND pid != pg_backend_pid();
```
```sql
DROP DATABASE mydb;
```

```sql
ALTER DATABASE mydb RENAME TO newname;
```

```sql
ALTER DATABASE mydb OWNER TO newuser;
```

---

## Table Operations

```sql
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    created_at TIMESTAMP DEFAULT NOW()
);
```

```sql
DROP TABLE IF EXISTS users CASCADE;
```

```sql
ALTER TABLE users ADD COLUMN phone VARCHAR(20);
```

```sql
ALTER TABLE users DROP COLUMN phone;
```

```sql
ALTER TABLE users ALTER COLUMN name TYPE TEXT;
```

```sql
ALTER TABLE users RENAME COLUMN name TO full_name;
```

```sql
ALTER TABLE users RENAME TO customers;
```

```sql
TRUNCATE TABLE users;
```
> Delete all rows (fast, no logging)

---

## CRUD Operations

### SELECT
```sql
SELECT * FROM users;
```

```sql
SELECT name, email FROM users WHERE id = 1;
```

```sql
SELECT * FROM users WHERE name LIKE '%amir%';
```

```sql
SELECT * FROM users WHERE name ILIKE '%amir%';
```
> Case-insensitive LIKE

```sql
SELECT * FROM users ORDER BY created_at DESC LIMIT 10;
```

```sql
SELECT * FROM users OFFSET 20 LIMIT 10;
```

```sql
SELECT COUNT(*) FROM users;
```

```sql
SELECT department, COUNT(*) as total FROM users GROUP BY department HAVING COUNT(*) > 5;
```

```sql
SELECT u.name, o.total FROM users u JOIN orders o ON u.id = o.user_id;
```

```sql
SELECT u.name, o.total FROM users u LEFT JOIN orders o ON u.id = o.user_id;
```

```sql
SELECT DISTINCT department FROM users;
```

### INSERT
```sql
INSERT INTO users (name, email) VALUES ('Amir', 'amir@example.com');
```

```sql
INSERT INTO users (name, email) VALUES ('A', 'a@a.com'), ('B', 'b@b.com'), ('C', 'c@c.com');
```

```sql
INSERT INTO users (name, email) VALUES ('Amir', 'amir@example.com') ON CONFLICT (email) DO UPDATE SET name = EXCLUDED.name;
```
> Upsert — insert or update on conflict

### UPDATE
```sql
UPDATE users SET name = 'New Name' WHERE id = 1;
```

```sql
UPDATE users SET name = 'New', email = 'new@email.com' WHERE id = 1;
```

### DELETE
```sql
DELETE FROM users WHERE id = 1;
```

```sql
DELETE FROM users WHERE created_at < NOW() - INTERVAL '1 year';
```

---

## Users & Permissions

```sql
CREATE USER myuser WITH PASSWORD 'mypassword';
```

```sql
CREATE ROLE myrole WITH LOGIN PASSWORD 'pass';
```

```sql
ALTER USER myuser WITH PASSWORD 'newpassword';
```

```sql
ALTER USER myuser WITH SUPERUSER;
```

```sql
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;
```

```sql
GRANT SELECT, INSERT, UPDATE ON TABLE users TO myuser;
```

```sql
GRANT ALL ON ALL TABLES IN SCHEMA public TO myuser;
```

```sql
REVOKE ALL ON DATABASE mydb FROM myuser;
```

```sql
DROP USER myuser;
```

---

## Indexes

```sql
CREATE INDEX idx_users_email ON users(email);
```

```sql
CREATE UNIQUE INDEX idx_users_email ON users(email);
```

```sql
CREATE INDEX idx_users_name ON users USING gin(to_tsvector('english', name));
```
> Full-text search index

```sql
DROP INDEX idx_users_email;
```

```sql
REINDEX TABLE users;
```

---

## Backup & Restore

```bash
pg_dump dbname > backup.sql
```

```bash
pg_dump -U username -h localhost dbname > backup.sql
```

```bash
pg_dump -Fc dbname > backup.dump
```
> Custom format (compressed)

```bash
pg_dump -Fc -U username -h hostname dbname > backup.dump
```

```bash
pg_dumpall > all_databases.sql
```
> Backup all databases

```bash
psql dbname < backup.sql
```

```bash
pg_restore -d dbname backup.dump
```

```bash
pg_restore -U username -h hostname -d dbname backup.dump
```

```bash
pg_restore -Fc -d dbname --clean backup.dump
```
> Restore with clean (drop existing)

---

## CSV Import/Export

```sql
COPY users TO '/tmp/users.csv' WITH CSV HEADER;
```

```sql
COPY users FROM '/tmp/users.csv' WITH CSV HEADER;
```

```sql
\copy users TO 'users.csv' WITH CSV HEADER
```
> Client-side copy (no server file access needed)

```sql
\copy users FROM 'users.csv' WITH CSV HEADER
```

---

## Monitoring & Maintenance

```sql
SELECT * FROM pg_stat_activity;
```
> View active connections

```sql
SELECT datname, count(*) FROM pg_stat_activity GROUP BY datname;
```
> Connection count per database

```sql
SELECT pg_size_pretty(pg_database_size('mydb'));
```
> Database size

```sql
SELECT pg_size_pretty(pg_total_relation_size('users'));
```
> Table size (including indexes)

```sql
VACUUM ANALYZE users;
```

```sql
VACUUM FULL users;
```
> Reclaim disk space (locks table)

```sql
ANALYZE users;
```
> Update statistics

```sql
EXPLAIN ANALYZE SELECT * FROM users WHERE email = 'test@test.com';
```
> Query execution plan

---

## Useful Queries

```sql
SELECT version();
```

```sql
SELECT now();
```

```sql
SELECT pg_reload_conf();
```
> Reload configuration without restart

```sql
SELECT table_name, pg_size_pretty(pg_total_relation_size(quote_ident(table_name))) AS size FROM information_schema.tables WHERE table_schema = 'public' ORDER BY pg_total_relation_size(quote_ident(table_name)) DESC;
```
> All table sizes sorted

---

*See also: [[Database Administration Commands]] | [[Bitnami & Odoo Server Commands]]*
