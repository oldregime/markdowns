# Linux Command Templates & Formatting Guide

## 📋 Command Template Structure

Every Linux command follows this general pattern:

```bash
command [options] [arguments] [target]
  │       │          │          │
  │       │          │          └─ What to act on (file, directory, etc.)
  │       │          └─ Additional parameters or values
  │       └─ Flags/switches that modify behavior
  └─ The program/utility to run
```

---

## 🗂️ File System Commands

### `ls` - List Directory Contents

**Template:**
```bash
ls [OPTIONS] [DIRECTORY]
│   │          │
│   │          └─ Where to list (default: current directory)
│   └─ How to display
└─ List command
```

**Common Options:**
```bash
ls                    # Simple list of current directory
ls -l                 # Long format (permissions, owner, size, date)
ls -a                 # All files (including hidden files starting with .)
ls -h                 # Human-readable sizes (KB, MB, GB instead of bytes)
ls -R                 # Recursive (show subdirectories too)
ls -t                 # Sort by modification time (newest first)
ls -S                 # Sort by size (largest first)
ls -r                 # Reverse order

# Combined options
ls -lah configs/      # Long format + All files + Human-readable in configs/
│  │││  │
│  │││  └─ Target directory
│  ││└─ h = human-readable sizes
│  │└─ a = show hidden files
│  └─ l = long format
└─ list command
```

**Practical Examples:**
```bash
# List with details
ls -lah                           # Current directory, all files, detailed
ls -lh /var/log/                  # System logs with readable sizes
ls -lt ~/luganode2025/            # Sort by modification time

# Find specific files
ls -la | grep ".conf"             # Only show .conf files
ls -lhS | head -10                # 10 largest files

# Recursive listing
ls -R ansible/                    # Show all subdirectories
ls -lR configs/ | grep ".yml"     # Find all YAML files recursively
```

---

### `cd` - Change Directory

**Template:**
```bash
cd [DIRECTORY]
│   │
│   └─ Where to go
└─ Change directory command
```

**Special Paths:**
```bash
cd /absolute/path     # Absolute path (starts with /)
cd relative/path      # Relative to current location
cd ..                 # Up one directory (parent)
cd ../..              # Up two directories
cd ~                  # Home directory (/home/username)
cd -                  # Previous directory (toggle back)
cd                    # Home directory (same as cd ~)

# Your project examples
cd ~/luganode2025/luganode25              # Absolute path to project
cd luganode25/configs                     # Relative path from current dir
cd ../../                                 # Up two levels
cd ~/luganode2025/luganode25/ansible && ls  # Change and list
```

---

### `pwd` - Print Working Directory

**Template:**
```bash
pwd [OPTIONS]
│    │
│    └─ Usually no options needed
└─ Print working directory
```

**Usage:**
```bash
pwd                   # Show current directory path
pwd -P                # Show physical path (resolve symlinks)

# In scripts
CURRENT_DIR=$(pwd)    # Save current directory to variable
echo "Working in: $(pwd)"
```

---

### `mkdir` - Make Directory

**Template:**
```bash
mkdir [OPTIONS] DIRECTORY_NAME
│      │         │
│      │         └─ Name of directory to create
│      └─ How to create
└─ Make directory command
```

**Options:**
```bash
mkdir mydir                      # Create single directory
mkdir -p path/to/nested/dir      # Create parent directories as needed
│      │  │
│      │  └─ Will create 'path', 'to', 'nested', and 'dir'
│      └─ p = parents (create intermediate directories)
└─ make directory

mkdir -m 755 mydir               # Create with specific permissions
mkdir dir1 dir2 dir3             # Create multiple directories

# Practical examples
mkdir -p ~/projects/bitcoin/logs           # Create nested structure
mkdir -p configs/{bitcoin,loki,grafana}    # Create multiple subdirs
mkdir backup-$(date +%Y%m%d)              # Create with timestamp
```

---

### `cp` - Copy Files/Directories

**Template:**
```bash
cp [OPTIONS] SOURCE DESTINATION
│   │         │       │
│   │         │       └─ Where to copy to
│   │         └─ What to copy
│   └─ How to copy
└─ Copy command
```

**Options:**
```bash
cp file.txt backup.txt            # Copy file
cp -r directory/ backup/          # Copy directory recursively
│  │  │           │
│  │  │           └─ Destination directory
│  │  └─ Source directory
│  └─ r = recursive (needed for directories)
└─ copy command

cp -i file.txt dest.txt           # Interactive (prompt before overwrite)
cp -v file.txt dest.txt           # Verbose (show what's being copied)
cp -p file.txt dest.txt           # Preserve attributes (permissions, timestamps)
cp -u source.txt dest.txt         # Update (copy only if source is newer)

# Practical examples
cp configs/bitcoin/bitcoin.conf configs/bitcoin/bitcoin.conf.bak
cp -r configs/ configs-backup/
cp -v scripts/*.sh backup/        # Copy all .sh files with verbose
```

---

### `mv` - Move/Rename Files

**Template:**
```bash
mv [OPTIONS] SOURCE DESTINATION
│   │         │       │
│   │         │       └─ Where to move to (or new name)
│   │         └─ What to move
│   └─ How to move
└─ Move command
```

**Usage:**
```bash
# Rename file
mv oldname.txt newname.txt
│  │           │
│  │           └─ New name
│  └─ Old name
└─ move/rename

# Move file to directory
mv file.txt /path/to/directory/
mv file.txt ../                   # Move to parent directory

# Move multiple files
mv file1.txt file2.txt directory/
mv *.log logs/                    # Move all .log files

# Options
mv -i file.txt dest.txt           # Interactive (confirm overwrite)
mv -v file.txt dest.txt           # Verbose (show action)
mv -n file.txt dest.txt           # No-clobber (don't overwrite)

# Practical examples
mv docker-compose.yml docker-compose.yml.old
mv configs/old/* configs/archive/
mv -i *.conf /etc/myapp/          # Move configs with confirmation
```

---

### `rm` - Remove Files/Directories

**Template:**
```bash
rm [OPTIONS] FILE/DIRECTORY
│   │         │
│   │         └─ What to remove
│   └─ How to remove (BE CAREFUL!)
└─ Remove command
```

**Options:**
```bash
rm file.txt                       # Remove file
rm -i file.txt                    # Interactive (confirm before delete)
│  │  │
│  │  └─ File to remove
│  └─ i = interactive
└─ remove

rm -r directory/                  # Recursive (remove directory and contents)
rm -f file.txt                    # Force (no confirmation, ignore errors)
rm -rf directory/                 # ⚠️  DANGEROUS: Force remove directory

rm -v file.txt                    # Verbose (show what's being removed)

# Practical examples
rm *.log                          # Remove all .log files
rm -i configs/*.bak               # Remove backups with confirmation
rm -rf /tmp/old-project/          # ⚠️  Force remove (use with caution!)

# Safer pattern: use find with -delete
find . -name "*.tmp" -delete      # Delete .tmp files
```

**⚠️ SAFETY TIP:**
```bash
# NEVER run these (system destruction):
rm -rf /                          # ❌ Deletes everything
rm -rf /*                         # ❌ Deletes everything
rm -rf ~/*                        # ❌ Deletes your home directory

# Safer practices:
ls -la *.log                      # ✅ First, list what would be deleted
rm -i *.log                       # ✅ Then delete with confirmation
```

---

## 🔍 Search & Find Commands

### `find` - Search for Files

**Template:**
```bash
find [PATH] [OPTIONS] [EXPRESSION]
│     │      │         │
│     │      │         └─ What to match (name, size, type, etc.)
│     │      └─ How to search
│     └─ Where to search
└─ Find command
```

**Common Patterns:**
```bash
# Find by name
find . -name "*.log"              # Find .log files in current directory
│    │  │      │
│    │  │      └─ Pattern to match
│    │  └─ name = match filename
│    └─ . = current directory
└─ find

find /var/log -name "*.gz"        # Find compressed logs
find ~ -name "docker-compose.yml" # Find compose files in home

# Find by type
find . -type f                    # Files only
find . -type d                    # Directories only
find . -type l                    # Symbolic links only

# Find by size
find . -size +100M                # Files larger than 100MB
find . -size -1M                  # Files smaller than 1MB
find /var/log -size +10M -size -100M  # Between 10MB and 100MB

# Find by time
find . -mtime -1                  # Modified in last 24 hours
find . -mtime +30                 # Modified more than 30 days ago
find . -mmin -60                  # Modified in last 60 minutes

# Execute commands on found files
find . -name "*.log" -delete      # Delete all .log files
find . -name "*.txt" -exec cat {} \;  # Cat all .txt files

# Practical examples
find ~/luganode2025 -name "*.yml"
find . -type f -name "*.sh" -exec chmod +x {} \;
find /var/log -name "*.log" -mtime +7 -delete
find . -type f -size +50M -exec ls -lh {} \;
```

---

### `grep` - Search Inside Files

**Template:**
```bash
grep [OPTIONS] PATTERN [FILE...]
│     │         │        │
│     │         │        └─ Files to search (stdin if omitted)
│     │         └─ Text pattern to find
│     └─ How to search
└─ Global regular expression print
```

**Options:**
```bash
# Basic search
grep "error" file.log             # Find "error" in file.log
│     │       │
│     │       └─ File to search
│     └─ Pattern to find
└─ grep command

grep -i "ERROR" file.log          # Case-insensitive
grep -v "debug" file.log          # Invert match (exclude lines with "debug")
grep -n "error" file.log          # Show line numbers
grep -c "error" file.log          # Count matches

# Multiple files
grep "error" *.log                # Search all .log files
grep -r "error" /var/log/         # Recursive search in directory
grep -l "error" *.log             # Show filenames only (not content)

# Context
grep -A 3 "error" file.log        # Show 3 lines After match
grep -B 3 "error" file.log        # Show 3 lines Before match
grep -C 3 "error" file.log        # Show 3 lines Context (before + after)

# Advanced patterns
grep -E "error|warn" file.log     # Extended regex (OR operator)
grep "^error" file.log            # Lines starting with "error"
grep "error$" file.log            # Lines ending with "error"
grep -w "error" file.log          # Match whole word only

# Practical examples from your project
docker compose logs bitcoin | grep "error"
grep -r "port.*3000" configs/
grep -i "alert" configs/grafana/provisioning/alerting/rules.yml
grep -v "^#" configs/bitcoin/bitcoin.conf | grep -v "^$"  # Skip comments and empty lines
```

---

## 📝 Text Viewing Commands

### `cat` - Concatenate and Display Files

**Template:**
```bash
cat [OPTIONS] [FILE...]
│    │         │
│    │         └─ Files to display
│    └─ How to display
└─ Concatenate command
```

**Usage:**
```bash
cat file.txt                      # Display entire file
cat file1.txt file2.txt           # Display multiple files
cat file.txt > newfile.txt        # Redirect to new file
cat file1.txt file2.txt > combined.txt  # Combine files

# Options
cat -n file.txt                   # Show line numbers
cat -b file.txt                   # Number non-blank lines only
cat -s file.txt                   # Squeeze multiple blank lines

# Practical examples
cat configs/bitcoin/bitcoin.conf
cat docker-compose.yml | grep ports
cat > newfile.txt                 # Create file (Ctrl+D to save)
```

---

### `less` / `more` - Page Through Files

**Template:**
```bash
less [OPTIONS] FILE
│     │         │
│     │         └─ File to view
│     └─ How to view
└─ Pager command
```

**Usage:**
```bash
less file.log                     # Open file in pager
│    │
│    └─ File to view
└─ less (better than more)

# Navigation in less:
# Space = next page
# b = previous page
# / = search forward
# ? = search backward
# n = next search result
# N = previous search result
# q = quit
# G = go to end
# g = go to start

# Options
less -N file.log                  # Show line numbers
less +F file.log                  # Follow mode (like tail -f)
less -S file.log                  # Don't wrap long lines

# Practical examples
less /var/log/syslog
docker compose logs bitcoin | less
journalctl -u docker | less +G    # Start at end
```

---

### `head` / `tail` - View File Beginning/End

**Template:**
```bash
head [OPTIONS] [FILE]
│     │         │
│     │         └─ File to view
│     └─ How many lines
└─ Show beginning of file

tail [OPTIONS] [FILE]
│     │         │
│     │         └─ File to view
│     └─ How many lines / follow
└─ Show end of file
```

**Usage:**
```bash
# head - first lines
head file.txt                     # First 10 lines (default)
head -n 20 file.txt               # First 20 lines
│     │  │
│     │  └─ Number of lines
│     └─ n = number
└─ head

head -n 5 *.log                   # First 5 lines of each .log file

# tail - last lines
tail file.txt                     # Last 10 lines (default)
tail -n 50 file.log               # Last 50 lines
tail -f file.log                  # Follow (live updates)
│     │  │
│     │  └─ File to follow
│     └─ f = follow
└─ tail

tail -F file.log                  # Follow even if file rotates

# Practical examples
head -20 configs/bitcoin/bitcoin.conf
tail -f /var/log/syslog
docker compose logs bitcoin | tail -100
docker compose logs -f bitcoin    # Built-in follow
```

---

## 🔧 Process Management Commands

### `ps` - Process Status

**Template:**
```bash
ps [OPTIONS]
│   │
│   └─ Which processes and how to display
└─ Process status command
```

**Common Patterns:**
```bash
ps                                # Current shell processes only
ps aux                            # All processes, detailed
│  │││
│  ││└─ x = show processes without controlling terminal
│  │└─ u = user-oriented format
│  └─ a = all users
└─ process status

ps -ef                            # All processes, full format
ps -aux --sort=-%mem              # Sort by memory usage
ps -aux --sort=-%cpu              # Sort by CPU usage

# Find specific process
ps aux | grep docker
ps aux | grep bitcoin | grep -v grep

# Practical examples
ps aux | grep grafana
ps -ef --forest                   # Show process tree
ps -eo pid,ppid,cmd,%mem,%cpu --sort=-%mem | head
```

---

### `top` / `htop` - Interactive Process Viewer

**Template:**
```bash
top [OPTIONS]
│    │
│    └─ Display options
└─ Table of processes
```

**Usage:**
```bash
top                               # Interactive process viewer
top -b -n 1                       # Batch mode, one iteration
│   │  │  │
│   │  │  └─ Number of iterations
│   │  └─ n = number
│   └─ b = batch mode
└─ top

# Inside top (interactive keys):
# M = sort by memory
# P = sort by CPU
# T = sort by time
# k = kill process (prompts for PID)
# q = quit
# 1 = toggle individual CPU cores
# h = help

htop                              # Better interactive viewer (needs install)
# Mouse support, easier to read, tree view
```

---

### `kill` - Terminate Processes

**Template:**
```bash
kill [SIGNAL] PID
│     │       │
│     │       └─ Process ID to kill
│     └─ Signal to send (optional, default=15)
└─ Kill command
```

**Signals:**
```bash
kill 1234                         # Send SIGTERM (15) - graceful shutdown
kill -15 1234                     # Same as above
│     │  │
│     │  └─ Process ID
│     └─ Signal number
└─ kill

kill -9 1234                      # SIGKILL - force kill (cannot be caught)
kill -HUP 1234                    # SIGHUP - reload configuration
kill -1 1234                      # Same as -HUP

# Kill by name
killall firefox                   # Kill all firefox processes
pkill -f "bitcoin-signet"         # Kill by pattern

# List signals
kill -l                           # Show all signals

# Practical examples
kill $(pgrep grafana-server)
pkill -9 -f bitcoin
docker compose kill bitcoin       # Docker's kill (sends SIGKILL)
docker compose stop bitcoin       # Docker's stop (sends SIGTERM, then SIGKILL)
```

---

## 💾 Disk & System Commands

### `df` - Disk Free Space

**Template:**
```bash
df [OPTIONS] [FILESYSTEM]
│   │         │
│   │         └─ Specific filesystem (optional)
│   └─ Display format
└─ Disk free command
```

**Usage:**
```bash
df                                # Show all mounted filesystems
df -h                             # Human-readable sizes
│  │
│  └─ h = human-readable (KB, MB, GB)
└─ disk free

df -h /                           # Show root filesystem only
df -h /var/lib/docker             # Show Docker's filesystem
df -i                             # Show inode usage
df -T                             # Show filesystem type

# Practical examples
df -h | grep -v tmpfs             # Exclude temporary filesystems
df -h | sort -k5 -r               # Sort by usage percentage
```

---

### `du` - Disk Usage

**Template:**
```bash
du [OPTIONS] [DIRECTORY/FILE]
│   │         │
│   │         └─ What to measure
│   └─ How to display
└─ Disk usage command
```

**Usage:**
```bash
du                                # Current directory, all subdirs
du -h                             # Human-readable
du -sh                            # Summary (total only)
│  ││
│  │└─ h = human-readable
│  └─ s = summary
└─ disk usage

du -sh *                          # Size of each item in current dir
du -h --max-depth=1               # Limit subdirectory depth
du -ah                            # All files (not just directories)

# Practical examples
du -sh ~/luganode2025/luganode25/*
du -sh /var/lib/docker/volumes/*
du -h | sort -h | tail -20        # 20 largest items
du -sh . && du -sh * | sort -h    # Total + breakdown
```

---

### `free` - Memory Usage

**Template:**
```bash
free [OPTIONS]
│     │
│     └─ Display format
└─ Free memory command
```

**Usage:**
```bash
free                              # Memory in kilobytes
free -h                           # Human-readable
│     │
│     └─ h = human-readable
└─ free memory

free -m                           # Megabytes
free -g                           # Gigabytes
free -s 5                         # Update every 5 seconds
free -h --si                      # Use 1000 instead of 1024

# Output columns:
# total = Total installed RAM
# used = Used RAM
# free = Completely unused RAM
# shared = Shared memory
# buff/cache = Cache and buffers (can be freed if needed)
# available = Memory available for new applications
```

---

## 🌐 Network Commands

### `curl` - Transfer Data from URLs

**Template:**
```bash
curl [OPTIONS] URL
│     │         │
│     │         └─ URL to fetch
│     └─ How to fetch/display
└─ Client URL command
```

**Options:**
```bash
curl http://localhost:3000         # GET request, print response
│     │
│     └─ URL to fetch
└─ curl

curl -I http://localhost:3000      # HEAD request (headers only)
│     │  │
│     │  └─ URL
│     └─ I = head request
└─ curl

curl -v http://localhost:3000      # Verbose (show request/response headers)
curl -s http://localhost:3000      # Silent (no progress bar)
curl -f http://localhost:3000      # Fail silently on HTTP errors

# POST requests
curl -X POST http://localhost:3100/loki/api/v1/push
│     │  │
│     │  └─ HTTP method
│     └─ X = specify method
└─ curl

curl -X POST -H "Content-Type: application/json" -d '{"key":"value"}' URL

# Save to file
curl -o output.txt http://example.com/file.txt
curl -O http://example.com/file.txt  # Use remote filename

# Practical examples
curl -I http://localhost:3000/api/health
curl -s http://localhost:3100/ready
curl http://localhost:38332 --user satoshi:password --data-binary '{"method":"getblockchaininfo"}'
curl -sfL https://get.docker.com | sh  # Download and execute script
```

---

### `ping` - Test Network Connectivity

**Template:**
```bash
ping [OPTIONS] DESTINATION
│     │         │
│     │         └─ Host to ping (IP or hostname)
│     └─ How to ping
└─ Packet Internet Groper
```

**Usage:**
```bash
ping google.com                    # Ping continuously (Ctrl+C to stop)
│     │
│     └─ Hostname or IP
└─ ping

ping -c 4 8.8.8.8                 # Send 4 packets only
│     │ │ │
│     │ │ └─ Destination IP
│     │ └─ Count
│     └─ c = count
└─ ping

ping -i 0.5 localhost             # Interval 0.5 seconds
ping -s 1000 google.com           # Packet size 1000 bytes
ping -W 1 10.0.0.1                # Timeout 1 second

# Practical examples
ping -c 3 localhost
ping -c 1 8.8.8.8 > /dev/null && echo "Internet OK"
```

---

### `ss` / `netstat` - Network Statistics

**Template:**
```bash
ss [OPTIONS]
│   │
│   └─ What to show
└─ Socket statistics (modern replacement for netstat)
```

**Usage:**
```bash
ss -tulpn                         # Most common: TCP/UDP, listening, process names
│  │││││
│  ││││└─ n = numeric (don't resolve names)
│  │││└─ p = process (show which program)
│  ││└─ l = listening sockets only
│  │└─ u = UDP
│  └─ t = TCP
└─ socket statistics

ss -tulpn | grep :3000            # Find what's on port 3000
ss -t state established           # Show established TCP connections
ss -s                             # Summary statistics

# netstat (older, but still common)
netstat -tulpn                    # Same as ss -tulpn
netstat -an | grep LISTEN
netstat -r                        # Routing table

# Practical examples
ss -tulpn | grep -E ':3000|:3100|:38332'
ss -t state established | wc -l   # Count active connections
```

---

## 📦 Package Management

### `apt` (Debian/Ubuntu)

**Template:**
```bash
apt [OPTIONS] COMMAND [PACKAGE]
│    │         │        │
│    │         │        └─ Package name
│    │         └─ Action (install, remove, update, etc.)
│    └─ How to perform action
└─ Advanced Package Tool
```

**Usage:**
```bash
# Update package lists
sudo apt update                    # Refresh package database
│     │   │
│     │   └─ Update package lists
│     └─ apt command
└─ sudo (run as admin)

# Upgrade packages
sudo apt upgrade                   # Upgrade installed packages
sudo apt full-upgrade              # Upgrade + handle dependencies

# Install packages
sudo apt install docker.io         # Install package
│     │   │       │
│     │   │       └─ Package name
│     │   └─ install command
│     └─ apt
└─ sudo

sudo apt install -y htop           # -y = assume yes (no prompts)
sudo apt install pkg1 pkg2 pkg3    # Multiple packages

# Remove packages
sudo apt remove docker.io          # Remove but keep config
sudo apt purge docker.io           # Remove including config
sudo apt autoremove                # Remove unused dependencies

# Search and info
apt search docker                  # Search package names/descriptions
apt show docker.io                 # Show package details
apt list --installed               # List installed packages
apt list --upgradable              # Show upgradable packages

# Practical examples
sudo apt update && sudo apt upgrade -y
sudo apt install -y htop ncdu jq curl wget
sudo apt remove --purge apache2
```

---

### `systemctl` - System Control (Services)

**Template:**
```bash
systemctl [OPTIONS] COMMAND SERVICE
│          │         │       │
│          │         │       └─ Service name
│          │         └─ Action (start, stop, status, etc.)
│          └─ Modifiers
└─ System control command
```

**Usage:**
```bash
# Service management
sudo systemctl start docker        # Start service
│     │         │     │
│     │         │     └─ Service name
│     │         └─ start command
│     └─ systemctl
└─ sudo

sudo systemctl stop docker         # Stop service
sudo systemctl restart docker      # Stop then start
sudo systemctl reload docker       # Reload config without stopping
sudo systemctl try-restart docker  # Restart only if running

# Enable/disable on boot
sudo systemctl enable docker       # Start on boot
sudo systemctl disable docker      # Don't start on boot
sudo systemctl enable --now docker # Enable and start immediately

# Status and info
systemctl status docker            # Service status (no sudo needed)
systemctl is-active docker         # Just show if active
systemctl is-enabled docker        # Show if enabled on boot
systemctl is-failed docker         # Check if failed

# System operations
sudo systemctl reboot              # Reboot system
sudo systemctl poweroff            # Shutdown system

# List services
systemctl list-units --type=service          # All services
systemctl list-units --type=service --state=running  # Running only
systemctl list-unit-files --type=service     # All service files

# Practical examples
sudo systemctl status docker
sudo systemctl restart docker && docker ps
systemctl list-units --type=service | grep running
```

---

## 🔐 Permissions & Ownership

### `chmod` - Change File Mode (Permissions)

**Template:**
```bash
chmod [OPTIONS] MODE FILE
│      │         │    │
│      │         │    └─ Target file/directory
│      │         └─ Permission mode (numeric or symbolic)
│      └─ How to apply
└─ Change mode command
```

**Numeric Mode:**
```bash
chmod 755 script.sh               # rwxr-xr-x
│      │   │
│      │   └─ File
│      └─ 7=rwx (owner), 5=r-x (group), 5=r-x (others)
└─ chmod

# Permission numbers:
# 0 = --- (no permissions)
# 1 = --x (execute only)
# 2 = -w- (write only)
# 3 = -wx (write + execute)
# 4 = r-- (read only)
# 5 = r-x (read + execute)
# 6 = rw- (read + write)
# 7 = rwx (read + write + execute)

chmod 644 file.txt                # rw-r--r-- (common for files)
chmod 755 script.sh               # rwxr-xr-x (common for scripts)
chmod 700 private.key             # rwx------ (private files)
chmod 600 ~/.ssh/id_rsa           # rw------- (SSH private key)
```

**Symbolic Mode:**
```bash
chmod +x script.sh                # Add execute for all
│      │  │
│      │  └─ File
│      └─ +x = add execute permission
└─ chmod

chmod u+x file.sh                 # Add execute for user (owner)
chmod g+w file.txt                # Add write for group
chmod o-r file.txt                # Remove read for others
chmod a+r file.txt                # Add read for all (a=all)

# u=user (owner), g=group, o=others, a=all
chmod u=rwx,g=rx,o=rx file.sh     # Set specific permissions
chmod -R 755 directory/           # Recursive (all files in directory)

# Practical examples
chmod +x scripts/*.sh
chmod 644 configs/*.conf
chmod 700 ~/.ssh
chmod -R u+w logs/
```

---

### `chown` - Change Owner

**Template:**
```bash
chown [OPTIONS] OWNER[:GROUP] FILE
│      │         │     │       │
│      │         │     │       └─ Target file/directory
│      │         │     └─ Group (optional)
│      │         └─ New owner
│      └─ How to apply
└─ Change owner command
```

**Usage:**
```bash
sudo chown username file.txt       # Change owner only
│     │     │        │
│     │     │        └─ File
│     │     └─ New owner
│     └─ chown
└─ sudo (usually needed)

sudo chown username:groupname file.txt  # Change owner and group
sudo chown :groupname file.txt          # Change group only

# Recursive
sudo chown -R username:groupname directory/
│     │     │  │
│     │     │  └─ New owner:group
│     │     └─ R = recursive
│     └─ chown
└─ sudo

# Use current user
sudo chown $USER:$USER file.txt
sudo chown -R $(whoami):$(whoami) directory/

# Practical examples
sudo chown divyansh:divyansh configs/
sudo chown -R $USER:$USER ~/luganode2025
sudo chown root:root /etc/config.conf
```

---

## 🔄 Pipes & Redirection

### Redirection Operators

**Template:**
```bash
command > file                     # Redirect stdout to file (overwrite)
│        │ │
│        │ └─ Output file
│        └─ > = redirect (overwrite)
└─ command

command >> file                    # Redirect stdout to file (append)
command 2> file                    # Redirect stderr to file
command 2>&1                       # Redirect stderr to stdout
command &> file                    # Redirect both stdout and stderr
command < file                     # Use file as stdin
```

**Examples:**
```bash
# Save output
ls -la > file-list.txt            # Save directory listing
echo "Log entry" >> app.log       # Append to log
docker compose ps > status.txt    # Save container status

# Redirect errors
./script.sh 2> errors.log         # Save only errors
./script.sh > output.log 2>&1     # Save both output and errors

# Discard output
command > /dev/null               # Discard stdout
command 2> /dev/null              # Discard stderr
command &> /dev/null              # Discard everything

# Use file as input
sort < unsorted.txt > sorted.txt
mysql -u root -p < database.sql

# Here document
cat << EOF > file.txt
Line 1
Line 2
EOF
```

---

### Pipe Operator

**Template:**
```bash
command1 | command2 | command3
│         │ │         │
│         │ │         └─ Third command
│         │ └─ Second command (uses output of first)
│         └─ | = pipe operator
└─ First command
```

**Examples:**
```bash
# Basic pipes
ls -la | grep docker               # List files, filter for docker
ps aux | grep bitcoin | wc -l     # Count bitcoin processes
docker logs bitcoin | tail -100    # Last 100 log lines

# Multiple pipes
docker compose logs | grep error | sort | uniq -c | sort -rn
│                     │            │      │         │
│                     │            │      │         └─ Sort numerically reverse
│                     │            │      └─ Count unique lines
│                     │            └─ Sort lines
│                     └─ Filter for errors
└─ Get logs

# Common patterns
ps aux | awk '{print $11}' | sort | uniq  # Unique commands running
docker stats --no-stream | column -t      # Format container stats
cat file.log | grep -v "debug" | less     # Filter and page

# Practical examples
docker compose ps | awk '{print $1}' | tail -n +2  # Get container names
find . -name "*.log" | xargs grep "error"          # Search in all logs
ls -lh | sort -k5 -h | tail -10                    # 10 largest files
```

---

## 🎯 Quick Reference Table

| Command | Purpose | Common Options | Example |
|---------|---------|----------------|---------|
| `ls` | List files | `-lah` (long, all, human) | `ls -lah configs/` |
| `cd` | Change directory | none | `cd ~/project` |
| `pwd` | Print working dir | none | `pwd` |
| `cp` | Copy | `-r` (recursive), `-i` (interactive) | `cp -r src/ backup/` |
| `mv` | Move/rename | `-i` (interactive), `-v` (verbose) | `mv old.txt new.txt` |
| `rm` | Remove | `-rf` (recursive force) | `rm -i file.txt` |
| `find` | Search files | `-name`, `-type`, `-size` | `find . -name "*.log"` |
| `grep` | Search in files | `-r` (recursive), `-i` (ignore case) | `grep -r "error" .` |
| `cat` | Display file | `-n` (line numbers) | `cat file.txt` |
| `less` | Page through file | `+F` (follow) | `less +F log.txt` |
| `head` | First lines | `-n NUM` | `head -20 file.txt` |
| `tail` | Last lines | `-f` (follow), `-n NUM` | `tail -f /var/log/syslog` |
| `ps` | Process list | `aux` (all, user format, X) | `ps aux \| grep docker` |
| `top` | Process monitor | Interactive | `top` |
| `kill` | Kill process | `-9` (force), `-15` (term) | `kill -9 1234` |
| `df` | Disk free | `-h` (human-readable) | `df -h` |
| `du` | Disk usage | `-sh` (summary, human) | `du -sh *` |
| `free` | Memory usage | `-h` (human-readable) | `free -h` |
| `curl` | Transfer data | `-I` (headers), `-s` (silent) | `curl -I localhost:3000` |
| `ping` | Test connectivity | `-c NUM` (count) | `ping -c 4 google.com` |
| `ss` | Socket stats | `-tulpn` (TCP, UDP, listening, process, numeric) | `ss -tulpn` |
| `chmod` | Change permissions | `+x` (add execute), `755`, `644` | `chmod +x script.sh` |
| `chown` | Change owner | `-R` (recursive) | `chown user:group file` |
| `systemctl` | Service control | `start`, `stop`, `status`, `enable` | `systemctl status docker` |

---

## 💡 Pro Tips for Interview

### Explain Your Thought Process
```bash
# Instead of just running:
docker ps

# Say out loud:
"I'm using 'docker ps' to check which containers are currently running.
This will show me the container ID, image, status, and ports."
```

### Use Tab Completion
```bash
# Type partial command and press Tab
cd lug[Tab]              # Completes to luganode2025/
docker compose ps bit[Tab]  # Completes to bitcoin
```

### Check Command Help
```bash
man ls                   # Manual page
ls --help                # Quick help
docker compose --help    # Docker compose help
```

### Chain Commands Safely
```bash
# Good: Only run second if first succeeds
cd ~/project && ls -la

# Check before dangerous operations
ls *.log && rm *.log    # List first, then delete
```

### Use History
```bash
history                  # Show command history
!123                     # Repeat command 123 from history
!!                       # Repeat last command
sudo !!                  # Run last command with sudo
Ctrl+R                   # Search command history (interactive)
```

---

**Remember**: Linux commands follow patterns. Once you understand the template structure, you can predict how commands work even if you haven't used them before! 🚀