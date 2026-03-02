---
type: cheatsheet
tags: [commands, linux, bash, shell, cheatsheet]
created: 2026-03-02
---

# Linux & Bash Commands

---

## Navigation

```bash
pwd
```

```bash
cd /path/to/directory
```

```bash
cd ~
```
> Home directory

```bash
cd -
```
> Previous directory

```bash
ls -la
```

```bash
ls -lah
```
> Human-readable sizes

```bash
ls -lt
```
> Sort by modification time

```bash
ls -lS
```
> Sort by size

```bash
tree -L 2
```
> Directory tree, 2 levels deep

---

## File Operations

```bash
cp source.txt dest.txt
```

```bash
cp -r source-dir/ dest-dir/
```

```bash
cp -a source/ dest/
```
> Copy preserving all attributes

```bash
mv old-name.txt new-name.txt
```

```bash
mv file.txt /new/path/
```

```bash
rm file.txt
```

```bash
rm -rf directory/
```

```bash
mkdir -p path/to/nested/dir
```

```bash
touch newfile.txt
```

```bash
ln -s /path/to/original /path/to/symlink
```

```bash
readlink -f symlink
```
> Show target of symlink

---

## File Viewing

```bash
cat file.txt
```

```bash
head -n 20 file.txt
```

```bash
tail -n 20 file.txt
```

```bash
tail -f logfile.log
```
> Follow file in real-time

```bash
less file.txt
```
> Paginated viewer (q to quit, / to search)

```bash
wc -l file.txt
```
> Count lines

```bash
wc -w file.txt
```
> Count words

```bash
diff file1.txt file2.txt
```

```bash
diff -u file1.txt file2.txt
```
> Unified diff format

---

## Permissions

```bash
chmod 755 script.sh
```

```bash
chmod +x script.sh
```

```bash
chmod -R 644 directory/
```

```bash
chown user:group file.txt
```

```bash
chown -R user:group directory/
```

| Permission | Number | Meaning            |
|------------|--------|--------------------|
| rwx        | 7      | read+write+execute |
| rw-        | 6      | read+write         |
| r-x        | 5      | read+execute       |
| r--        | 4      | read only          |

---

## Search & Find

```bash
find . -name "*.py"
```

```bash
find . -type f -name "*.log" -mtime -7
```
> Files modified in last 7 days

```bash
find . -type f -size +100M
```
> Files larger than 100MB

```bash
find . -name "*.tmp" -delete
```

```bash
find . -type f -name "*.py" -exec grep -l "pattern" {} \;
```

```bash
grep "pattern" file.txt
```

```bash
grep -r "pattern" directory/
```
> Recursive search

```bash
grep -rn "pattern" directory/
```
> With line numbers

```bash
grep -ri "pattern" directory/
```
> Case insensitive

```bash
grep -rl "pattern" directory/
```
> Only filenames

```bash
grep -v "pattern" file.txt
```
> Invert match (lines NOT matching)

```bash
grep -E "pattern1|pattern2" file.txt
```
> Extended regex (OR)

```bash
grep -c "pattern" file.txt
```
> Count matches

```bash
which command-name
```

```bash
locate filename
```

---

## Text Processing

```bash
sed 's/old/new/g' file.txt
```
> Replace all occurrences (print to stdout)

```bash
sed -i 's/old/new/g' file.txt
```
> Replace in-place

```bash
sed -n '5,10p' file.txt
```
> Print lines 5-10

```bash
sed '/pattern/d' file.txt
```
> Delete lines matching pattern

```bash
awk '{print $1, $3}' file.txt
```
> Print columns 1 and 3

```bash
awk -F',' '{print $2}' file.csv
```
> CSV — print column 2

```bash
awk 'NR>=5 && NR<=10' file.txt
```
> Print lines 5-10

```bash
awk '{sum+=$1} END {print sum}' file.txt
```
> Sum column 1

```bash
sort file.txt
```

```bash
sort -n file.txt
```
> Numeric sort

```bash
sort -rn file.txt
```
> Reverse numeric sort

```bash
sort -t',' -k2 file.csv
```
> Sort CSV by column 2

```bash
sort file.txt | uniq
```

```bash
sort file.txt | uniq -c | sort -rn
```
> Count occurrences, sorted by frequency

```bash
cut -d',' -f1,3 file.csv
```
> Extract columns 1 and 3

```bash
cut -c1-10 file.txt
```
> Extract first 10 characters

```bash
tr 'a-z' 'A-Z' < file.txt
```
> Convert to uppercase

```bash
tr -d '\r' < file.txt > clean.txt
```
> Remove Windows carriage returns

---

## Pipes & Redirection

```bash
command > output.txt
```
> Redirect stdout (overwrite)

```bash
command >> output.txt
```
> Redirect stdout (append)

```bash
command 2> error.txt
```
> Redirect stderr

```bash
command > output.txt 2>&1
```
> Redirect both stdout and stderr

```bash
command &> output.txt
```
> Redirect both (shorthand)

```bash
command1 | command2
```
> Pipe stdout of command1 to command2

```bash
command | tee output.txt
```
> Write to file AND stdout

```bash
command | tee -a output.txt
```
> Append to file AND stdout

```bash
cat file.txt | xargs command
```

```bash
find . -name "*.log" | xargs rm
```

```bash
find . -name "*.py" -print0 | xargs -0 grep "pattern"
```
> Handle filenames with spaces

---

## Archives & Compression

```bash
tar czf archive.tar.gz directory/
```
> Create gzipped tarball

```bash
tar xzf archive.tar.gz
```
> Extract gzipped tarball

```bash
tar xzf archive.tar.gz -C /target/dir/
```
> Extract to specific directory

```bash
tar tzf archive.tar.gz
```
> List contents without extracting

```bash
zip -r archive.zip directory/
```

```bash
unzip archive.zip
```

```bash
unzip archive.zip -d /target/dir/
```

```bash
gzip file.txt
```

```bash
gunzip file.txt.gz
```

---

## Downloads

```bash
curl -O https://example.com/file.zip
```
> Download with original filename

```bash
curl -o output.zip https://example.com/file.zip
```
> Download with custom name

```bash
curl -L https://example.com/redirect
```
> Follow redirects

```bash
curl -s https://api.example.com/data | jq .
```
> Silent mode, pipe to jq for JSON

```bash
curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' https://api.example.com
```

```bash
curl -u user:pass https://api.example.com
```
> Basic auth

```bash
wget https://example.com/file.zip
```

```bash
wget -c https://example.com/largefile.zip
```
> Resume interrupted download

```bash
wget -r -l 2 https://example.com/
```
> Recursive download, 2 levels

---

## Process Management

```bash
ps aux
```

```bash
ps aux | grep process-name
```

```bash
top
```

```bash
htop
```

```bash
kill PID
```

```bash
kill -9 PID
```
> Force kill

```bash
killall process-name
```

```bash
pkill -f "pattern"
```

```bash
nohup command &
```
> Run in background, survives terminal close

```bash
command &
```
> Run in background

```bash
jobs
```

```bash
fg %1
```
> Bring job 1 to foreground

```bash
bg %1
```
> Resume job 1 in background

---

## Disk & System Info

```bash
df -h
```
> Disk space usage

```bash
du -sh directory/
```
> Directory size

```bash
du -sh * | sort -rh | head -10
```
> Top 10 largest items in current dir

```bash
free -h
```
> Memory usage

```bash
uname -a
```
> System info

```bash
lsb_release -a
```
> OS version

```bash
uptime
```

```bash
whoami
```

```bash
hostname
```

```bash
date
```

```bash
cal
```

```bash
lsblk
```
> List block devices

```bash
mount | column -t
```

---

## User Management

```bash
sudo adduser username
```

```bash
sudo usermod -aG groupname username
```
> Add user to group

```bash
sudo passwd username
```

```bash
su - username
```

```bash
groups username
```

```bash
id username
```

---

## Cron Jobs

```bash
crontab -e
```
> Edit crontab

```bash
crontab -l
```
> List cron jobs

```
# Cron format: MIN HOUR DOM MON DOW command
# Every day at 2am
0 2 * * * /path/to/script.sh

# Every Monday at 9am
0 9 * * 1 /path/to/script.sh

# Every 5 minutes
*/5 * * * * /path/to/script.sh
```

---

## Environment Variables

```bash
echo $PATH
```

```bash
export VAR_NAME="value"
```

```bash
env
```
> List all environment variables

```bash
printenv VAR_NAME
```

```bash
source ~/.bashrc
```
> Reload bash config

---

## Useful One-Liners

```bash
history | grep "command"
```

```bash
!!
```
> Repeat last command

```bash
sudo !!
```
> Repeat last command with sudo

```bash
watch -n 5 command
```
> Run command every 5 seconds

```bash
time command
```
> Measure execution time

```bash
yes | command
```
> Auto-answer yes to prompts

```bash
column -t -s',' file.csv
```
> Pretty-print CSV

---

*See also: [[SSH & SCP Commands]] | [[Networking Commands]] | [[Regex Patterns]]*
