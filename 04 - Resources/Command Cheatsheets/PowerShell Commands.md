---
type: cheatsheet
tags: [commands, powershell, windows, cheatsheet]
created: 2026-03-02
---

# PowerShell Commands

---

## Navigation & Files

```powershell
Get-Location
```
> Current directory (alias: pwd)

```powershell
Set-Location C:\Users
```
> Change directory (alias: cd)

```powershell
Get-ChildItem
```
> List files (alias: ls, dir)

```powershell
Get-ChildItem -Recurse -Filter "*.py"
```
> Find files recursively

```powershell
Get-ChildItem -Path C:\Users -Hidden
```
> Show hidden files

```powershell
New-Item -ItemType File -Name "file.txt"
```

```powershell
New-Item -ItemType Directory -Name "folder"
```

```powershell
New-Item -ItemType Directory -Path "nested\path\here" -Force
```

```powershell
Copy-Item source.txt dest.txt
```

```powershell
Copy-Item -Recurse source\ dest\
```

```powershell
Move-Item old.txt new.txt
```

```powershell
Remove-Item file.txt
```

```powershell
Remove-Item -Recurse -Force folder\
```

```powershell
Rename-Item old-name.txt new-name.txt
```

---

## File Content

```powershell
Get-Content file.txt
```
> Read file (alias: cat, type)

```powershell
Get-Content file.txt -Head 20
```
> First 20 lines

```powershell
Get-Content file.txt -Tail 20
```
> Last 20 lines

```powershell
Get-Content file.txt -Wait
```
> Follow file (like tail -f)

```powershell
Set-Content file.txt "new content"
```
> Write to file (overwrites)

```powershell
Add-Content file.txt "appended line"
```
> Append to file

```powershell
Clear-Content file.txt
```

---

## Search

```powershell
Select-String -Pattern "search" -Path *.txt
```
> Search in files (like grep)

```powershell
Select-String -Pattern "search" -Path *.txt -Recurse
```

```powershell
Select-String -Pattern "error" -Path *.log -CaseSensitive
```

```powershell
Get-ChildItem -Recurse | Select-String "pattern"
```
> Search all files recursively

---

## Process Management

```powershell
Get-Process
```

```powershell
Get-Process | Where-Object {$_.CPU -gt 100}
```

```powershell
Get-Process -Name "node"
```

```powershell
Stop-Process -Name "node"
```

```powershell
Stop-Process -Id 1234
```

```powershell
Stop-Process -Id 1234 -Force
```

```powershell
Start-Process notepad
```

```powershell
Start-Process "C:\Program Files\app.exe" -ArgumentList "--flag"
```

---

## Service Management

```powershell
Get-Service
```

```powershell
Get-Service -Name "wuauserv"
```

```powershell
Get-Service | Where-Object {$_.Status -eq "Running"}
```

```powershell
Start-Service -Name "servicename"
```

```powershell
Stop-Service -Name "servicename"
```

```powershell
Restart-Service -Name "servicename"
```

```powershell
Set-Service -Name "servicename" -StartupType Automatic
```

---

## Networking

```powershell
Test-Connection google.com
```
> Ping

```powershell
Test-Connection google.com -Count 3
```

```powershell
Test-NetConnection -ComputerName server -Port 443
```
> Test specific port

```powershell
Resolve-DnsName google.com
```

```powershell
Get-NetIPAddress
```

```powershell
Get-NetAdapter
```

```powershell
Invoke-WebRequest -Uri "https://api.example.com"
```
> HTTP request (alias: curl, wget)

```powershell
Invoke-RestMethod -Uri "https://api.example.com/data"
```
> HTTP request returning parsed JSON

```powershell
(Invoke-RestMethod "https://api.example.com/data").results
```

```powershell
Invoke-WebRequest -Uri "https://example.com/file.zip" -OutFile "file.zip"
```
> Download file

---

## System Info

```powershell
Get-ComputerInfo | Select-Object OsName, OsVersion, CsTotalPhysicalMemory
```

```powershell
Get-CimInstance -ClassName Win32_OperatingSystem
```

```powershell
systeminfo
```

```powershell
Get-Disk
```

```powershell
Get-Volume
```

```powershell
Get-PSDrive -PSProvider FileSystem
```
> Drive space info

```powershell
[System.Environment]::OSVersion
```

---

## Environment Variables

```powershell
$env:PATH
```

```powershell
$env:MY_VAR = "value"
```
> Set for current session

```powershell
[Environment]::SetEnvironmentVariable("MY_VAR", "value", "User")
```
> Set permanently for user

```powershell
Get-ChildItem Env:
```
> List all env vars

---

## Pipeline & Objects

```powershell
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
```
> Top 10 CPU-consuming processes

```powershell
Get-ChildItem | Where-Object {$_.Length -gt 1MB}
```
> Files larger than 1MB

```powershell
Get-ChildItem | Measure-Object -Property Length -Sum
```
> Total size of files

```powershell
Get-ChildItem | Group-Object Extension
```
> Group files by extension

```powershell
Get-ChildItem | ForEach-Object { $_.Name.ToUpper() }
```

```powershell
Get-Process | Export-Csv processes.csv -NoTypeInformation
```

```powershell
Import-Csv data.csv
```

```powershell
Get-Process | ConvertTo-Json | Out-File processes.json
```

---

## Utility

```powershell
Get-Clipboard
```

```powershell
Set-Clipboard "text to copy"
```

```powershell
Get-FileHash file.txt -Algorithm SHA256
```

```powershell
Compress-Archive -Path folder\ -DestinationPath archive.zip
```

```powershell
Expand-Archive archive.zip -DestinationPath folder\
```

```powershell
Start-Transcript -Path log.txt
```
> Record all session output

```powershell
Stop-Transcript
```

```powershell
Get-History
```

```powershell
Get-ExecutionPolicy
```

```powershell
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
```

---

## Windows Commands (CMD)

```cmd
dir
```

```cmd
cd /d D:\path
```

```cmd
mkdir folder
```

```cmd
copy source.txt dest.txt
```

```cmd
robocopy source\ dest\ /MIR /Z
```
> Mirror directory (robust copy)

```cmd
del file.txt
```

```cmd
ren old.txt new.txt
```

```cmd
type file.txt
```

```cmd
tasklist
```

```cmd
taskkill /PID 1234 /F
```

```cmd
taskkill /IM process.exe /F
```

```cmd
ipconfig /all
```

```cmd
ipconfig /flushdns
```

```cmd
netstat -an
```

```cmd
netstat -an | findstr :8080
```

```cmd
shutdown /s /t 0
```
> Shutdown immediately

```cmd
shutdown /r /t 0
```
> Restart immediately

```cmd
sfc /scannow
```
> System file checker

```cmd
chkdsk C: /f
```

```cmd
where program-name
```
> Find executable location

```cmd
setx MY_VAR "value"
```
> Set permanent env var

---

*See also: [[Linux & Bash Commands]] | [[VS Code Shortcuts]]*
