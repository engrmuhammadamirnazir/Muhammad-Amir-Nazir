
# ============================================================================
#  ULTIMATE DEVELOPER COMMAND CHEATSHEET
#  Copy-Pasteable Reference for 20 Categories
#  Generated: 2026-03-02
# ============================================================================


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  1. GIT                                                                  ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Configuration ---
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
git config --global color.ui auto
git config --global core.editor "code --wait"
git config --global init.defaultBranch main
git config --list

## --- Repository Setup ---
git init
git init <directory>
git clone <url>
git clone <url> <directory>
git clone --depth 1 <url>                        # Shallow clone (latest commit only)
git clone --branch <branch> <url>

## --- Staging & Committing ---
git status
git status -s                                     # Short format
git add <file>
git add .                                         # Stage all changes
git add -p                                        # Interactive staging (hunks)
git add *.py                                      # Stage by pattern
git commit -m "message"
git commit -am "message"                          # Stage tracked + commit
git commit --amend                                # Amend last commit
git commit --amend --no-edit                      # Amend without changing message
git commit --allow-empty -m "empty commit"        # Trigger CI

## --- Branching ---
git branch                                        # List local branches
git branch -a                                     # List all branches (local + remote)
git branch -r                                     # List remote branches
git branch <name>                                 # Create branch
git branch -d <name>                              # Delete branch (safe)
git branch -D <name>                              # Force delete branch
git branch -m <old> <new>                         # Rename branch
git checkout <branch>
git checkout -b <new-branch>                      # Create and switch
git switch <branch>                               # Modern switch
git switch -c <new-branch>                        # Modern create + switch

## --- Merging & Rebasing ---
git merge <branch>
git merge --no-ff <branch>                        # Force merge commit
git merge --squash <branch>                       # Squash all commits into one
git merge --abort                                 # Abort conflicted merge
git rebase <branch>
git rebase -i HEAD~3                              # Interactive rebase last 3 commits
git rebase --onto <new-base> <old-base> <branch>
git rebase --abort
git rebase --continue
git cherry-pick <commit-hash>
git cherry-pick <hash1> <hash2>                   # Multiple commits
git cherry-pick --no-commit <hash>                # Apply without committing

## --- Remote ---
git remote -v                                     # List remotes
git remote add origin <url>
git remote remove origin
git remote rename origin upstream
git remote set-url origin <new-url>
git fetch
git fetch --all
git fetch --prune                                 # Remove stale remote branches
git pull                                          # Fetch + merge
git pull --rebase                                 # Fetch + rebase
git push
git push -u origin <branch>                       # Set upstream + push
git push origin --delete <branch>                 # Delete remote branch
git push --tags                                   # Push all tags
git push --force-with-lease                       # Safer force push

## --- Stash ---
git stash                                         # Stash changes
git stash push -m "description"                   # Stash with message
git stash list                                    # List stashes
git stash pop                                     # Apply and remove latest
git stash apply stash@{0}                         # Apply without removing
git stash drop stash@{0}                          # Delete specific stash
git stash clear                                   # Delete all stashes
git stash show -p stash@{0}                       # Show stash diff
git stash branch <branch> stash@{0}              # Create branch from stash

## --- History & Diff ---
git log
git log --oneline
git log --oneline --graph --all
git log --author="name"
git log --since="2024-01-01" --until="2024-12-31"
git log -n 5                                      # Last 5 commits
git log -p <file>                                 # History of a file
git log --stat                                    # Show file change stats
git shortlog -sn                                  # Commit count per author
git diff                                          # Unstaged changes
git diff --staged                                 # Staged changes
git diff <branch1>..<branch2>                     # Between branches
git diff HEAD~3..HEAD                             # Last 3 commits
git diff --name-only                              # Only file names

## --- Undo & Reset ---
git reset <file>                                  # Unstage file
git reset --soft HEAD~1                           # Undo commit, keep staged
git reset --mixed HEAD~1                          # Undo commit, keep unstaged
git reset --hard HEAD~1                           # Undo commit, discard changes
git revert <commit-hash>                          # Create inverse commit
git revert --no-commit <hash>                     # Revert without committing
git checkout -- <file>                            # Discard file changes
git restore <file>                                # Modern: discard changes
git restore --staged <file>                       # Modern: unstage

## --- Tags ---
git tag                                           # List tags
git tag v1.0.0                                    # Lightweight tag
git tag -a v1.0.0 -m "Release 1.0.0"            # Annotated tag
git tag -a v1.0.0 <commit-hash>                  # Tag specific commit
git tag -d v1.0.0                                 # Delete local tag
git push origin v1.0.0                            # Push specific tag
git push origin --delete v1.0.0                   # Delete remote tag

## --- Advanced ---
git submodule add <url> <path>
git submodule update --init --recursive
git submodule foreach git pull origin main
git worktree add <path> <branch>
git worktree list
git worktree remove <path>
git bisect start
git bisect bad
git bisect good <commit>
git bisect reset
git reflog                                        # Show reference log
git reflog expire --expire=now --all
git clean -n                                      # Dry run: show untracked files
git clean -fd                                     # Remove untracked files + dirs
git clean -fdx                                    # Also remove ignored files
git blame <file>
git blame -L 10,20 <file>                        # Blame specific lines


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  2. DOCKER                                                               ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Images ---
docker build .
docker build -t myapp:latest .
docker build -t myapp:latest -f Dockerfile.prod .
docker images
docker images -a                                  # Include intermediates
docker rmi <image>
docker rmi $(docker images -q)                    # Remove all images
docker image prune                                # Remove dangling images
docker image prune -a                             # Remove all unused images
docker pull nginx:latest
docker push myrepo/myapp:latest
docker tag myapp:latest myrepo/myapp:v1.0
docker save myapp > myapp.tar                     # Export image
docker load < myapp.tar                           # Import image
docker history <image>
docker inspect <image>

## --- Containers ---
docker run -d -p 80:80 nginx                     # Detached, port mapping
docker run -it ubuntu bash                        # Interactive terminal
docker run --name myapp -d -p 3000:3000 myapp
docker run --rm -it alpine sh                     # Auto-remove on exit
docker run -d -v /host/path:/container/path myapp # Bind mount
docker run -d -v myvolume:/data myapp             # Named volume
docker run -d --env-file .env myapp               # Environment file
docker run -d -e NODE_ENV=production myapp        # Single env var
docker run -d --restart=always myapp              # Auto-restart
docker run -d --network mynet myapp               # Specific network
docker run -d --memory="512m" --cpus="1.0" myapp  # Resource limits
docker ps                                         # Running containers
docker ps -a                                      # All containers
docker ps -s                                      # With size
docker start <container>
docker stop <container>
docker restart <container>
docker pause <container>
docker unpause <container>
docker kill <container>
docker rm <container>
docker rm -f <container>                          # Force remove running
docker rm $(docker ps -aq)                        # Remove all containers
docker container prune                            # Remove stopped containers
docker exec -it <container> bash                  # Shell into container
docker exec -it <container> sh                    # For Alpine
docker exec <container> <command>                 # Run command
docker logs <container>
docker logs -f <container>                        # Follow logs
docker logs --tail 100 <container>                # Last 100 lines
docker logs --since 1h <container>                # Last hour
docker inspect <container>
docker stats                                      # Live resource usage
docker top <container>                            # Running processes
docker cp <container>:/path/file ./local          # Copy from container
docker cp ./local <container>:/path/file          # Copy to container
docker diff <container>                           # Changed files

## --- Network ---
docker network ls
docker network create mynet
docker network create -d bridge mybridge
docker network create -d overlay myoverlay
docker network inspect mynet
docker network connect mynet <container>
docker network disconnect mynet <container>
docker network rm mynet
docker network prune                              # Remove unused networks

## --- Volume ---
docker volume ls
docker volume create myvolume
docker volume inspect myvolume
docker volume rm myvolume
docker volume prune                               # Remove unused volumes

## --- Docker Compose ---
docker compose up                                 # Start services
docker compose up -d                              # Detached mode
docker compose up --build                         # Rebuild images
docker compose up -d --scale web=3                # Scale service
docker compose down                               # Stop and remove
docker compose down -v                            # Also remove volumes
docker compose down --rmi all                     # Also remove images
docker compose build                              # Build images
docker compose build --no-cache                   # Force rebuild
docker compose logs                               # View logs
docker compose logs -f <service>                  # Follow specific service
docker compose ps                                 # List services
docker compose exec <service> bash                # Shell into service
docker compose run <service> <command>            # Run one-off command
docker compose pull                               # Pull latest images
docker compose restart <service>                  # Restart service
docker compose config                             # Validate compose file
docker compose top                                # Running processes

## --- System Cleanup ---
docker system prune                               # Remove unused data
docker system prune -a                            # Remove everything unused
docker system prune -a --volumes                  # Including volumes
docker system df                                  # Disk usage summary


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  3. LINUX / BASH                                                         ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Navigation & Listing ---
pwd                                               # Print working directory
ls                                                # List files
ls -la                                            # Long format + hidden
ls -lh                                            # Human-readable sizes
ls -lt                                            # Sort by time
ls -lS                                            # Sort by size
ls -R                                             # Recursive
cd /path/to/dir
cd ~                                              # Home directory
cd -                                              # Previous directory
cd ..                                             # Parent directory
tree                                              # Directory tree
tree -L 2                                         # Limit depth

## --- File Operations ---
cp file1 file2                                    # Copy file
cp -r dir1 dir2                                   # Copy directory recursively
cp -i file1 file2                                 # Interactive (confirm overwrite)
mv file1 file2                                    # Move/rename
mv file1 /path/to/dest/
rm file                                           # Remove file
rm -r dir                                         # Remove directory
rm -rf dir                                        # Force remove
rm -i file                                        # Interactive remove
mkdir dirname
mkdir -p parent/child/grandchild                  # Create nested dirs
touch newfile.txt                                 # Create empty file
ln -s /path/to/target linkname                    # Symbolic link

## --- Permissions ---
chmod 755 file                                    # rwxr-xr-x
chmod 644 file                                    # rw-r--r--
chmod +x script.sh                                # Add execute permission
chmod -R 755 dir                                  # Recursive
chown user:group file
chown -R user:group dir

## --- File Viewing ---
cat file                                          # Display entire file
head file                                         # First 10 lines
head -n 20 file                                   # First 20 lines
tail file                                         # Last 10 lines
tail -n 20 file                                   # Last 20 lines
tail -f file                                      # Follow (live updates)
less file                                         # Scrollable viewer
wc file                                           # Lines, words, chars
wc -l file                                        # Line count only
wc -w file                                        # Word count only

## --- Search ---
find . -name "*.py"                               # Find by name
find . -type f -name "*.log"                      # Files only
find . -type d -name "src"                        # Directories only
find . -mtime -7                                  # Modified in last 7 days
find . -size +100M                                # Files > 100MB
find . -name "*.tmp" -delete                      # Find and delete
find . -name "*.js" -exec grep "TODO" {} \;       # Find and grep
grep "pattern" file                               # Search in file
grep -r "pattern" dir                             # Recursive search
grep -rn "pattern" dir                            # With line numbers
grep -ri "pattern" dir                            # Case-insensitive
grep -rl "pattern" dir                            # Files only (list)
grep -v "pattern" file                            # Invert match
grep -c "pattern" file                            # Count matches
grep -E "regex" file                              # Extended regex
grep -A 3 -B 3 "pattern" file                    # Context lines

## --- Text Processing ---
sed 's/old/new/' file                             # Replace first occurrence per line
sed 's/old/new/g' file                            # Replace all occurrences
sed -i 's/old/new/g' file                         # In-place edit
sed -n '5,10p' file                               # Print lines 5-10
sed '/pattern/d' file                             # Delete matching lines
awk '{print $1}' file                             # Print first column
awk -F: '{print $1}' /etc/passwd                  # Custom delimiter
awk '{sum+=$1} END {print sum}' file              # Sum column
awk 'NR>=5 && NR<=10' file                        # Lines 5-10
sort file                                         # Sort lines
sort -n file                                      # Numeric sort
sort -r file                                      # Reverse sort
sort -u file                                      # Sort + unique
sort -t: -k3 -n file                              # Sort by 3rd field
uniq                                              # Remove adjacent dupes
uniq -c                                           # Count occurrences
uniq -d                                           # Show only duplicates
cut -d: -f1 file                                  # Cut by delimiter
cut -c1-10 file                                   # Cut by character position
tee output.txt                                    # Write to file and stdout
tee -a output.txt                                 # Append mode

## --- Pipes & Redirection ---
command > file                                    # Redirect stdout (overwrite)
command >> file                                   # Redirect stdout (append)
command 2> file                                   # Redirect stderr
command 2>&1                                      # Stderr to stdout
command &> file                                   # Both stdout + stderr
command1 | command2                               # Pipe stdout
command1 | tee file | command2                    # Pipe + save to file
command | xargs <cmd>                             # Pass input as args
find . -name "*.log" | xargs rm                   # Find and remove
find . -name "*.py" | xargs grep "import"         # Find and search

## --- Archives ---
tar -czf archive.tar.gz dir/                      # Create gzip archive
tar -cjf archive.tar.bz2 dir/                     # Create bzip2 archive
tar -xzf archive.tar.gz                           # Extract gzip
tar -xzf archive.tar.gz -C /dest/                 # Extract to directory
tar -xjf archive.tar.bz2                          # Extract bzip2
tar -tf archive.tar.gz                            # List contents
zip -r archive.zip dir/                           # Create zip
unzip archive.zip                                 # Extract zip
unzip archive.zip -d /dest/                       # Extract to directory
unzip -l archive.zip                              # List contents

## --- Downloads ---
curl -O https://example.com/file.tar.gz           # Download (keep name)
curl -o output.txt https://example.com/file       # Download (custom name)
curl -L https://example.com/redirect              # Follow redirects
curl -s https://api.example.com/data              # Silent mode
curl -H "Authorization: Bearer TOKEN" https://api.example.com
curl -X POST -d '{"key":"value"}' -H "Content-Type: application/json" https://api.example.com
curl -I https://example.com                       # Headers only
wget https://example.com/file.tar.gz              # Download file
wget -O output.txt https://example.com/file       # Custom name
wget -r -l 1 https://example.com                  # Recursive (depth 1)
wget -c https://example.com/largefile.iso         # Resume download
wget -q https://example.com/file                  # Quiet mode

## --- Process Management ---
ps aux                                            # All processes
ps aux | grep <name>                              # Find process
top                                               # Live processes
htop                                              # Better live processes
kill <PID>                                        # Graceful kill
kill -9 <PID>                                     # Force kill
killall <process-name>                            # Kill by name
nohup command &                                   # Run in background (persists)
jobs                                              # List background jobs
fg %1                                             # Bring job to foreground
bg %1                                             # Send job to background

## --- Disk & System ---
df -h                                             # Disk space (human readable)
du -sh dir/                                       # Directory size
du -sh * | sort -h                                # Sizes sorted
free -h                                           # Memory usage
uname -a                                          # System info
uptime                                            # System uptime
whoami                                            # Current user
hostname                                          # Machine name
env                                               # Environment variables
export VAR="value"                                # Set env variable
echo $VAR                                         # Print variable
which <command>                                   # Find command path
history                                           # Command history
history | grep "pattern"                          # Search history


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  4. NPM / YARN / PNPM                                                   ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Initialize ---
npm init                                          # Interactive setup
npm init -y                                       # Default package.json
yarn init
yarn init -y
pnpm init

## --- Install Dependencies ---
npm install                                       # Install from package.json
npm ci                                            # Clean install (CI)
yarn                                              # Install from package.json
yarn install --frozen-lockfile                    # CI install
pnpm install
pnpm install --frozen-lockfile                    # CI install

## --- Add Packages ---
npm install <pkg>                                 # Add dependency
npm install <pkg> --save-dev                      # Add dev dependency
npm install <pkg> -g                              # Global install
npm install <pkg>@<version>                       # Specific version
yarn add <pkg>
yarn add <pkg> --dev
yarn global add <pkg>
yarn add <pkg>@<version>
pnpm add <pkg>
pnpm add <pkg> --save-dev
pnpm add <pkg> -g
pnpm add <pkg>@<version>

## --- Remove Packages ---
npm uninstall <pkg>
npm uninstall <pkg> -g                            # Global uninstall
yarn remove <pkg>
yarn global remove <pkg>
pnpm uninstall <pkg>
pnpm uninstall <pkg> -g

## --- Update ---
npm update                                        # Update all
npm update <pkg>                                  # Update specific
npm outdated                                      # Check outdated
yarn upgrade
yarn upgrade <pkg>
yarn upgrade-interactive                          # Interactive update
pnpm update
pnpm update <pkg>
pnpm outdated

## --- Run Scripts ---
npm run <script>
npm run build
npm run dev
npm test                                          # Shorthand for "test" script
npm start                                         # Shorthand for "start" script
yarn run <script>
yarn build                                        # No "run" needed
yarn dev
yarn test
pnpm run <script>
pnpm build
pnpm dev
pnpm test

## --- Publish & Link ---
npm publish
npm publish --tag beta
npm link                                          # Link current package globally
npm link <pkg>                                    # Link global package locally
yarn publish
yarn link
yarn link <pkg>
pnpm publish
pnpm link
pnpm link <pkg>

## --- Info & Audit ---
npm list --depth 0                                # Top-level packages
npm list -g --depth 0                             # Global packages
npm info <pkg>                                    # Package info
npm audit                                         # Security audit
npm audit fix                                     # Fix vulnerabilities
npm cache clean --force                           # Clear cache
yarn list --depth 0
yarn info <pkg>
yarn audit
yarn cache clean
pnpm list --depth 0
pnpm audit
pnpm store prune                                  # Clean store

## --- Run Without Installing ---
npx <pkg>                                         # npm
yarn dlx <pkg>                                    # yarn
pnpm dlx <pkg>                                    # pnpm


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  5. PYTHON                                                               ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Virtual Environments ---
python -m venv venv                               # Create venv
python -m venv .venv                              # Create .venv (common convention)
source venv/bin/activate                          # Activate (Linux/Mac)
venv\Scripts\activate                             # Activate (Windows CMD)
venv\Scripts\Activate.ps1                         # Activate (Windows PowerShell)
deactivate                                        # Deactivate

## --- pip Package Management ---
pip install <package>
pip install <package>==1.2.3                      # Specific version
pip install <package>>=1.0,<2.0                   # Version range
pip install -r requirements.txt                   # From requirements file
pip install -e .                                  # Editable/dev install
pip install --upgrade <package>
pip install --upgrade pip                         # Upgrade pip itself
pip uninstall <package>
pip uninstall -r requirements.txt                 # Uninstall from file
pip freeze > requirements.txt                     # Export installed packages
pip list                                          # List installed packages
pip list --outdated                               # Show outdated packages
pip show <package>                                # Package info
pip check                                         # Verify dependencies
pip cache purge                                   # Clear cache

## --- uv (Modern Alternative - Much Faster) ---
uv venv                                           # Create venv
uv pip install <package>                          # Install package
uv pip install -r requirements.txt                # From requirements
uv pip freeze > requirements.txt                  # Export
uv pip compile requirements.in -o requirements.txt # Lock dependencies
uv run python script.py                           # Run with auto-venv

## --- Conda ---
conda create -n myenv python=3.11                 # Create environment
conda activate myenv                              # Activate
conda deactivate                                  # Deactivate
conda install <package>
conda install -c conda-forge <package>            # From conda-forge
conda remove <package>
conda list                                        # List packages
conda env list                                    # List environments
conda env export > environment.yml                # Export environment
conda env create -f environment.yml               # Create from file
conda env remove -n myenv                         # Remove environment
conda update --all                                # Update all packages
conda clean --all                                 # Clean caches

## --- Running Python ---
python script.py
python -m <module>                                # Run module as script
python -m http.server 8000                        # Quick HTTP server
python -m json.tool < data.json                   # Pretty-print JSON
python -c "print('hello')"                        # Run inline code
python -i script.py                               # Interactive after script

## --- Testing (pytest) ---
pytest                                            # Run all tests
pytest tests/                                     # Run tests in directory
pytest tests/test_file.py                         # Run specific file
pytest tests/test_file.py::test_func              # Run specific test
pytest tests/test_file.py::TestClass              # Run specific class
pytest tests/test_file.py::TestClass::test_method # Run specific method
pytest -v                                         # Verbose output
pytest -s                                         # Show print statements
pytest -x                                         # Stop on first failure
pytest --lf                                       # Rerun last failed
pytest --ff                                       # Run failed first
pytest -k "keyword"                               # Filter by keyword
pytest -m "marker"                                # Filter by marker
pytest --cov=src                                  # Coverage report
pytest --cov=src --cov-report=html                # HTML coverage
pytest -n auto                                    # Parallel (pytest-xdist)
pytest --tb=short                                 # Shorter traceback

## --- Code Quality ---
black .                                           # Format code
black --check .                                   # Check formatting
black --diff .                                    # Show diff
flake8                                            # Lint code
flake8 --max-line-length=120                      # Custom line length
flake8 --ignore=E501,W503                         # Ignore rules
isort .                                           # Sort imports
isort --check-only .                              # Check import order
mypy .                                            # Type checking
ruff check .                                      # Fast linter (modern)
ruff check --fix .                                # Auto-fix
ruff format .                                     # Format (like black)


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  6. POWERSHELL                                                           ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Navigation ---
Set-Location C:\Users                             # cd equivalent
Get-Location                                      # pwd equivalent
Push-Location C:\temp                             # Push directory to stack
Pop-Location                                      # Return to previous

## --- File & Directory Operations ---
Get-ChildItem                                     # ls equivalent
Get-ChildItem -Recurse                            # Recursive listing
Get-ChildItem -Filter *.txt                       # Filter by extension
Get-ChildItem -Hidden                             # Show hidden files
Get-ChildItem -Path C:\ -Recurse -Filter *.log
New-Item -ItemType File -Name "file.txt"          # Create file
New-Item -ItemType Directory -Name "newdir"       # Create directory
New-Item -ItemType File -Path "dir\file.txt" -Force  # Create with parent dirs
Copy-Item source.txt dest.txt
Copy-Item -Recurse sourcedir destdir
Move-Item old.txt new.txt
Move-Item file.txt C:\destination\
Remove-Item file.txt
Remove-Item -Recurse -Force dir\                  # Force remove directory
Rename-Item old.txt new.txt
Test-Path C:\file.txt                             # Check if exists

## --- File Content ---
Get-Content file.txt                              # cat equivalent
Get-Content file.txt -Head 10                     # First 10 lines
Get-Content file.txt -Tail 20                     # Last 20 lines
Get-Content file.txt -Wait                        # tail -f equivalent
Set-Content file.txt "new content"                # Write to file
Add-Content file.txt "appended text"              # Append to file
Clear-Content file.txt                            # Empty file

## --- Search ---
Select-String -Path *.txt -Pattern "search"       # grep equivalent
Select-String -Path *.txt -Pattern "search" -CaseSensitive
Select-String -Recurse -Path .\* -Pattern "TODO"
Get-ChildItem -Recurse -Filter *.py | Select-String "import"

## --- Process Management ---
Get-Process                                       # List all processes
Get-Process -Name "chrome"                        # Filter by name
Get-Process | Sort-Object CPU -Descending | Select-Object -First 10
Stop-Process -Name "notepad"                      # Kill by name
Stop-Process -Id 1234                             # Kill by PID
Start-Process notepad                             # Start process
Start-Process "cmd.exe" -ArgumentList "/c dir"

## --- Service Management ---
Get-Service                                       # List all services
Get-Service -Name "wuauserv"                      # Specific service
Get-Service | Where-Object {$_.Status -eq "Running"}
Start-Service -Name "ServiceName"
Stop-Service -Name "ServiceName"
Restart-Service -Name "ServiceName"
Set-Service -Name "ServiceName" -StartupType Automatic

## --- Networking ---
Test-Connection google.com                        # ping equivalent
Test-Connection google.com -Count 4
Invoke-WebRequest -Uri "https://api.example.com"  # curl equivalent
Invoke-WebRequest -Uri "https://example.com" -OutFile "file.zip"
Invoke-RestMethod -Uri "https://api.example.com/data"  # REST API
Invoke-RestMethod -Method Post -Uri "https://api.example.com" -Body '{"key":"value"}'
Resolve-DnsName google.com                        # nslookup equivalent
Get-NetTCPConnection                              # netstat equivalent
Get-NetIPAddress                                  # ipconfig equivalent

## --- System Info ---
Get-ComputerInfo                                  # Full system info
$PSVersionTable                                   # PowerShell version
Get-WmiObject Win32_OperatingSystem               # OS info
Get-PSDrive                                       # Drive info
Get-EventLog -LogName System -Newest 10           # Recent events

## --- Pipeline & Objects ---
Get-Process | Where-Object {$_.CPU -gt 100}       # Filter
Get-Process | Select-Object Name, CPU, Id         # Select properties
Get-Process | Sort-Object CPU -Descending         # Sort
Get-Process | Measure-Object                      # Count
Get-Process | Export-Csv processes.csv             # Export to CSV
Get-Process | ConvertTo-Json                      # Convert to JSON
Get-Process | Format-Table -AutoSize              # Table format
Get-Process | Format-List                         # List format
Get-Process | Out-File processes.txt              # Save to file
Get-Process | Out-GridView                        # GUI grid view


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  7. SSH / SCP                                                            ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- SSH Connection ---
ssh user@hostname
ssh user@hostname -p 2222                         # Custom port
ssh -i ~/.ssh/mykey.pem user@hostname             # Specific key
ssh -v user@hostname                              # Verbose (debug)
ssh -o StrictHostKeyChecking=no user@hostname     # Skip host verification
ssh user@hostname "command"                       # Run remote command
ssh user@hostname "ls -la && df -h"               # Multiple commands

## --- SSH Key Management ---
ssh-keygen -t ed25519 -C "your@email.com"         # Generate Ed25519 key (recommended)
ssh-keygen -t rsa -b 4096 -C "your@email.com"    # Generate RSA key
ssh-keygen -t ed25519 -f ~/.ssh/mykey             # Custom filename
ssh-keygen -lf ~/.ssh/id_ed25519.pub              # Show key fingerprint
ssh-keygen -R hostname                            # Remove from known_hosts
ssh-copy-id user@hostname                         # Copy key to server
ssh-copy-id -i ~/.ssh/mykey.pub user@hostname     # Specific key

## --- SSH Agent ---
eval "$(ssh-agent -s)"                            # Start agent
ssh-add ~/.ssh/id_ed25519                         # Add key to agent
ssh-add -l                                        # List keys in agent
ssh-add -D                                        # Remove all keys

## --- SCP (Secure Copy) ---
scp file.txt user@host:/remote/path/              # Upload file
scp user@host:/remote/file.txt ./local/           # Download file
scp -r dir/ user@host:/remote/path/               # Upload directory
scp -r user@host:/remote/dir/ ./local/            # Download directory
scp -P 2222 file.txt user@host:/path/             # Custom port
scp -i ~/.ssh/mykey file.txt user@host:/path/     # Specific key
scp user1@host1:/file user2@host2:/path/          # Between two remotes

## --- SSH Tunneling & Port Forwarding ---
# Local port forwarding: access remote:8080 via localhost:8080
ssh -L 8080:localhost:8080 user@hostname

# Local forwarding to another host via jump server
ssh -L 3306:db-server:3306 user@jump-server

# Remote port forwarding: expose local:3000 on remote:3000
ssh -R 3000:localhost:3000 user@hostname

# Dynamic port forwarding (SOCKS proxy)
ssh -D 1080 user@hostname

# Background tunnel (no shell)
ssh -fN -L 8080:localhost:8080 user@hostname

# Jump/bastion host
ssh -J user@bastion user@target
ssh -o ProxyJump=user@bastion user@target

## --- SSH Config (~/.ssh/config) ---
# Host myserver
#     HostName 192.168.1.100
#     User admin
#     Port 2222
#     IdentityFile ~/.ssh/mykey
#     ForwardAgent yes
#
# Host bastion
#     HostName bastion.example.com
#     User jump-user
#
# Host internal
#     HostName 10.0.0.5
#     User admin
#     ProxyJump bastion
#
# Host *
#     ServerAliveInterval 60
#     ServerAliveCountMax 3
#     AddKeysToAgent yes

# After config, just: ssh myserver


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  8. VS CODE                                                              ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Essential Shortcuts (Windows/Linux) ---
# Ctrl+Shift+P          Command Palette
# Ctrl+P                Quick Open file
# Ctrl+Shift+N          New window
# Ctrl+Shift+W          Close window
# Ctrl+,                Settings
# Ctrl+`                Toggle terminal
# Ctrl+B                Toggle sidebar
# Ctrl+J                Toggle bottom panel
# Ctrl+\                Split editor

## --- Editing ---
# Ctrl+X                Cut line (empty selection)
# Ctrl+C                Copy line (empty selection)
# Ctrl+Shift+K          Delete line
# Ctrl+Enter            Insert line below
# Ctrl+Shift+Enter      Insert line above
# Alt+Up/Down           Move line up/down
# Shift+Alt+Up/Down     Copy line up/down
# Ctrl+D                Select next occurrence
# Ctrl+Shift+L          Select all occurrences
# Ctrl+H                Find and replace
# Ctrl+F                Find in file
# Ctrl+Shift+F          Find in all files
# Ctrl+/                Toggle line comment
# Shift+Alt+A           Toggle block comment
# Ctrl+Z                Undo
# Ctrl+Shift+Z          Redo
# Ctrl+Shift+[          Fold region
# Ctrl+Shift+]          Unfold region
# Ctrl+K Ctrl+0         Fold all
# Ctrl+K Ctrl+J         Unfold all

## --- Navigation ---
# Ctrl+G                Go to line
# Ctrl+T                Go to symbol in workspace
# Ctrl+Shift+O          Go to symbol in file
# F12                   Go to definition
# Alt+F12               Peek definition
# Shift+F12             Show references
# Ctrl+Shift+\          Jump to matching bracket
# Alt+Left/Right        Navigate back/forward
# Ctrl+Tab              Switch between open files
# Ctrl+PageUp/Down      Switch editor tabs

## --- Multi-Cursor ---
# Alt+Click             Add cursor
# Ctrl+Alt+Up/Down      Add cursor above/below
# Ctrl+U                Undo last cursor operation

## --- Terminal ---
# Ctrl+`                Toggle terminal
# Ctrl+Shift+`          New terminal
# Ctrl+Shift+5          Split terminal

## --- CLI Commands ---
code .                                            # Open current directory
code file.txt                                     # Open file
code -r .                                         # Open in current window
code --diff file1 file2                           # Diff two files
code --goto file.txt:10:5                         # Open at line:column
code --install-extension <ext-id>                 # Install extension
code --uninstall-extension <ext-id>               # Remove extension
code --list-extensions                            # List extensions
code --disable-extensions                         # Start with no extensions
code -n                                           # New window
code --add <dir>                                  # Add folder to workspace


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  9. NETWORKING                                                           ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Connectivity Testing ---
ping google.com
ping -c 4 google.com                              # 4 packets (Linux)
ping -n 4 google.com                              # 4 packets (Windows)
ping -i 2 google.com                              # Interval 2 seconds
traceroute google.com                             # Linux/Mac
tracert google.com                                # Windows
traceroute -n google.com                          # No DNS resolution
mtr google.com                                    # Continuous traceroute

## --- DNS Lookup ---
nslookup google.com
nslookup -type=MX google.com                     # MX records
nslookup -type=NS google.com                     # Name servers
nslookup -type=TXT google.com                    # TXT records
nslookup google.com 8.8.8.8                      # Use specific DNS server
dig google.com                                    # Detailed DNS query
dig google.com MX                                 # MX records
dig google.com NS                                 # Name servers
dig google.com +short                             # Short answer
dig @8.8.8.8 google.com                          # Use specific DNS
dig -x 8.8.8.8                                   # Reverse DNS
dig google.com ANY                                # All records

## --- Network Interfaces ---
ifconfig                                          # Linux/Mac (legacy)
ip addr show                                      # Linux (modern)
ip addr add 192.168.1.10/24 dev eth0             # Add IP
ip link set eth0 up                               # Bring interface up
ip link set eth0 down                             # Bring interface down
ip route show                                     # Show routes
ip route add default via 192.168.1.1              # Add default route
ipconfig                                          # Windows
ipconfig /all                                     # Windows detailed
ipconfig /release                                 # Release DHCP
ipconfig /renew                                   # Renew DHCP
ipconfig /flushdns                                # Flush DNS cache

## --- Connections & Ports ---
netstat -tuln                                     # Listening ports (Linux)
netstat -an                                       # All connections
netstat -tulnp                                    # With process info (Linux)
netstat -ano                                      # With PIDs (Windows)
ss -tuln                                          # Modern netstat (Linux)
ss -s                                             # Connection summary
ss -p                                             # With process info
lsof -i :8080                                     # What's on port 8080
lsof -i -P -n                                     # All connections

## --- HTTP Testing ---
curl https://example.com                          # GET request
curl -I https://example.com                       # Headers only
curl -X POST https://api.example.com              # POST request
curl -d '{"key":"val"}' -H "Content-Type: application/json" https://api.example.com
curl -u user:pass https://api.example.com         # Basic auth
curl -o output.html https://example.com           # Save to file
curl -w "%{http_code}" https://example.com        # Show status code
curl -k https://self-signed.example.com           # Ignore SSL errors
wget https://example.com/file.zip                 # Download file
wget --spider https://example.com                 # Check if URL exists

## --- Netcat (nc) ---
nc -zv hostname 80                                # Port scan single
nc -zv hostname 20-100                            # Port range scan
nc -l 8080                                        # Listen on port
nc hostname 8080                                  # Connect to port
nc -l 8080 > received_file                        # Receive file
nc hostname 8080 < send_file                      # Send file

## --- Firewall (iptables) ---
iptables -L                                       # List rules
iptables -L -n -v                                 # Detailed listing
iptables -A INPUT -p tcp --dport 80 -j ACCEPT     # Allow HTTP
iptables -A INPUT -p tcp --dport 443 -j ACCEPT    # Allow HTTPS
iptables -A INPUT -p tcp --dport 22 -j ACCEPT     # Allow SSH
iptables -A INPUT -j DROP                         # Drop all other
iptables -D INPUT -p tcp --dport 80 -j ACCEPT     # Delete rule
iptables -F                                       # Flush all rules
iptables-save > /etc/iptables/rules.v4            # Save rules
iptables-restore < /etc/iptables/rules.v4         # Restore rules

## --- Modern Firewall (ufw) ---
ufw enable
ufw disable
ufw status verbose
ufw allow 80/tcp
ufw allow 443/tcp
ufw allow ssh
ufw deny 3306
ufw delete allow 80/tcp


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  10. POSTGRESQL                                                          ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Connection ---
sudo -u postgres psql                             # Login as postgres
psql -d mydb                                      # Connect to database
psql -U john mydb                                 # Connect as user
psql -h localhost -p 5432 -U admin mydb           # Full connection
psql -h 192.168.1.5 -p 5432 -U admin -d mydb     # Remote connection
psql -W mydb                                      # Force password prompt
psql "postgresql://user:pass@host:5432/mydb"      # Connection string

## --- Meta-Commands (psql) ---
# \l              List all databases
# \c <database>   Connect to database
# \dt             List tables in current schema
# \dt+            List tables with sizes
# \d <table>      Describe table structure
# \d+ <table>     Detailed table info
# \dn             List schemas
# \du             List users/roles
# \df             List functions
# \di             List indexes
# \dv             List views
# \ds             List sequences
# \conninfo       Show connection info
# \timing         Toggle query timing
# \x              Toggle expanded display
# \i file.sql     Execute SQL file
# \o output.txt   Send output to file
# \o              Stop sending to file
# \q              Quit
# \!              Execute shell command
# \copy           Client-side COPY

## --- Database Operations ---
CREATE DATABASE mydb;
CREATE DATABASE mydb WITH OWNER myuser;
DROP DATABASE IF EXISTS mydb;
ALTER DATABASE mydb RENAME TO newname;
ALTER DATABASE mydb OWNER TO newowner;
SELECT current_database();
SELECT pg_size_pretty(pg_database_size('mydb'));

## --- Table Operations ---
CREATE TABLE users (
    id SERIAL PRIMARY KEY,
    name VARCHAR(100) NOT NULL,
    email VARCHAR(255) UNIQUE NOT NULL,
    age INTEGER DEFAULT 0,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE IF NOT EXISTS orders (
    id SERIAL PRIMARY KEY,
    user_id INTEGER REFERENCES users(id) ON DELETE CASCADE,
    total DECIMAL(10,2) NOT NULL,
    status VARCHAR(20) DEFAULT 'pending'
);

DROP TABLE IF EXISTS users CASCADE;
ALTER TABLE users ADD COLUMN phone VARCHAR(20);
ALTER TABLE users ALTER COLUMN name TYPE TEXT;
ALTER TABLE users DROP COLUMN phone;
ALTER TABLE users RENAME COLUMN name TO full_name;
ALTER TABLE users RENAME TO customers;
ALTER TABLE users ADD CONSTRAINT unique_email UNIQUE (email);
TRUNCATE TABLE users CASCADE;

## --- CRUD Operations ---
# SELECT
SELECT * FROM users;
SELECT name, email FROM users;
SELECT * FROM users WHERE age > 25;
SELECT * FROM users WHERE name LIKE '%john%';
SELECT * FROM users WHERE name ILIKE '%john%';     -- Case-insensitive
SELECT * FROM users WHERE age BETWEEN 20 AND 30;
SELECT * FROM users WHERE status IN ('active', 'pending');
SELECT * FROM users WHERE email IS NOT NULL;
SELECT * FROM users ORDER BY created_at DESC;
SELECT * FROM users ORDER BY name ASC LIMIT 10;
SELECT * FROM users LIMIT 10 OFFSET 20;
SELECT COUNT(*) FROM users;
SELECT status, COUNT(*) FROM users GROUP BY status;
SELECT status, COUNT(*) FROM users GROUP BY status HAVING COUNT(*) > 5;
SELECT DISTINCT status FROM users;

# INSERT
INSERT INTO users (name, email, age) VALUES ('John', 'john@example.com', 30);
INSERT INTO users (name, email) VALUES
    ('Alice', 'alice@example.com'),
    ('Bob', 'bob@example.com');
INSERT INTO users (name, email) VALUES ('John', 'john@example.com')
    ON CONFLICT (email) DO UPDATE SET name = EXCLUDED.name;  -- Upsert
INSERT INTO users (name, email) VALUES ('John', 'john@example.com')
    ON CONFLICT (email) DO NOTHING;

# UPDATE
UPDATE users SET age = 31 WHERE name = 'John';
UPDATE users SET age = age + 1 WHERE id = 1;
UPDATE users SET status = 'inactive' WHERE last_login < NOW() - INTERVAL '1 year';

# DELETE
DELETE FROM users WHERE id = 1;
DELETE FROM users WHERE status = 'inactive';
DELETE FROM users WHERE created_at < '2024-01-01';

## --- Joins ---
SELECT u.name, o.total
FROM users u
INNER JOIN orders o ON u.id = o.user_id;

SELECT u.name, o.total
FROM users u
LEFT JOIN orders o ON u.id = o.user_id;

SELECT u.name, o.total
FROM users u
RIGHT JOIN orders o ON u.id = o.user_id;

SELECT u.name, o.total
FROM users u
FULL OUTER JOIN orders o ON u.id = o.user_id;

## --- User & Permissions ---
CREATE USER myuser WITH PASSWORD 'mypassword';
CREATE ROLE myrole WITH LOGIN PASSWORD 'pass';
DROP USER IF EXISTS myuser;
ALTER ROLE myuser WITH PASSWORD 'newpassword';
ALTER ROLE myuser WITH SUPERUSER;
GRANT ALL PRIVILEGES ON DATABASE mydb TO myuser;
GRANT SELECT, INSERT, UPDATE ON ALL TABLES IN SCHEMA public TO myuser;
GRANT USAGE ON SCHEMA public TO myuser;
REVOKE ALL PRIVILEGES ON DATABASE mydb FROM myuser;
SELECT rolname FROM pg_roles;

## --- Index ---
CREATE INDEX idx_users_email ON users(email);
CREATE UNIQUE INDEX idx_users_email ON users(email);
CREATE INDEX idx_users_name ON users USING gin(to_tsvector('english', name));  -- Full text
DROP INDEX IF EXISTS idx_users_email;
REINDEX TABLE users;

## --- Backup & Restore ---
pg_dump mydb > backup.sql                         # Dump database
pg_dump -U postgres mydb > backup.sql             # As specific user
pg_dump -Fc mydb > backup.dump                    # Custom format (compressed)
pg_dump -t users mydb > users.sql                 # Single table
pg_dump -s mydb > schema.sql                      # Schema only
pg_dump -a mydb > data.sql                        # Data only
pg_dumpall -U postgres > all_databases.sql        # All databases
psql mydb < backup.sql                            # Restore SQL dump
pg_restore -d mydb backup.dump                    # Restore custom format
pg_restore -d mydb -c backup.dump                 # Clean (drop) before restore

## --- Import/Export CSV ---
# \copy users TO '/path/users.csv' CSV HEADER
# \copy users FROM '/path/users.csv' CSV HEADER
# \copy (SELECT * FROM users WHERE age > 25) TO '/path/filtered.csv' CSV HEADER
COPY users TO '/path/users.csv' CSV HEADER;       -- Server-side
COPY users FROM '/path/users.csv' CSV HEADER;      -- Server-side


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  11. NGINX                                                               ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Management Commands ---
nginx -t                                          # Test configuration
nginx -T                                          # Test and dump config
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
sudo systemctl reload nginx                       # Graceful reload
sudo systemctl status nginx
sudo systemctl enable nginx                       # Start on boot
nginx -s reload                                   # Alternative reload
nginx -s stop                                     # Alternative stop

## --- Basic Server Block ---
# server {
#     listen 80;
#     listen [::]:80;
#     server_name yourdomain.com www.yourdomain.com;
#     root /var/www/html;
#     index index.html index.htm;
#
#     location / {
#         try_files $uri $uri/ =404;
#     }
# }

## --- Reverse Proxy ---
# server {
#     listen 80;
#     server_name yourdomain.com;
#
#     location / {
#         proxy_pass http://localhost:3000;
#         proxy_http_version 1.1;
#         proxy_set_header Upgrade $http_upgrade;
#         proxy_set_header Connection 'upgrade';
#         proxy_set_header Host $host;
#         proxy_set_header X-Real-IP $remote_addr;
#         proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
#         proxy_set_header X-Forwarded-Proto $scheme;
#         proxy_cache_bypass $http_upgrade;
#     }
# }

## --- SSL / HTTPS ---
# server {
#     listen 443 ssl http2;
#     server_name yourdomain.com;
#
#     ssl_certificate /etc/letsencrypt/live/yourdomain.com/fullchain.pem;
#     ssl_certificate_key /etc/letsencrypt/live/yourdomain.com/privkey.pem;
#
#     ssl_protocols TLSv1.2 TLSv1.3;
#     ssl_ciphers HIGH:!aNULL:!MD5;
#     ssl_prefer_server_ciphers on;
#     ssl_session_cache shared:SSL:10m;
#     ssl_session_timeout 10m;
#
#     add_header Strict-Transport-Security "max-age=31536000" always;
#
#     location / {
#         proxy_pass http://localhost:3000;
#     }
# }
#
# # HTTP to HTTPS redirect
# server {
#     listen 80;
#     server_name yourdomain.com;
#     return 301 https://$host$request_uri;
# }

## --- Load Balancing ---
# upstream backend {
#     server 127.0.0.1:3001;
#     server 127.0.0.1:3002;
#     server 127.0.0.1:3003;
# }
#
# server {
#     listen 80;
#     server_name yourdomain.com;
#
#     location / {
#         proxy_pass http://backend;
#     }
# }

## --- WebSocket Support ---
# location /ws {
#     proxy_pass http://localhost:3000;
#     proxy_http_version 1.1;
#     proxy_set_header Upgrade $http_upgrade;
#     proxy_set_header Connection "upgrade";
#     proxy_set_header Host $host;
#     proxy_read_timeout 86400;
# }

## --- Static Files + Cache ---
# location ~* \.(jpg|jpeg|png|gif|ico|css|js|svg|woff2)$ {
#     expires 30d;
#     add_header Cache-Control "public, immutable";
# }

## --- Gzip Compression ---
# gzip on;
# gzip_vary on;
# gzip_min_length 1024;
# gzip_proxied any;
# gzip_comp_level 6;
# gzip_types text/plain text/css application/json application/javascript
#            text/xml application/xml application/xml+rss text/javascript;

## --- Rate Limiting ---
# limit_req_zone $binary_remote_addr zone=one:10m rate=10r/s;
#
# server {
#     location /api/ {
#         limit_req zone=one burst=20 nodelay;
#         proxy_pass http://localhost:3000;
#     }
# }

## --- Redirects ---
# return 301 http://yourdomain.com$request_uri;    # Permanent
# return 302 http://otherdomain.com;                # Temporary
# rewrite ^/old-page$ /new-page permanent;          # URL rewrite

## --- Client Upload Size ---
# client_max_body_size 50M;

## --- Security Headers ---
# add_header X-Frame-Options "SAMEORIGIN" always;
# add_header X-Content-Type-Options "nosniff" always;
# add_header X-XSS-Protection "1; mode=block" always;
# add_header Referrer-Policy "strict-origin-when-cross-origin" always;


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  12. SYSTEMCTL / SYSTEMD                                                 ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Service Management ---
sudo systemctl start <service>
sudo systemctl stop <service>
sudo systemctl restart <service>
sudo systemctl reload <service>                   # Reload config without restart
sudo systemctl status <service>
sudo systemctl enable <service>                   # Start on boot
sudo systemctl disable <service>                  # Don't start on boot
sudo systemctl enable --now <service>             # Enable and start
sudo systemctl is-active <service>
sudo systemctl is-enabled <service>
sudo systemctl mask <service>                     # Prevent starting entirely
sudo systemctl unmask <service>
sudo systemctl daemon-reload                      # Reload systemd manager config

## --- Listing ---
systemctl list-units                              # Active units
systemctl list-units --type=service               # Active services
systemctl list-units --all                        # All units
systemctl list-units --all --state=inactive       # Inactive units
systemctl list-unit-files                         # All installed units
systemctl list-unit-files --type=service          # Installed services
systemctl list-timers                             # Active timers
systemctl list-dependencies <service>             # Show dependencies
systemctl cat <service>                           # Show unit file

## --- System Targets ---
systemctl get-default                             # Current default target
sudo systemctl set-default multi-user.target      # CLI boot
sudo systemctl set-default graphical.target       # GUI boot
sudo systemctl isolate multi-user.target          # Switch to CLI now
sudo systemctl reboot
sudo systemctl poweroff
sudo systemctl suspend

## --- journalctl (Logs) ---
journalctl                                        # All logs
journalctl -u <service>                           # Logs for service
journalctl -u <service> -f                        # Follow logs (tail -f)
journalctl -u <service> --since today             # Today's logs
journalctl -u <service> --since "2024-01-01"      # Since date
journalctl -u <service> --since "1 hour ago"      # Last hour
journalctl -u <service> -n 50                     # Last 50 lines
journalctl -u <service> --no-pager                # Without pager
journalctl -b                                     # Current boot logs
journalctl -b -1                                  # Previous boot
journalctl --list-boots                           # List boots
journalctl -p err                                 # Error priority only
journalctl -p warning                             # Warnings and above
journalctl -k                                     # Kernel messages (dmesg)
journalctl --disk-usage                           # Journal disk usage
sudo journalctl --vacuum-size=500M                # Limit journal to 500M
sudo journalctl --vacuum-time=7d                  # Keep only 7 days

## --- Custom Service File (/etc/systemd/system/myapp.service) ---
# [Unit]
# Description=My Application
# After=network.target
#
# [Service]
# Type=simple
# User=www-data
# WorkingDirectory=/opt/myapp
# ExecStart=/opt/myapp/start.sh
# Restart=always
# RestartSec=5
# Environment=NODE_ENV=production
#
# [Install]
# WantedBy=multi-user.target


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  13. AWS CLI                                                              ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Configuration ---
aws configure                                     # Interactive setup
aws configure --profile project1                  # Named profile
aws configure list                                # Show current config
aws sts get-caller-identity                       # Who am I?

## --- S3 ---
aws s3 ls                                         # List buckets
aws s3 ls s3://mybucket                           # List bucket contents
aws s3 ls s3://mybucket/prefix/ --recursive       # Recursive listing
aws s3 mb s3://mybucket                           # Create bucket
aws s3 mb s3://mybucket --region us-west-2        # Create in region
aws s3 rb s3://mybucket                           # Remove empty bucket
aws s3 rb s3://mybucket --force                   # Remove with contents
aws s3 cp file.txt s3://mybucket/                 # Upload file
aws s3 cp s3://mybucket/file.txt ./               # Download file
aws s3 cp s3://mybucket/file.txt s3://other/      # Copy between buckets
aws s3 mv file.txt s3://mybucket/                 # Move to S3
aws s3 mv s3://mybucket/old.txt s3://mybucket/new.txt  # Rename
aws s3 sync ./local s3://mybucket/remote          # Sync local to S3
aws s3 sync s3://mybucket/remote ./local          # Sync S3 to local
aws s3 sync ./local s3://mybucket/ --delete       # Sync with delete
aws s3 rm s3://mybucket/file.txt                  # Delete file
aws s3 rm s3://mybucket/folder/ --recursive       # Delete folder
aws s3 presign s3://mybucket/file.txt --expires-in 3600  # Pre-signed URL

## --- EC2 ---
aws ec2 describe-instances
aws ec2 describe-instances --filters "Name=instance-state-name,Values=running"
aws ec2 describe-instances --instance-ids i-1234567890abcdef0
aws ec2 run-instances \
    --image-id ami-0abcdef1234567890 \
    --count 1 \
    --instance-type t2.micro \
    --key-name MyKeyPair \
    --security-group-ids sg-12345678 \
    --subnet-id subnet-12345678
aws ec2 start-instances --instance-ids i-1234567890abcdef0
aws ec2 stop-instances --instance-ids i-1234567890abcdef0
aws ec2 reboot-instances --instance-ids i-1234567890abcdef0
aws ec2 terminate-instances --instance-ids i-1234567890abcdef0
aws ec2 describe-instance-status --instance-ids i-1234567890abcdef0
aws ec2 describe-security-groups
aws ec2 describe-key-pairs
aws ec2 describe-vpcs
aws ec2 describe-subnets

## --- IAM ---
aws iam list-users
aws iam create-user --user-name newuser
aws iam delete-user --user-name olduser
aws iam get-user --user-name myuser
aws iam create-access-key --user-name myuser
aws iam list-access-keys --user-name myuser
aws iam delete-access-key --user-name myuser --access-key-id AKIA...
aws iam list-roles
aws iam create-role --role-name myrole --assume-role-policy-document file://trust.json
aws iam delete-role --role-name myrole
aws iam attach-role-policy --role-name myrole --policy-arn arn:aws:iam::aws:policy/ReadOnlyAccess
aws iam list-attached-role-policies --role-name myrole
aws iam list-policies
aws iam create-policy --policy-name mypolicy --policy-document file://policy.json
aws iam attach-user-policy --user-name myuser --policy-arn arn:aws:iam::aws:policy/...
aws iam add-user-to-group --user-name myuser --group-name mygroup

## --- Lambda ---
aws lambda list-functions
aws lambda get-function --function-name myfunction
aws lambda create-function \
    --function-name myfunction \
    --runtime python3.11 \
    --role arn:aws:iam::123456789:role/lambda-role \
    --handler lambda_function.lambda_handler \
    --zip-file fileb://function.zip
aws lambda update-function-code --function-name myfunction --zip-file fileb://function.zip
aws lambda update-function-configuration --function-name myfunction --timeout 30 --memory-size 256
aws lambda invoke --function-name myfunction --payload '{"key":"value"}' output.json
aws lambda delete-function --function-name myfunction
aws lambda publish-version --function-name myfunction
aws lambda list-versions-by-function --function-name myfunction
aws lambda create-alias --function-name myfunction --name prod --function-version 1
aws lambda list-aliases --function-name myfunction

## --- CloudFormation ---
aws cloudformation create-stack \
    --stack-name mystack \
    --template-body file://template.yaml \
    --parameters ParameterKey=Env,ParameterValue=prod \
    --capabilities CAPABILITY_IAM
aws cloudformation update-stack \
    --stack-name mystack \
    --template-body file://template.yaml
aws cloudformation delete-stack --stack-name mystack
aws cloudformation describe-stacks
aws cloudformation describe-stacks --stack-name mystack
aws cloudformation describe-stack-events --stack-name mystack
aws cloudformation list-stacks --stack-status-filter CREATE_COMPLETE
aws cloudformation validate-template --template-body file://template.yaml


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  14. GITHUB CLI (gh)                                                     ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Authentication ---
gh auth login                                     # Interactive login
gh auth login --with-token < token.txt            # Token login
gh auth logout
gh auth status
gh auth refresh
gh auth token                                     # Print token

## --- Repositories ---
gh repo create                                    # Interactive
gh repo create myrepo --public                    # Public repo
gh repo create myrepo --private                   # Private repo
gh repo create myrepo --clone                     # Create and clone
gh repo clone owner/repo
gh repo clone owner/repo -- --depth 1             # Shallow clone
gh repo fork owner/repo
gh repo fork owner/repo --clone                   # Fork and clone
gh repo view                                      # View current repo
gh repo view owner/repo --web                     # Open in browser
gh repo list                                      # Your repos
gh repo list owner                                # User's repos
gh repo rename new-name
gh repo delete owner/repo --yes
gh repo archive owner/repo
gh repo sync                                      # Sync fork with upstream

## --- Pull Requests ---
gh pr create                                      # Interactive
gh pr create --title "Fix bug" --body "Details"
gh pr create --title "Feature" --base main --head feature-branch
gh pr create --draft                              # Draft PR
gh pr create --assignee @me --reviewer user1,user2
gh pr create --label "bug" --milestone "v1.0"
gh pr list                                        # Open PRs
gh pr list --state all                            # All PRs
gh pr list --author @me                           # My PRs
gh pr list --search "is:open review-requested:@me"
gh pr view 123                                    # View PR details
gh pr view 123 --web                              # Open in browser
gh pr checkout 123                                # Checkout PR branch
gh pr diff 123                                    # View PR diff
gh pr merge 123                                   # Merge PR
gh pr merge 123 --squash                          # Squash merge
gh pr merge 123 --rebase                          # Rebase merge
gh pr merge 123 --delete-branch                   # Delete branch after
gh pr close 123                                   # Close PR
gh pr reopen 123
gh pr review 123 --approve
gh pr review 123 --comment --body "Looks good!"
gh pr review 123 --request-changes --body "Fix X"
gh pr ready 123                                   # Mark ready for review
gh pr checks 123                                  # View CI status
gh pr comment 123 --body "This is a comment"

## --- Issues ---
gh issue create                                   # Interactive
gh issue create --title "Bug" --body "Description"
gh issue create --label "bug" --assignee @me
gh issue list                                     # Open issues
gh issue list --state closed                      # Closed issues
gh issue list --label "bug"                       # Filter by label
gh issue list --assignee @me                      # My issues
gh issue view 456                                 # View issue
gh issue view 456 --web                           # Open in browser
gh issue close 456
gh issue reopen 456
gh issue comment 456 --body "Working on this"
gh issue edit 456 --add-label "priority"

## --- GitHub Actions ---
gh workflow list                                  # List workflows
gh workflow view                                  # View workflow
gh workflow run build.yml                         # Trigger workflow
gh workflow run build.yml -f param=value          # With parameters
gh workflow enable build.yml
gh workflow disable build.yml
gh run list                                       # Recent runs
gh run list --workflow=build.yml                  # Filter by workflow
gh run view 12345                                 # View run details
gh run view 12345 --log                           # View run logs
gh run watch 12345                                # Watch live
gh run rerun 12345                                # Re-run
gh run cancel 12345                               # Cancel run
gh run download 12345                             # Download artifacts

## --- Releases ---
gh release create v1.0.0                          # Create release
gh release create v1.0.0 --title "Release 1.0"
gh release create v1.0.0 --generate-notes         # Auto release notes
gh release create v1.0.0 ./dist/*.zip             # With assets
gh release list                                   # List releases
gh release view v1.0.0                            # View release
gh release download v1.0.0                        # Download assets
gh release delete v1.0.0 --yes

## --- Gists ---
gh gist create file.txt                           # Create gist
gh gist create file.txt --public                  # Public gist
gh gist create file1.txt file2.txt --desc "My gist"
gh gist list                                      # List gists
gh gist view <gist-id>
gh gist edit <gist-id>
gh gist delete <gist-id>

## --- Misc ---
gh browse                                         # Open repo in browser
gh browse --settings                              # Open settings
gh browse issues                                  # Open issues page
gh api repos/owner/repo                           # Raw API call
gh api repos/owner/repo/pulls/123/comments        # PR comments
gh search repos "keyword" --language=python       # Search repos
gh search code "function" --repo=owner/repo       # Search code


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  15. TMUX                                                                ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Sessions ---
tmux                                              # New session
tmux new -s mysession                             # Named session
tmux ls                                           # List sessions
tmux attach -t mysession                          # Attach to session
tmux a -t mysession                               # Short form attach
tmux kill-session -t mysession                    # Kill session
tmux kill-server                                  # Kill all sessions
tmux rename-session -t old new                    # Rename session

## --- Session Shortcuts (prefix = Ctrl+B) ---
# Ctrl+B d              Detach from session
# Ctrl+B s              List sessions (interactive)
# Ctrl+B $              Rename session
# Ctrl+B (              Switch to previous session
# Ctrl+B )              Switch to next session

## --- Windows ---
# Ctrl+B c              Create new window
# Ctrl+B w              List windows (interactive)
# Ctrl+B n              Next window
# Ctrl+B p              Previous window
# Ctrl+B 0-9            Switch to window by number
# Ctrl+B ,              Rename window
# Ctrl+B &              Kill window
# Ctrl+B f              Find window

## --- Panes ---
# Ctrl+B %              Split horizontally (left/right)
# Ctrl+B "              Split vertically (top/bottom)
# Ctrl+B Arrow          Move between panes
# Ctrl+B o              Cycle through panes
# Ctrl+B x              Kill current pane
# Ctrl+B z              Toggle pane zoom (fullscreen)
# Ctrl+B {              Move pane left
# Ctrl+B }              Move pane right
# Ctrl+B Space          Toggle pane layouts
# Ctrl+B q              Show pane numbers
# Ctrl+B q 0-9          Switch to pane by number
# Ctrl+B !              Convert pane to window

## --- Resize Panes ---
# Ctrl+B :resize-pane -D 5     Resize down 5 rows
# Ctrl+B :resize-pane -U 5     Resize up 5 rows
# Ctrl+B :resize-pane -L 5     Resize left 5 columns
# Ctrl+B :resize-pane -R 5     Resize right 5 columns

## --- Copy Mode ---
# Ctrl+B [              Enter copy mode
# q                     Exit copy mode
# Space                 Start selection (in copy mode)
# Enter                 Copy selection (in copy mode)
# Ctrl+B ]              Paste buffer

## --- Useful Commands ---
tmux send-keys -t mysession "command" Enter       # Send command to session
tmux source-file ~/.tmux.conf                     # Reload config
tmux list-keys                                    # List all key bindings
tmux list-commands                                # List all commands


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  16. VIM                                                                  ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Modes ---
# i         Enter Insert mode (before cursor)
# a         Enter Insert mode (after cursor)
# I         Insert at beginning of line
# A         Insert at end of line
# o         Open new line below
# O         Open new line above
# v         Visual mode (character)
# V         Visual mode (line)
# Ctrl+V    Visual Block mode
# Esc       Return to Normal mode
# R         Replace mode

## --- Navigation ---
# h j k l        Left, Down, Up, Right
# w              Next word start
# b              Previous word start
# e              End of word
# W B E          Same but WORD (whitespace-delimited)
# 0              Start of line
# ^              First non-blank character
# $              End of line
# gg             First line of file
# G              Last line of file
# 5G or :5       Go to line 5
# Ctrl+U         Half page up
# Ctrl+D         Half page down
# Ctrl+B         Full page up
# Ctrl+F         Full page down
# H              Top of screen
# M              Middle of screen
# L              Bottom of screen
# zz             Center current line on screen
# zt             Current line at top
# zb             Current line at bottom
# %              Jump to matching bracket
# f{char}        Jump to next {char} on line
# F{char}        Jump to prev {char} on line
# ;              Repeat last f/F
# ,              Repeat last f/F (reverse)
# *              Search word under cursor (forward)
# #              Search word under cursor (backward)

## --- Editing ---
# x              Delete character
# dd             Delete line
# dw             Delete word
# d$  or  D      Delete to end of line
# d0             Delete to start of line
# diw            Delete inner word
# di(            Delete inside parentheses
# di"            Delete inside quotes
# dip            Delete inner paragraph
# cc             Change (replace) entire line
# cw             Change word
# c$  or  C      Change to end of line
# ci(            Change inside parentheses
# ci"            Change inside quotes
# yy             Yank (copy) line
# yw             Yank word
# y$             Yank to end of line
# p              Paste after cursor
# P              Paste before cursor
# u              Undo
# Ctrl+R         Redo
# .              Repeat last command
# r{char}        Replace single character
# ~              Toggle case
# >>             Indent line
# <<             Unindent line
# J              Join line below to current
# gU{motion}     Uppercase
# gu{motion}     Lowercase

## --- Search & Replace ---
# /pattern       Search forward
# ?pattern       Search backward
# n              Next match
# N              Previous match
# :%s/old/new/g         Replace all in file
# :%s/old/new/gc        Replace all with confirmation
# :s/old/new/g          Replace all in current line
# :%s/old/new/gi        Case-insensitive replace
# :5,10s/old/new/g      Replace in lines 5-10

## --- Visual Mode Operations ---
# v + motion     Select text
# V              Select lines
# Ctrl+V         Block select
# d              Delete selection
# y              Yank (copy) selection
# >              Indent selection
# <              Unindent selection
# =              Auto-indent selection
# U              Uppercase selection
# u              Lowercase selection

## --- Save & Exit ---
# :w             Save
# :w filename    Save as
# :q             Quit
# :q!            Quit without saving
# :wq            Save and quit
# :x             Save and quit (same as :wq)
# ZZ             Save and quit (shortcut)
# ZQ             Quit without saving (shortcut)
# :wa            Save all buffers
# :qa            Quit all buffers
# :qa!           Quit all without saving

## --- Buffers & Windows ---
# :e filename    Open file in buffer
# :bn            Next buffer
# :bp            Previous buffer
# :bd            Close buffer
# :ls            List buffers
# :sp filename   Horizontal split
# :vsp filename  Vertical split
# Ctrl+W w       Switch between splits
# Ctrl+W h/j/k/l Navigate splits
# Ctrl+W =       Equal size splits
# :tabnew        New tab
# gt             Next tab
# gT             Previous tab

## --- Marks & Macros ---
# m{a-z}         Set mark
# '{a-z}         Jump to mark
# qa             Start recording macro to register 'a'
# q              Stop recording
# @a             Play macro 'a'
# @@             Replay last macro
# 10@a           Play macro 'a' 10 times


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  17. REGEX (Common Patterns)                                             ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Character Classes ---
# .              Any character (except newline)
# \d             Digit [0-9]
# \D             Not a digit
# \w             Word character [a-zA-Z0-9_]
# \W             Not a word character
# \s             Whitespace (space, tab, newline)
# \S             Not whitespace
# \b             Word boundary
# [abc]          Character set (a, b, or c)
# [^abc]         Not in set
# [a-z]          Range
# [A-Za-z0-9]    Alphanumeric

## --- Quantifiers ---
# *              0 or more
# +              1 or more
# ?              0 or 1
# {3}            Exactly 3
# {3,}           3 or more
# {3,5}          3 to 5
# *?  +?  ??     Non-greedy (lazy) versions

## --- Anchors & Groups ---
# ^              Start of string/line
# $              End of string/line
# (abc)          Capture group
# (?:abc)        Non-capture group
# (?=abc)        Positive lookahead
# (?!abc)        Negative lookahead
# (?<=abc)       Positive lookbehind
# (?<!abc)       Negative lookbehind
# \1 \2          Back-reference to group 1, 2
# a|b            Alternation (a or b)

## --- Common Validation Patterns ---

# Email
^[a-zA-Z0-9._%+-]+@[a-zA-Z0-9.-]+\.[a-zA-Z]{2,}$

# URL (with protocol)
https?:\/\/(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z]{2,6}\b([-a-zA-Z0-9@:%_\+.~#?&//=]*)

# URL (protocol optional)
(https?:\/\/)?(www\.)?[-a-zA-Z0-9@:%._\+~#=]{1,256}\.[a-zA-Z]{2,6}\b([-a-zA-Z0-9@:%_\+.~#?&//=]*)

# IPv4 Address
^((25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)$

# IPv6 Address
^([0-9a-fA-F]{1,4}:){7}[0-9a-fA-F]{1,4}$

# US Phone Number (flexible)
^(\+1)?[-.\s]?\(?\d{3}\)?[-.\s]?\d{3}[-.\s]?\d{4}$

# International Phone
^\+?[1-9]\d{1,14}$

# Date (YYYY-MM-DD)
^\d{4}-(0[1-9]|1[0-2])-(0[1-9]|[12]\d|3[01])$

# Date (MM/DD/YYYY)
^(0[1-9]|1[0-2])\/(0[1-9]|[12]\d|3[01])\/\d{4}$

# Time (HH:MM 24hr)
^([01]\d|2[0-3]):([0-5]\d)$

# Username (3-16 chars, alphanumeric, underscore, hyphen)
^[a-zA-Z0-9_-]{3,16}$

# Strong Password (8+ chars, upper, lower, digit, special)
^(?=.*[a-z])(?=.*[A-Z])(?=.*\d)(?=.*[@$!%*?&])[A-Za-z\d@$!%*?&]{8,}$

# Hex Color
^#?([a-fA-F0-9]{6}|[a-fA-F0-9]{3})$

# HTML Tag
<\/?[\w\s]*>|<.+[\W]>

# Zip Code (US)
^\d{5}(-\d{4})?$

# Credit Card (basic)
^(?:4[0-9]{12}(?:[0-9]{3})?|5[1-5][0-9]{14}|3[47][0-9]{13}|6(?:011|5[0-9]{2})[0-9]{12})$

# SSN (US)
^\d{3}-\d{2}-\d{4}$

# Slug (URL-friendly)
^[a-z0-9]+(?:-[a-z0-9]+)*$

# File Path (Unix)
^\/(?:[^\/]+\/)*[^\/]+$

# Whitespace trimming (for replacement)
^\s+|\s+$

# Duplicate words
\b(\w+)\s+\1\b

# Only digits
^\d+$

# Only alphabetic
^[a-zA-Z]+$

# Only alphanumeric
^[a-zA-Z0-9]+$

# Domain name
^([a-zA-Z0-9]([a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?\.)+[a-zA-Z]{2,}$

# UUID
^[0-9a-fA-F]{8}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{4}-[0-9a-fA-F]{12}$

# JSON String
"(?:[^"\\]|\\.)*"

# Semantic Version
^(0|[1-9]\d*)\.(0|[1-9]\d*)\.(0|[1-9]\d*)(?:-((?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*)(?:\.(?:0|[1-9]\d*|\d*[a-zA-Z-][0-9a-zA-Z-]*))*))?(?:\+([0-9a-zA-Z-]+(?:\.[0-9a-zA-Z-]+)*))?$


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  18. WINDOWS CMD / POWERSHELL                                            ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- CMD Essentials ---
dir                                               # List directory
dir /s /b *.txt                                   # Recursive search
cd \path\to\dir                                   # Change directory
cd ..                                             # Parent directory
mkdir dirname                                     # Create directory
rmdir /s /q dirname                               # Remove directory
copy source dest                                  # Copy file
xcopy /s /e source dest                           # Copy directory tree
robocopy source dest /MIR                         # Mirror directories
move source dest                                  # Move/rename
del file.txt                                      # Delete file
del /s /q *.tmp                                   # Delete recursively
ren oldname newname                               # Rename
type file.txt                                     # Display file contents
more file.txt                                     # Paginated display
echo text > file.txt                              # Write to file
echo text >> file.txt                             # Append to file
cls                                               # Clear screen
set VAR=value                                     # Set variable
echo %VAR%                                        # Print variable
setx VAR "value"                                  # Permanent env variable
path                                              # Show PATH
where command                                     # Find executable
whoami                                            # Current user

## --- System & Network (CMD) ---
systeminfo                                        # System information
tasklist                                          # List processes
tasklist | findstr "chrome"                       # Find process
taskkill /PID 1234 /F                             # Kill process
taskkill /IM "chrome.exe" /F                      # Kill by name
ipconfig                                          # Network config
ipconfig /all                                     # Detailed
ipconfig /flushdns                                # Flush DNS
ipconfig /release                                 # Release DHCP
ipconfig /renew                                   # Renew DHCP
ping google.com                                   # Connectivity test
tracert google.com                                # Trace route
nslookup google.com                               # DNS lookup
netstat -an                                       # Network connections
netstat -ano | findstr :8080                      # Find port usage
arp -a                                            # ARP table
route print                                       # Routing table
hostname                                          # Computer name
shutdown /s /t 0                                  # Shutdown now
shutdown /r /t 0                                  # Restart now
shutdown /l                                       # Log off
sfc /scannow                                      # System file checker
chkdsk C: /f                                      # Check disk
diskpart                                          # Disk management

## --- Windows PowerShell Additions ---
Get-Clipboard                                     # Get clipboard content
Set-Clipboard "text"                              # Set clipboard
Get-FileHash file.txt                             # Get file hash
Get-FileHash file.txt -Algorithm SHA256
Compress-Archive -Path dir -DestinationPath file.zip   # Create zip
Expand-Archive file.zip -DestinationPath dir           # Extract zip
Start-Transcript -Path log.txt                    # Record session
Stop-Transcript                                   # Stop recording
Get-ExecutionPolicy                               # Check execution policy
Set-ExecutionPolicy RemoteSigned -Scope CurrentUser
Get-Alias                                         # List all aliases
Set-Alias ll Get-ChildItem                        # Create alias
Get-Command *service*                             # Find commands by keyword
Get-Help Get-Process -Full                        # Detailed help
Update-Help                                       # Update help files


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  19. ODOO                                                                ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Server Management ---
odoo --version                                    # Check version
python odoo-bin --version                         # Alternative

# Start server (common patterns)
python odoo-bin -c odoo.conf                      # Start with config file
python odoo-bin -d mydb -r dbuser -w dbpass       # Specify DB credentials
python odoo-bin --addons-path=addons,custom_addons
python odoo-bin -d mydb --addons-path=addons,/path/to/custom
python odoo-bin -d mydb -p 8069                   # Specify port
python odoo-bin -d mydb --http-port=8069
python odoo-bin -d mydb --xmlrpc-port=8069
python odoo-bin --db-filter=^mydb$                # Only show this DB
python odoo-bin --workers=4                       # Multi-worker mode
python odoo-bin --proxy-mode                      # Behind reverse proxy
python odoo-bin --limit-memory-hard=2684354560    # Memory limit
python odoo-bin --log-level=debug                 # Debug logging
python odoo-bin --dev=all                         # Dev mode (auto-reload)
python odoo-bin --dev=xml                         # Reload XML only
python odoo-bin --dev=qweb                        # Reload QWeb templates

## --- Module Operations ---
# Install module
python odoo-bin -d mydb -i module_name
python odoo-bin -d mydb -i module1,module2,module3

# Update module
python odoo-bin -d mydb -u module_name
python odoo-bin -d mydb -u module1,module2
python odoo-bin -d mydb -u all                    # Update all modules

# Install + Update combined
python odoo-bin -d mydb -i new_module -u existing_module

# Common flags with module operations
python odoo-bin -d mydb -u my_module --addons-path=addons,custom --stop-after-init
python odoo-bin -d mydb -u my_module --no-http    # Update without starting server

## --- Scaffolding (Generate Module Structure) ---
python odoo-bin scaffold my_module /path/to/addons/
python odoo-bin scaffold my_module ./custom_addons/

# This creates:
# my_module/
#   __init__.py
#   __manifest__.py
#   controllers/
#   models/
#   views/
#   security/
#   static/

## --- Shell (Interactive Python Console) ---
python odoo-bin shell -d mydb
python odoo-bin shell -d mydb --addons-path=addons,custom

# Inside shell:
# env['res.partner'].search([])
# env['res.partner'].search([('name', 'ilike', 'admin')])
# env['res.partner'].browse(1).name
# env['res.partner'].create({'name': 'New Partner'})
# env.cr.commit()
# partner = env['res.partner'].browse(1)
# partner.write({'name': 'Updated Name'})
# env.cr.commit()
# env['sale.order'].search_count([('state', '=', 'sale')])

## --- Database Operations ---
python odoo-bin db --help
# Dump database
python odoo-bin db dump mydb backup.zip
# Duplicate database
python odoo-bin db duplicate mydb mydb_copy
# Drop database
python odoo-bin db drop mydb
# Rename database
python odoo-bin db rename mydb newdb

## --- Testing ---
python odoo-bin -d testdb -i my_module --test-enable --stop-after-init
python odoo-bin -d testdb -u my_module --test-enable --stop-after-init
python odoo-bin -d testdb --test-tags=/my_module --stop-after-init
python odoo-bin -d testdb --test-tags=my_module:MyTestClass --stop-after-init

## --- Utility ---
python odoo-bin cloc -d mydb                      # Count lines of code
python odoo-bin cloc --path /path/to/module       # LOC for path
python odoo-bin populate -d mydb                  # Populate test data
python odoo-bin neutralize -d mydb                # Neutralize DB (remove crons, emails)

## --- Config File (odoo.conf) Essential Options ---
# [options]
# addons_path = /opt/odoo/addons,/opt/odoo/custom
# db_host = localhost
# db_port = 5432
# db_user = odoo
# db_password = odoo
# db_name = mydb
# http_port = 8069
# admin_passwd = admin_master_password
# workers = 4
# limit_memory_hard = 2684354560
# limit_memory_soft = 2147483648
# limit_time_cpu = 600
# limit_time_real = 1200
# proxy_mode = True
# logfile = /var/log/odoo/odoo.log
# log_level = info
# dbfilter = ^%d$
# list_db = False

## --- Systemd Service for Odoo ---
# /etc/systemd/system/odoo.service
# [Unit]
# Description=Odoo
# After=network.target postgresql.service
#
# [Service]
# Type=simple
# User=odoo
# ExecStart=/opt/odoo/odoo-bin -c /etc/odoo/odoo.conf
# Restart=always
#
# [Install]
# WantedBy=multi-user.target


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  20. CLAUDE CODE CLI                                                     ║
# ╚══════════════════════════════════════════════════════════════════════════╝

## --- Installation ---
curl -sL https://install.anthropic.com | sh       # Linux/Mac
npm install -g @anthropic-ai/claude-code           # Via npm

## --- Basic Usage ---
claude                                            # Start interactive REPL
claude "explain this project"                     # Start with prompt
claude -p "explain this function"                 # Print mode (non-interactive)
claude -c                                         # Continue last conversation
claude -c -p "check for type errors"              # Continue in print mode
claude -r "session-name" "finish this PR"         # Resume session by name/ID
claude --version                                  # Check version
claude -v                                         # Short version
claude update                                     # Update to latest

## --- Authentication ---
claude auth login                                 # Sign in
claude auth login --email user@example.com        # Pre-fill email
claude auth login --email user@example.com --sso  # Force SSO
claude auth logout                                # Sign out
claude auth status                                # Auth status (JSON)
claude auth status --text                         # Human-readable

## --- Model Selection ---
claude --model sonnet                             # Use Sonnet
claude --model opus                               # Use Opus
claude --model claude-sonnet-4-6                  # Specific model ID

## --- System Prompt Customization ---
claude --append-system-prompt "Always use TypeScript"          # Add to default prompt
claude --system-prompt "You are a Python expert"               # Replace entire prompt
claude -p --system-prompt-file ./prompt.txt "query"            # From file (replaces)
claude -p --append-system-prompt-file ./rules.txt "query"      # From file (appends)

## --- Input/Output ---
cat file.txt | claude -p "explain this"           # Pipe content
claude -p "query" --output-format json            # JSON output
claude -p "query" --output-format stream-json     # Streaming JSON
cat logs.txt | claude -p "summarize errors"       # Process piped content

## --- Permission & Safety ---
claude --permission-mode plan                     # Plan mode (read-only)
claude --dangerously-skip-permissions             # Skip all permission prompts
claude --allowedTools "Bash(git log *)" "Read"    # Allow specific tools
claude --disallowedTools "Bash(rm *)" "Edit"      # Block specific tools
claude --tools "Bash,Edit,Read"                   # Restrict to specific tools
claude --tools ""                                 # Disable all tools

## --- Advanced Options ---
claude --add-dir ../apps ../lib                   # Additional working directories
claude --max-turns 3 -p "query"                   # Limit agent turns
claude --max-budget-usd 5.00 -p "query"           # Spending limit
claude --verbose                                  # Verbose logging
claude --debug "api,mcp"                          # Debug specific categories
claude -w feature-auth                            # Start in git worktree
claude --mcp-config ./mcp.json                    # Load MCP servers
claude --fallback-model sonnet -p "query"         # Fallback model
claude --no-session-persistence -p "query"        # Don't save session
claude --chrome                                   # Enable browser integration

## --- Agents & Subagents ---
claude agents                                     # List configured agents
claude --agent my-custom-agent                    # Use specific agent
claude --agents '{"reviewer":{"description":"Reviews code","prompt":"You are a reviewer"}}'

## --- Configuration ---
claude config list                                # List project settings
claude config list --global                       # List global settings
claude config set key value                       # Set config
claude config get key                             # Get config

## --- MCP Servers ---
claude mcp add <server-name> <command>            # Add MCP server
claude mcp list                                   # List MCP servers
claude mcp remove <server-name>                   # Remove MCP server

## --- Slash Commands (Inside Interactive Session) ---
# /help           Show available commands
# /init           Create CLAUDE.md for project
# /clear          Clear conversation history
# /compact        Summarize conversation (save context)
# /cost           Show token usage & cost
# /doctor         Check system health
# /login          Switch account
# /logout         Sign out
# /model          Change model
# /permissions    View/manage permissions
# /review         Review changes made
# /status         Show session info
# /terminal-setup Configure terminal integration
# /vim            Toggle Vim keybindings
# /fast           Toggle fast mode (faster output)
# Shift+Tab       Toggle plan mode (Accept/Edit/Reject)


# ╔══════════════════════════════════════════════════════════════════════════╗
# ║  END OF CHEATSHEET                                                       ║
# ╚══════════════════════════════════════════════════════════════════════════╝
