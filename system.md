# 15 Essential Tools Every Linux User Should Master

## 🎯 The Power User's Arsenal

These tools will transform you from a beginner to a confident Linux power user. Learn them in this order!

---

## 1. 🔍 `grep` - The Text Search Master
**What it does:** Search for patterns in files  
**Why learn it:** Essential for log analysis, debugging, and finding anything in text files

**Key Skills:**
```bash
grep "error" file.log                    # Basic search
grep -r "TODO" .                         # Recursive search in all files
grep -i "warning" logs/*.log             # Case-insensitive search
grep -E "error|warn" file.log            # Multiple patterns (regex)
docker logs bitcoin | grep "block"       # Pipe output to grep
```

**Your Interview Project Use:**
```bash
grep -r "port" configs/                  # Find port configurations
docker compose logs | grep -i "error"    # Debug container issues
```

---

## 2. 📝 `vim` / `nano` - Text Editors
**What it does:** Edit files in terminal  
**Why learn it:** You'll need to edit config files on remote servers

**Choose Your Path:**

### Nano (Easier for beginners)
```bash
nano file.txt                            # Open file
# Ctrl+O = Save
# Ctrl+X = Exit
# Ctrl+W = Search
```

### Vim (More powerful, steeper learning curve)
```bash
vim file.txt                             # Open file
# i = Insert mode
# Esc = Normal mode
# :w = Save
# :q = Quit
# :wq = Save and quit
# /search = Search
```

**Practice:** Edit your `docker-compose.yml` and config files

---

## 3. 🌐 `curl` - The HTTP Swiss Army Knife
**What it does:** Transfer data from/to servers  
**Why learn it:** Test APIs, download files, debug network issues

**Essential Commands:**
```bash
curl https://api.example.com             # GET request
curl -I https://google.com               # Show headers only
curl -o file.zip https://example.com/file.zip  # Download file
curl -X POST -d "data" https://api.com   # POST request
curl -H "Authorization: Bearer token" https://api.com  # Headers

# Test your Bitcoin RPC
curl --user satoshi:password --data-binary '{"jsonrpc":"1.0","method":"getblockcount"}' http://localhost:38332
```

---

## 4. 🔗 `ssh` - Secure Shell
**What it does:** Connect to remote servers securely  
**Why learn it:** Manage remote servers, essential for DevOps

**Key Commands:**
```bash
ssh user@hostname                        # Connect to server
ssh -p 2222 user@hostname                # Custom port
ssh -i key.pem user@hostname             # Use SSH key

# Copy files (scp)
scp file.txt user@remote:/path/          # Upload file
scp user@remote:/path/file.txt .         # Download file

# Port forwarding
ssh -L 8080:localhost:80 user@remote     # Forward port 80 to local 8080
```

**Practice:** Connect to a VPS, set up your Bitcoin node remotely

---

## 5. 🗜️ `tar` - Archive Master
**What it does:** Create and extract compressed archives  
**Why learn it:** Backup, distribute, and manage large file collections

**Essential Commands:**
```bash
tar -czf backup.tar.gz folder/           # Create compressed archive
│   ││  │                │
│   ││  │                └─ Folder to compress
│   ││  └─ Output file
│   │└─ f = file
│   └─ z = gzip compression
└─ c = create

tar -xzf backup.tar.gz                   # Extract archive
│   ││  │
│   ││  └─ Archive file
│   │└─ f = file
│   └─ z = gzip
└─ x = extract

tar -tzf backup.tar.gz                   # List contents (don't extract)

# Common patterns
tar -czf bitcoin-data-$(date +%Y%m%d).tar.gz bitcoin-data/
tar -xzf backup.tar.gz -C /destination/path/
```

---

## 6. 🔐 `chmod` / `chown` - Permissions Control
**What it does:** Change file permissions and ownership  
**Why learn it:** Secure your files, fix permission issues

**Understanding Permissions:**
```
-rwxr-xr--
 │││││││││
 ││││││││└─ Others: read
 │││││││└── Others: write (-)
 ││││││└─── Others: execute (-)
 │││││└──── Group: read
 ││││└───── Group: write (-)
 │││└────── Group: execute
 ││└─────── Owner: read
 │└──────── Owner: write
 └───────── Owner: execute
```

**Commands:**
```bash
chmod 755 script.sh                      # rwxr-xr-x
│      │   │
│      │   └─ File
│      └─ Permissions (7=rwx, 5=r-x)
└─ change mode

chmod +x script.sh                       # Add execute permission
chmod -R 644 folder/                     # Recursive

chown user:group file.txt                # Change owner
│      │    │     │
│      │    │     └─ File
│      │    └─ Group
│      └─ User
└─ change owner

# Your project use
chmod +x setup-fedora.sh
chown -R $USER:$USER bitcoin-data/
```

---

## 7. 🔍 `htop` / `btop` - System Monitor
**What it does:** Monitor CPU, memory, processes in real-time  
**Why learn it:** Debug performance issues, find resource hogs

**Installation:**
```bash
sudo dnf install htop btop               # Install both
```

**Usage:**
```bash
htop                                     # Interactive process viewer
# F6 = Sort by column
# F9 = Kill process
# F5 = Tree view
# / = Search

btop                                     # Modern alternative (prettier)
# Mouse support
# Better graphics
```

**What to Watch:**
- CPU usage spikes
- Memory consumption
- Swap usage (high = problem)
- Process tree (parent-child relationships)

---

## 8. 📊 `journalctl` - System Logs
**What it does:** View and query systemd logs  
**Why learn it:** Debug service issues, see system events

**Essential Commands:**
```bash
journalctl -u docker                     # Logs for docker service
│           │ │
│           │ └─ Service name
│           └─ u = unit
└─ journal control

journalctl -u docker -f                  # Follow (tail -f style)
journalctl -u docker --since "1 hour ago"
journalctl -u docker --since "2024-10-30 09:00:00"
journalctl -xe                           # Show recent errors
journalctl -p err                        # Only error messages
journalctl -b                            # Since last boot
journalctl --disk-usage                  # Check log size

# Your project use
journalctl -u docker --since today | grep bitcoin
```

---

## 9. 🌳 `tree` - Directory Visualizer
**What it does:** Display directory structure as a tree  
**Why learn it:** Understand project structure, document layouts

**Installation & Usage:**
```bash
sudo dnf install tree

tree                                     # Show entire tree
tree -L 2                                # Max depth 2 levels
│     │ │
│     │ └─ Depth level
│     └─ L = level
└─ tree

tree -d                                  # Directories only
tree -a                                  # Include hidden files
tree -h                                  # Human-readable sizes
tree -I "node_modules|.git"              # Ignore patterns

# Your project structure
tree -L 2 ~/luganode2025/
```

---

## 10. 🔄 `rsync` - Smart File Sync
**What it does:** Efficiently sync/backup files  
**Why learn it:** Better than `cp` for large transfers, only copies changes

**Commands:**
```bash
rsync -avz source/ destination/          # Basic sync
│      ││  │        │
│      ││  │        └─ Destination
│      ││  └─ Source (trailing / matters!)
│      │└─ v = verbose
│      └─ a = archive (preserve permissions)
└─ z = compression

rsync -avz --delete source/ destination/ # Delete extra files in dest
rsync -avz --progress source/ dest/      # Show progress
rsync -avzP source/ user@remote:/path/   # Sync to remote server

# Backup your project
rsync -avz --progress ~/luganode2025/ /backup/luganode2025/

# Exclude files
rsync -avz --exclude="*.log" --exclude="node_modules" source/ dest/
```

---

## 11. 🔎 `jq` - JSON Processor
**What it does:** Parse and manipulate JSON data  
**Why learn it:** Essential for working with APIs, config files

**Installation:**
```bash
sudo dnf install jq
```

**Essential Commands:**
```bash
# Pretty print JSON
curl https://api.github.com/users/bitcoin | jq

# Extract specific field
echo '{"name":"Bitcoin","price":50000}' | jq '.price'
│                                          │  │
│                                          │  └─ Field to extract
│                                          └─ jq
└─ JSON input

# Array access
jq '.[0].name' file.json                 # First element's name
jq '.[] | select(.price > 1000)' file.json  # Filter

# Your project use
docker inspect bitcoin-signet | jq '.[0].State'
curl --user satoshi:password --data-binary '{"jsonrpc":"1.0","method":"getblockchaininfo"}' http://localhost:38332 | jq '.result.blocks'
```

---

## 12. 🌐 `netstat` / `ss` - Network Connections
**What it does:** Show network connections and listening ports  
**Why learn it:** Debug connectivity issues, find port conflicts

**Commands:**
```bash
# Show listening ports
ss -tuln                                 # Modern alternative to netstat
│  ││││
│  │││└─ n = numeric (don't resolve names)
│  ││└── l = listening
│  │└─── u = UDP
│  └──── t = TCP
└─ socket statistics

ss -tuln | grep 3000                     # Is Grafana port open?
ss -tulnp                                # Show process using port (needs sudo)

# Alternative: netstat (older)
sudo dnf install net-tools               # Install if needed
netstat -tuln
netstat -tulnp | grep docker

# Your project ports
ss -tuln | grep -E "3000|3100|38332"     # Grafana, Loki, Bitcoin
```

---

## 13. 🔧 `systemctl` - Service Management
**What it does:** Control systemd services  
**Why learn it:** Manage services, enable auto-start, check status

**Essential Commands:**
```bash
systemctl status docker                  # Check service status
│          │      │
│          │      └─ Service name
│          └─ status command
└─ system control

systemctl start docker                   # Start service
systemctl stop docker                    # Stop service
systemctl restart docker                 # Restart service
systemctl enable docker                  # Auto-start on boot
systemctl disable docker                 # Disable auto-start

systemctl list-units --type=service      # List all services
systemctl is-active docker               # Check if running
systemctl is-enabled docker              # Check if auto-starts

# View logs (integrates with journalctl)
systemctl status docker -l --no-pager
```

---

## 14. 🗄️ `ncdu` - Disk Usage Analyzer
**What it does:** Interactive disk usage viewer  
**Why learn it:** Find what's eating your disk space

**Installation & Usage:**
```bash
sudo dnf install ncdu

ncdu /                                   # Scan root
ncdu ~/luganode2025                      # Scan project

# Interactive controls:
# ↑↓ = Navigate
# Enter = Enter directory
# d = Delete (be careful!)
# g = Show graph
# c = Show items
# q = Quit
```

**Find Space Hogs:**
```bash
ncdu /var/lib/docker                     # Check Docker storage
ncdu ~/.cache                            # Check cache
```

---

## 15. 🐚 `tmux` / `screen` - Terminal Multiplexer
**What it does:** Multiple terminal sessions, survive disconnections  
**Why learn it:** Essential for remote work, long-running processes

**Why You Need This:**
- Run processes that survive SSH disconnection
- Multiple terminal panes in one window
- Share terminal sessions

**tmux Basics:**
```bash
sudo dnf install tmux

tmux                                     # Start new session
tmux new -s bitcoin                      # Named session
│     │   │ │
│     │   │ └─ Session name
│     │   └─ s = session
│     └─ new
└─ tmux

# Inside tmux (Ctrl+b = prefix):
# Ctrl+b c = Create new window
# Ctrl+b n = Next window
# Ctrl+b p = Previous window
# Ctrl+b % = Split vertically
# Ctrl+b " = Split horizontally
# Ctrl+b d = Detach (keeps running!)

# Outside tmux:
tmux ls                                  # List sessions
tmux attach -t bitcoin                   # Reattach to session
tmux kill-session -t bitcoin             # Kill session
```

**Real-world Use:**
```bash
# Start Bitcoin node in tmux (survives disconnect)
tmux new -s bitcoin
docker compose up bitcoin
# Ctrl+b d to detach

# Later, reconnect
tmux attach -t bitcoin
```

---

## 🎓 Learning Path

### Week 1: Basics
1. `grep` - Search your config files
2. `nano` - Edit configs comfortably
3. `tree` - Visualize your project

### Week 2: System Management
4. `htop` - Monitor your containers
5. `journalctl` - Debug Docker issues
6. `systemctl` - Manage services

### Week 3: File Operations
7. `tar` - Backup your project
8. `rsync` - Sync your files
9. `chmod`/`chown` - Fix permissions

### Week 4: Network & Advanced
10. `curl` - Test Bitcoin RPC
11. `ssh` - Connect to remote servers
12. `ss`/`netstat` - Debug ports
13. `jq` - Parse JSON responses
14. `ncdu` - Clean up disk space
15. `tmux` - Persistent sessions

---

## 🚀 Practice Project

**Build a complete workflow:**

```bash
# 1. Use tree to explore
tree -L 2 ~/luganode2025

# 2. Edit config with vim/nano
nano configs/bitcoin/bitcoin.conf

# 3. Search for errors
grep -r "error" configs/

# 4. Monitor system
htop

# 5. Check logs
journalctl -u docker -f

# 6. Test API with curl & jq
curl --user satoshi:password --data-binary '{"jsonrpc":"1.0","method":"getblockchaininfo"}' http://localhost:38332 | jq

# 7. Check ports
ss -tuln | grep 38332

# 8. Backup project
tar -czf luganode-backup-$(date +%Y%m%d).tar.gz ~/luganode2025

# 9. Sync to remote
rsync -avzP ~/luganode2025 user@remote:/backup/

# 10. Run in tmux
tmux new -s monitoring
docker compose logs -f
```

---

## 📚 Bonus Resources

**Man Pages (Built-in Documentation):**
```bash
man grep                                 # Full manual
grep --help                              # Quick reference
tldr grep                                # Install: sudo dnf install tldr
```

**Practice Sites:**
- [OverTheWire: Bandit](https://overthewire.org/wargames/bandit/) - Learn by doing
- [LinuxJourney](https://linuxjourney.com/) - Interactive tutorials

---

**Master these 15 tools and you'll be dangerous in any Linux environment!** 🔥🐧

Each tool solves real problems in your Bitcoin monitoring project. Practice them daily!