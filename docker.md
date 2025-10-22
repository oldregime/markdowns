# Docker Mastery Guide - From Zero to Hero

## 📦 What is Docker? (The Big Picture)

**Docker** is a platform that lets you package applications and their dependencies into **containers** - lightweight, portable, isolated environments that run consistently anywhere.

### Why Docker Matters for Your Interview
Your Bitcoin monitoring stack uses Docker extensively:
- `bitcoin-signet` container runs Bitcoin Core
- `grafana-alloy` container collects logs
- `loki` container stores logs
- `grafana` container visualizes data

Understanding Docker = Understanding your project's foundation!

---

## 🏗️ Docker Concepts (Before Commands)

### 1. Images vs Containers

```
Image (Blueprint)           Container (Running Instance)
     📄                            🏃
     │                             │
     ├─ Ubuntu OS                  ├─ Active process
     ├─ Bitcoin software           ├─ Using CPU/memory
     ├─ Config files               ├─ Has IP address
     └─ Dependencies               └─ Can be stopped/started

Analogy: Image = Recipe, Container = Cooked meal
```

### 2. Docker Architecture

```bash
Your Computer (Host)
│
├─ Docker Engine (Daemon)
│  │
│  ├─ Container 1 (bitcoin-signet)
│  │  └─ Isolated filesystem, network, processes
│  │
│  ├─ Container 2 (grafana-alloy)
│  │  └─ Isolated filesystem, network, processes
│  │
│  └─ Container 3 (loki)
│     └─ Isolated filesystem, network, processes
│
└─ Shared kernel (Linux)
```

### 3. Key Components

| Component | Purpose | Your Project Example |
|-----------|---------|---------------------|
| **Dockerfile** | Instructions to build image | (You use pre-built images) |
| **Image** | Template/snapshot of app | `bitcoind/signet:latest` |
| **Container** | Running instance of image | `bitcoin-signet` container |
| **Volume** | Persistent storage | `bitcoin-data`, `grafana-data` |
| **Network** | Container communication | `luganode25_default` |
| **Docker Compose** | Multi-container orchestration | Your `docker-compose.yml` |

---

## 🚀 Docker Installation (If Needed)

### Check if Docker is Installed
```bash
docker --version              # Check Docker version
│       │
│       └─ --version flag
└─ Docker command

docker version                # Detailed version info
docker info                   # System-wide information
```

**Expected Output:**
```
Docker version 24.0.6, build ed223bc
```

### Install Docker (Ubuntu/Debian)
```bash
# Official Docker installation script
curl -fsSL https://get.docker.com | sh
│    │││  │
│    │││  └─ URL to Docker install script
│    ││└─ L = follow redirects
│    │└─ S = show errors
│    └─ s = silent mode
└─ curl

# Add your user to docker group (avoid sudo)
sudo usermod -aG docker $USER
│     │       │  │      │
│     │       │  │      └─ Current user variable
│     │       │  └─ docker group
│     │       └─ a = append, G = supplementary group
│     └─ usermod (modify user)
└─ sudo

# Logout and login again for group to take effect
# Or use: newgrp docker
```

---

## 🐳 Docker Commands - The Essentials

### Template Structure
```bash
docker MANAGEMENT_COMMAND COMMAND [OPTIONS] [ARGUMENTS]
│       │                 │        │         │
│       │                 │        │         └─ What to act on
│       │                 │        └─ Flags/options
│       │                 └─ Action to perform
│       └─ Resource category (image, container, network, etc.)
└─ Docker CLI
```

---

## 📦 Working with Images

### `docker pull` - Download Images

**Template:**
```bash
docker pull [OPTIONS] IMAGE[:TAG]
│       │    │         │    │
│       │    │         │    └─ Version (default: latest)
│       │    │         └─ Image name
│       │    └─ Options (registry, etc.)
│       └─ pull command
└─ docker
```

**Usage:**
```bash
# Pull official images
docker pull ubuntu                    # Latest Ubuntu
docker pull ubuntu:22.04              # Specific version
│       │    │      │
│       │    │      └─ Tag (version)
│       │    └─ Image name
│       └─ pull
└─ docker

docker pull nginx:alpine              # Nginx with Alpine Linux
docker pull postgres:15               # PostgreSQL v15

# Pull from Docker Hub (default registry)
docker pull luganodes/bitcoin-core    # From luganodes account

# Your project's images (already pulled by compose)
docker pull grafana/grafana:latest
docker pull grafana/loki:latest
docker pull grafana/alloy:latest
```

### `docker images` - List Downloaded Images

**Template:**
```bash
docker images [OPTIONS]
│       │      │
│       │      └─ Display options
│       └─ List images command
└─ docker
```

**Usage:**
```bash
# List all images
docker images                         # All local images
│       │
│       └─ List images
└─ docker

# Alternative command
docker image ls                       # Same as docker images

# Filter images
docker images ubuntu                  # Only ubuntu images
docker images | grep grafana          # Images with "grafana"

# Show image IDs only
docker images -q
│       │      │
│       │      └─ q = quiet (IDs only)
│       └─ images
└─ docker

# Show image sizes
docker images --format "table {{.Repository}}\t{{.Tag}}\t{{.Size}}"
│       │      │
│       │      └─ Custom format string
│       └─ images
└─ docker
```

**Output Columns:**
```
REPOSITORY          TAG       IMAGE ID       CREATED        SIZE
grafana/grafana     latest    abc123def456   2 weeks ago    300MB
grafana/loki        latest    def456ghi789   3 weeks ago    80MB
grafana/alloy       latest    ghi789jkl012   1 month ago    200MB
```

### `docker rmi` - Remove Images

**Template:**
```bash
docker rmi [OPTIONS] IMAGE [IMAGE...]
│       │   │         │
│       │   │         └─ Image name or ID
│       │   └─ Options (force, etc.)
│       └─ Remove image command
└─ docker
```

**Usage:**
```bash
# Remove by name
docker rmi ubuntu:22.04               # Remove specific version
│       │   │
│       │   └─ Image name:tag
│       └─ rmi (remove image)
└─ docker

# Remove by ID
docker rmi abc123def456               # First few chars of ID

# Force remove (if in use)
docker rmi -f grafana/grafana
│       │   │  │
│       │   │  └─ Image to remove
│       │   └─ f = force
│       └─ rmi
└─ docker

# Remove multiple images
docker rmi image1 image2 image3

# Remove all unused images
docker image prune                    # Remove dangling images
docker image prune -a                 # Remove ALL unused images
│       │     │     │
│       │     │     └─ a = all (not just dangling)
│       │     └─ prune (clean up)
│       └─ image
└─ docker
```

---

## 🏃 Working with Containers

### `docker run` - Create and Start Container

**Template:**
```bash
docker run [OPTIONS] IMAGE [COMMAND] [ARGS]
│       │   │         │      │        │
│       │   │         │      │        └─ Command arguments
│       │   │         │      └─ Command to run inside container
│       │   │         └─ Image to use
│       │   └─ Options (ports, volumes, name, etc.)
│       └─ run command
└─ docker
```

**Common Options:**
```bash
# Basic run (container stops after command finishes)
docker run ubuntu echo "Hello Docker"
│       │   │     │    │
│       │   │     │    └─ Argument to echo
│       │   │     └─ Command to run
│       │   └─ Image name
│       └─ run
└─ docker

# Run interactively with terminal
docker run -it ubuntu bash
│       │   ││ │     │
│       │   ││ │     └─ Command (bash shell)
│       │   ││ └─ Image
│       │   │└─ t = allocate pseudo-TTY
│       │   └─ i = interactive (keep STDIN open)
│       └─ run
└─ docker

# Run in background (detached)
docker run -d nginx
│       │   │ │
│       │   │ └─ Image
│       │   └─ d = detached mode
│       └─ run
└─ docker

# Run with name
docker run -d --name my-nginx nginx
│       │   │  │     │        │
│       │   │  │     │        └─ Image
│       │   │  │     └─ Container name
│       │   │  └─ --name flag
│       │   └─ d = detached
│       └─ run
└─ docker

# Run with port mapping
docker run -d -p 8080:80 nginx
│       │   │  │ │    │  │
│       │   │  │ │    │  └─ Image
│       │   │  │ │    └─ Container port 80
│       │   │  │ └─ Host port 8080
│       │   │  └─ p = publish port
│       │   └─ d = detached
│       └─ run
└─ docker
# Access: http://localhost:8080

# Run with volume (persistent storage)
docker run -d -v my-data:/app/data nginx
│       │   │  │ │       │         │
│       │   │  │ │       │         └─ Image
│       │   │  │ │       └─ Container path
│       │   │  │ └─ Host path or volume name
│       │   │  └─ v = volume
│       │   └─ d = detached
│       └─ run
└─ docker

# Run with environment variables
docker run -d -e DATABASE_URL=postgres://... nginx
│       │   │  │ │
│       │   │  │ └─ Variable and value
│       │   │  └─ e = environment variable
│       │   └─ d = detached
│       └─ run
└─ docker

# Run with restart policy
docker run -d --restart=always nginx
│       │   │  │        │      │
│       │   │  │        │      └─ Image
│       │   │  │        └─ Policy (always, unless-stopped, on-failure)
│       │   │  └─ --restart flag
│       │   └─ d = detached
│       └─ run
└─ docker

# Combine multiple options (like your Bitcoin container)
docker run -d \
  --name bitcoin-signet \
  -p 38332:38332 \
  -v bitcoin-data:/root/.bitcoin \
  -e BITCOIN_NETWORK=signet \
  --restart=unless-stopped \
  bitcoin/bitcoin:latest
```

**Real Example from Your Project:**
```bash
# What docker-compose does behind the scenes for Bitcoin service
docker run -d \
  --name bitcoin-signet \                          # Container name
  -p 38332:38332 \                                 # RPC port
  -p 38333:38333 \                                 # P2P port
  -v bitcoin-data:/root/.bitcoin \                 # Persistent storage
  --health-cmd="bitcoin-cli -signet ping" \        # Health check
  --health-interval=30s \
  --health-timeout=10s \
  --health-retries=3 \
  bitcoindevproject/bitcoin:latest \               # Image
  bitcoind -signet -rest -rpcuser=satoshi ...      # Command
```

### `docker ps` - List Running Containers

**Template:**
```bash
docker ps [OPTIONS]
│       │  │
│       │  └─ Display options
│       └─ List containers (ps = process status)
└─ docker
```

**Usage:**
```bash
# List running containers
docker ps                             # Running only
│       │
│       └─ List processes
└─ docker

# List all containers (including stopped)
docker ps -a
│       │  │
│       │  └─ a = all (including stopped)
│       └─ ps
└─ docker

# List with custom format
docker ps --format "table {{.Names}}\t{{.Status}}\t{{.Ports}}"
│       │  │
│       │  └─ Format string
│       └─ ps
└─ docker

# List only container IDs
docker ps -q                          # Quiet mode (IDs only)
│       │  │
│       │  └─ q = quiet
│       └─ ps
└─ docker

# List containers by name
docker ps | grep bitcoin
docker ps --filter "name=bitcoin"
│       │  │       │
│       │  │       └─ Filter expression
│       │  └─ --filter flag
│       └─ ps
└─ docker

# Show container sizes
docker ps -s
│       │  │
│       │  └─ s = size
│       └─ ps
└─ docker
```

**Output Columns:**
```
CONTAINER ID   IMAGE              COMMAND       CREATED        STATUS         PORTS                    NAMES
abc123def456   grafana/grafana    "/run.sh"     2 hours ago    Up 2 hours     0.0.0.0:3000->3000/tcp   grafana
def456ghi789   grafana/loki       "/loki"       2 hours ago    Up 2 hours     3100/tcp                 loki
ghi789jkl012   bitcoin/bitcoin    "bitcoind"    2 hours ago    Up 2 hours     38332/tcp                bitcoin-signet
```

### `docker stop` - Stop Running Container

**Template:**
```bash
docker stop [OPTIONS] CONTAINER [CONTAINER...]
│       │    │         │
│       │    │         └─ Container name or ID
│       │    └─ Options (timeout, etc.)
│       └─ stop command
└─ docker
```

**Usage:**
```bash
# Stop by name
docker stop bitcoin-signet            # Graceful shutdown (SIGTERM)
│       │    │
│       │    └─ Container name
│       └─ stop
└─ docker

# Stop by ID
docker stop abc123def456              # First few chars

# Stop multiple containers
docker stop bitcoin-signet loki grafana

# Stop with timeout
docker stop -t 30 bitcoin-signet
│       │    │  │  │
│       │    │  │  └─ Container name
│       │    │  └─ Timeout in seconds (default: 10)
│       │    └─ t = time
│       └─ stop
└─ docker

# Stop all running containers
docker stop $(docker ps -q)
│       │    │       │   │
│       │    │       │   └─ q = quiet (IDs only)
│       │    │       └─ ps (list containers)
│       │    └─ Command substitution
│       └─ stop
└─ docker
```

### `docker start` - Start Stopped Container

**Template:**
```bash
docker start [OPTIONS] CONTAINER [CONTAINER...]
│       │     │         │
│       │     │         └─ Container name or ID
│       │     └─ Options (attach, interactive)
│       └─ start command
└─ docker
```

**Usage:**
```bash
# Start stopped container
docker start bitcoin-signet
│       │     │
│       │     └─ Container name
│       └─ start
└─ docker

# Start and attach to output
docker start -a bitcoin-signet
│       │     │ │
│       │     │ └─ Container name
│       │     └─ a = attach (show output)
│       └─ start
└─ docker

# Start multiple containers
docker start bitcoin-signet alloy loki
```

### `docker restart` - Restart Container

**Template:**
```bash
docker restart [OPTIONS] CONTAINER [CONTAINER...]
│       │       │         │
│       │       │         └─ Container name or ID
│       │       └─ Options (timeout)
│       └─ restart command
└─ docker
```

**Usage:**
```bash
# Restart container (stop then start)
docker restart bitcoin-signet
│       │       │
│       │       └─ Container name
│       └─ restart
└─ docker

# Restart with timeout
docker restart -t 30 bitcoin-signet
│       │       │  │  │
│       │       │  │  └─ Container
│       │       │  └─ Timeout seconds
│       │       └─ t = time
│       └─ restart
└─ docker

# Restart all containers
docker restart $(docker ps -q)
```

### `docker rm` - Remove Container

**Template:**
```bash
docker rm [OPTIONS] CONTAINER [CONTAINER...]
│       │  │         │
│       │  │         └─ Container name or ID
│       │  └─ Options (force, volumes)
│       └─ remove command
└─ docker
```

**Usage:**
```bash
# Remove stopped container
docker rm bitcoin-signet
│       │  │
│       │  └─ Container name
│       └─ rm (remove)
└─ docker

# Force remove running container
docker rm -f bitcoin-signet
│       │  │ │
│       │  │ └─ Container name
│       │  └─ f = force
│       └─ rm
└─ docker

# Remove with volumes
docker rm -v bitcoin-signet
│       │  │ │
│       │  │ └─ Container name
│       │  └─ v = remove associated volumes
│       └─ rm
└─ docker

# Remove all stopped containers
docker container prune                # Interactive
docker container prune -f             # Force (no prompt)
│       │         │     │
│       │         │     └─ f = force
│       │         └─ prune (clean up)
│       └─ container
└─ docker

# Remove all containers (dangerous!)
docker rm -f $(docker ps -aq)
│       │  │  │       │   ││
│       │  │  │       │   │└─ q = quiet (IDs only)
│       │  │  │       │   └─ a = all
│       │  │  │       └─ ps
│       │  │  └─ Command substitution
│       │  └─ f = force
│       └─ rm
└─ docker
```

---

## 🔍 Inspecting Containers

### `docker logs` - View Container Logs

**Template:**
```bash
docker logs [OPTIONS] CONTAINER
│       │    │         │
│       │    │         └─ Container name or ID
│       │    └─ Options (follow, tail, timestamps)
│       └─ logs command
└─ docker
```

**Usage:**
```bash
# View all logs
docker logs bitcoin-signet
│       │    │
│       │    └─ Container name
│       └─ logs
└─ docker

# Follow logs (like tail -f)
docker logs -f bitcoin-signet
│       │    │ │
│       │    │ └─ Container name
│       │    └─ f = follow
│       └─ logs
└─ docker

# Show last N lines
docker logs --tail 100 bitcoin-signet
│       │    │     │   │
│       │    │     │   └─ Container name
│       │    │     └─ Number of lines
│       │    └─ --tail flag
│       └─ logs
└─ docker

# Show logs since time
docker logs --since 30m bitcoin-signet
│       │    │      │   │
│       │    │      │   └─ Container
│       │    │      └─ Time (30m, 1h, 2024-10-22T09:00:00)
│       │    └─ --since flag
│       └─ logs
└─ docker

# Show logs until time
docker logs --until 2024-10-22T12:00:00 bitcoin-signet

# Show timestamps
docker logs -t bitcoin-signet
│       │    │ │
│       │    │ └─ Container
│       │    └─ t = timestamps
│       └─ logs
└─ docker

# Combine options
docker logs -f --tail 50 --since 10m bitcoin-signet
│       │    │  │     │   │      │   │
│       │    │  │     │   │      │   └─ Container
│       │    │  │     │   │      └─ Since 10 minutes ago
│       │    │  │     │   └─ Last 50 lines
│       │    │  │     └─ --tail
│       │    │  └─ Follow
│       │    └─ logs
│       └─ docker

# Filter logs
docker logs bitcoin-signet | grep error
docker logs bitcoin-signet 2>&1 | grep -i warning
```

### `docker exec` - Execute Command in Running Container

**Template:**
```bash
docker exec [OPTIONS] CONTAINER COMMAND [ARGS]
│       │    │         │         │       │
│       │    │         │         │       └─ Command arguments
│       │    │         │         └─ Command to run
│       │    │         └─ Container name or ID
│       │    └─ Options (interactive, user, workdir)
│       └─ exec command
└─ docker
```

**Usage:**
```bash
# Execute single command
docker exec bitcoin-signet ls -la /root/.bitcoin
│       │    │              │    │
│       │    │              │    └─ Command arguments
│       │    │              └─ Command
│       │    └─ Container name
│       └─ exec
└─ docker

# Interactive shell
docker exec -it bitcoin-signet bash
│       │    ││ │              │
│       │    ││ │              └─ Shell command
│       │    ││ └─ Container
│       │    │└─ t = TTY
│       │    └─ i = interactive
│       └─ exec
└─ docker

# Alpine containers use sh instead of bash
docker exec -it grafana-alloy sh

# Execute as specific user
docker exec -u root bitcoin-signet whoami
│       │    │ │    │              │
│       │    │ │    │              └─ Command
│       │    │ │    └─ Container
│       │    │ └─ User to run as
│       │    └─ u = user
│       └─ exec
└─ docker

# Execute in specific directory
docker exec -w /app bitcoin-signet pwd
│       │    │ │    │              │
│       │    │ │    │              └─ Command
│       │    │ │    └─ Container
│       │    │ └─ Working directory
│       │    └─ w = workdir
│       └─ exec
└─ docker

# Your project examples (Bitcoin RPC commands)
docker exec bitcoin-signet bitcoin-cli -signet getblockchaininfo
docker exec bitcoin-signet bitcoin-cli -signet getpeerinfo
docker exec bitcoin-signet bitcoin-cli -signet getnetworkinfo
docker exec bitcoin-signet bitcoin-cli -signet help

# Check Loki logs inside container
docker exec loki cat /var/log/loki/loki.log

# Verify Grafana config
docker exec grafana cat /etc/grafana/grafana.ini
```

### `docker inspect` - Detailed Container/Image Information

**Template:**
```bash
docker inspect [OPTIONS] CONTAINER|IMAGE [CONTAINER|IMAGE...]
│       │       │         │
│       │       │         └─ Container or image name/ID
│       │       └─ Options (format, type)
│       └─ inspect command
└─ docker
```

**Usage:**
```bash
# Full JSON output
docker inspect bitcoin-signet
│       │       │
│       │       └─ Container name
│       └─ inspect
└─ docker

# Extract specific field with jq
docker inspect bitcoin-signet | jq '.[0].State'
│       │       │              │ │
│       │       │              │ └─ JSON path
│       │       │              └─ jq (JSON processor)
│       │       └─ Container
│       └─ inspect
└─ docker

# Get IP address
docker inspect bitcoin-signet | jq '.[0].NetworkSettings.IPAddress'
docker inspect -f '{{.NetworkSettings.IPAddress}}' bitcoin-signet
│       │       │ │                                  │
│       │       │ │                                  └─ Container
│       │       │ └─ Go template format
│       │       └─ f = format
│       └─ inspect
└─ docker

# Get container status
docker inspect -f '{{.State.Status}}' bitcoin-signet

# Get environment variables
docker inspect -f '{{.Config.Env}}' bitcoin-signet

# Get mounted volumes
docker inspect -f '{{.Mounts}}' bitcoin-signet

# Get port mappings
docker inspect -f '{{.NetworkSettings.Ports}}' bitcoin-signet

# Check health status
docker inspect -f '{{.State.Health.Status}}' bitcoin-signet

# Practical examples
docker inspect bitcoin-signet | jq '.[0].Config.Cmd'        # Command
docker inspect bitcoin-signet | jq '.[0].HostConfig.RestartPolicy'  # Restart policy
docker inspect bitcoin-signet | jq '.[0].Mounts'             # Volumes
docker inspect bitcoin-signet | jq '.[0].NetworkSettings.Networks'  # Networks
```

### `docker stats` - Container Resource Usage

**Template:**
```bash
docker stats [OPTIONS] [CONTAINER...]
│       │     │         │
│       │     │         └─ Specific containers (empty = all)
│       │     └─ Options (no-stream, format)
│       └─ stats command
└─ docker
```

**Usage:**
```bash
# Live stats (all running containers)
docker stats
│       │
│       └─ stats (live updating)
└─ docker
# Press Ctrl+C to exit

# One-shot stats (no live updates)
docker stats --no-stream
│       │     │
│       │     └─ --no-stream (single snapshot)
│       └─ stats
└─ docker

# Specific container
docker stats bitcoin-signet
│       │     │
│       │     └─ Container name
│       └─ stats
└─ docker

# Multiple containers
docker stats bitcoin-signet loki grafana

# Custom format
docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}"
│       │     │
│       │     └─ Format string
│       └─ stats
└─ docker

# No stream with format
docker stats --no-stream --format "table {{.Name}}\t{{.CPUPerc}}\t{{.MemPerc}}\t{{.NetIO}}"
```

**Output Columns:**
```
CONTAINER ID   NAME            CPU %     MEM USAGE / LIMIT     MEM %     NET I/O           BLOCK I/O
abc123def456   bitcoin-signet  25.50%    450MiB / 7.78GiB     5.64%     1.2MB / 850kB     150MB / 2.1GB
def456ghi789   grafana         2.15%     120MiB / 7.78GiB     1.51%     500kB / 300kB     10MB / 50MB
ghi789jkl012   loki            1.80%     80MiB / 7.78GiB      1.00%     300kB / 200kB     5MB / 20MB
```

---

## 💾 Working with Volumes

### Understanding Volumes

```
Container (Temporary)       Volume (Persistent)
      🗑️                          💾
      │                           │
      ├─ Deleted on rm            ├─ Survives container removal
      └─ Lost on restart          └─ Shared between containers
```

### `docker volume` Commands

**Template:**
```bash
docker volume COMMAND [OPTIONS] [VOLUME]
│       │      │        │         │
│       │      │        │         └─ Volume name
│       │      │        └─ Command options
│       │      └─ Action (create, ls, rm, inspect)
│       └─ volume management
└─ docker
```

**Usage:**
```bash
# List volumes
docker volume ls
│       │      │
│       │      └─ ls (list)
│       └─ volume
└─ docker

# Create volume
docker volume create my-volume
│       │      │      │
│       │      │      └─ Volume name
│       │      └─ create
│       └─ volume
└─ docker

# Inspect volume
docker volume inspect bitcoin-data
│       │      │       │
│       │      │       └─ Volume name
│       │      └─ inspect
│       └─ volume
└─ docker

# Get volume mountpoint (actual location on host)
docker volume inspect bitcoin-data | jq '.[0].Mountpoint'
# Output: /var/lib/docker/volumes/luganode25_bitcoin-data/_data

# Remove volume
docker volume rm my-volume
│       │      │  │
│       │      │  └─ Volume name
│       │      └─ rm (remove)
│       └─ volume
└─ docker

# Remove unused volumes
docker volume prune                   # Interactive
docker volume prune -f                # Force
│       │      │     │
│       │      │     └─ f = force
│       │      └─ prune
│       └─ volume
└─ docker

# Your project volumes
docker volume ls
# DRIVER    VOLUME NAME
# local     luganode25_bitcoin-data
# local     luganode25_loki-data
# local     luganode25_grafana-data
# local     luganode25_alloy-data
```

### Using Volumes with Containers

```bash
# Named volume
docker run -d -v bitcoin-data:/root/.bitcoin bitcoin/bitcoin
│       │   │  │ │           │                 │
│       │   │  │ │           │                 └─ Image
│       │   │  │ │           └─ Container path
│       │   │  │ └─ Volume name
│       │   │  └─ v = volume
│       │   └─ d = detached
│       └─ run
└─ docker

# Bind mount (host directory)
docker run -d -v /host/path:/container/path nginx
│       │   │  │ │          │               │
│       │   │  │ │          │               └─ Image
│       │   │  │ │          └─ Container path
│       │   │  │ └─ Host absolute path
│       │   │  └─ v = volume/mount
│       │   └─ d = detached
│       └─ run
└─ docker

# Read-only volume
docker run -d -v my-data:/data:ro nginx
│       │   │  │ │       │     │  │
│       │   │  │ │       │     │  └─ Image
│       │   │  │ │       │     └─ ro = read-only
│       │   │  │ │       └─ Container path
│       │   │  │ └─ Volume name
│       │   │  └─ v = volume
│       │   └─ d = detached
│       └─ run
└─ docker

# Your project example
docker run -d \
  --name bitcoin-signet \
  -v luganode25_bitcoin-data:/root/.bitcoin \  # Named volume
  bitcoin/bitcoin:latest
```

---

## 🌐 Working with Networks

### Understanding Docker Networks

```
Docker Networks (Isolation)
│
├─ bridge (default)        # Containers on same host
├─ host                    # Use host's network directly
├─ none                    # No networking
└─ custom                  # User-defined bridge
   │
   └─ luganode25_default   # Your project's network
```

### `docker network` Commands

**Template:**
```bash
docker network COMMAND [OPTIONS] [NETWORK]
│       │       │        │         │
│       │       │        │         └─ Network name
│       │       │        └─ Command options
│       │       └─ Action (create, ls, rm, inspect)
│       └─ network management
└─ docker
```

**Usage:**
```bash
# List networks
docker network ls
│       │       │
│       │       └─ ls (list)
│       └─ network
└─ docker

# Inspect network
docker network inspect luganode25_default
│       │       │       │
│       │       │       └─ Network name
│       │       └─ inspect
│       └─ network
└─ docker

# See which containers are on network
docker network inspect luganode25_default | jq '.[0].Containers'

# Create network
docker network create my-network
│       │       │      │
│       │       │      └─ Network name
│       │       └─ create
│       └─ network
└─ docker

# Create with specific subnet
docker network create --subnet=172.18.0.0/16 my-network

# Remove network
docker network rm my-network
│       │       │  │
│       │       │  └─ Network name
│       │       └─ rm
│       └─ network
└─ docker

# Connect container to network
docker network connect my-network bitcoin-signet
│       │       │       │          │
│       │       │       │          └─ Container name
│       │       │       └─ Network name
│       │       └─ connect
│       └─ network
└─ docker

# Disconnect container from network
docker network disconnect my-network bitcoin-signet

# Your project's network
docker network inspect luganode25_default
# Shows: bitcoin-signet, grafana-alloy, loki, grafana all connected
```

### Container Communication on Networks

```bash
# Containers on same network can reach each other by name
# In your project:

# From grafana-alloy container:
docker exec grafana-alloy ping -c 2 loki
# Works! Because both on luganode25_default network

# From bitcoin-signet container:
docker exec bitcoin-signet curl http://loki:3100/ready
# Works! DNS resolution by container name

# Get container IP on network
docker inspect -f '{{range .NetworkSettings.Networks}}{{.IPAddress}}{{end}}' bitcoin-signet
```

---

## 🎼 Docker Compose - Multi-Container Orchestration

### What is Docker Compose?

**Docker Compose** manages multiple containers as a single application using a YAML configuration file.

```
Without Compose (Manual):                With Compose (Automated):
docker run bitcoin...                    docker compose up
docker run alloy...            vs.       (Starts all services)
docker run loki...
docker run grafana...
```

### `docker compose` Commands

**Template:**
```bash
docker compose [OPTIONS] COMMAND [ARGS]
│       │       │         │       │
│       │       │         │       └─ Command arguments
│       │       │         └─ Action (up, down, ps, logs, etc.)
│       │       └─ Global options (-f, -p)
│       └─ compose subcommand
└─ docker
```

### Essential Compose Commands

```bash
# Start all services (create and start)
docker compose up
│       │       │
│       │       └─ up (create + start)
│       └─ compose
└─ docker

# Start in background (detached)
docker compose up -d
│       │       │  │
│       │       │  └─ d = detached
│       │       └─ up
│       └─ compose
└─ docker

# Start specific service
docker compose up -d bitcoin
│       │       │  │ │
│       │       │  │ └─ Service name from compose file
│       │       │  └─ d = detached
│       │       └─ up
│       └─ compose
└─ docker

# Build images before starting
docker compose up --build
│       │       │  │
│       │       │  └─ --build flag
│       │       └─ up
│       └─ compose
└─ docker

# Force recreate containers
docker compose up -d --force-recreate
│       │       │  │  │
│       │       │  │  └─ Recreate even if config unchanged
│       │       │  └─ d = detached
│       │       └─ up
│       └─ compose
└─ docker

# Stop and remove containers, networks
docker compose down
│       │       │
│       │       └─ down (stop + remove)
│       └─ compose
└─ docker

# Stop and remove with volumes (CAREFUL: deletes data!)
docker compose down -v
│       │       │    │
│       │       │    └─ v = volumes
│       │       └─ down
│       └─ compose
└─ docker

# List services
docker compose ps
│       │       │
│       │       └─ ps (list services)
│       └─ compose
└─ docker

# View logs
docker compose logs                    # All services
docker compose logs bitcoin            # Specific service
docker compose logs -f                 # Follow
docker compose logs -f --tail=100      # Follow last 100 lines
docker compose logs --since 10m        # Last 10 minutes

# Execute command in service
docker compose exec bitcoin bitcoin-cli -signet getblockchaininfo
│       │       │    │       │
│       │       │    │       └─ Command and args
│       │       │    └─ Service name
│       │       └─ exec
│       └─ compose
└─ docker

# Stop services (don't remove)
docker compose stop                    # All services
docker compose stop bitcoin            # Specific service

# Start stopped services
docker compose start                   # All services
docker compose start bitcoin           # Specific service

# Restart services
docker compose restart                 # All services
docker compose restart bitcoin         # Specific service

# View service configuration
docker compose config
│       │       │
│       │       └─ config (show resolved config)
│       └─ compose
└─ docker

# List images used by services
docker compose images

# Pull latest images
docker compose pull

# Build service images
docker compose build bitcoin

# Remove stopped service containers
docker compose rm                      # Interactive
docker compose rm -f                   # Force
```

---

## 📄 Understanding docker-compose.yml

### Your Project's Compose File Structure

```yaml
services:                              # Define services (containers)
  bitcoin:                             # Service name (for internal DNS)
    image: bitcoindevproject/bitcoin:latest   # Image to use
    container_name: bitcoin-signet     # Actual container name
    ports:                             # Port mapping host:container
      - "38332:38332"                  # RPC port
      - "38333:38333"                  # P2P port
    volumes:                           # Persistent storage
      - bitcoin-data:/root/.bitcoin    # Named volume
      - ./configs/bitcoin:/etc/bitcoin:ro  # Bind mount (read-only)
    environment:                       # Environment variables
      - BITCOIN_NETWORK=signet
    command: >                         # Command to run
      bitcoind -signet -rest
      -rpcuser=satoshi
      -rpcpassword=nakamoto
    healthcheck:                       # Container health monitoring
      test: ["CMD", "bitcoin-cli", "-signet", "ping"]
      interval: 30s
      timeout: 10s
      retries: 3
      start_period: 60s
    restart: unless-stopped            # Restart policy
    networks:                          # Networks to connect
      - monitoring

  alloy:
    image: grafana/alloy:latest
    container_name: grafana-alloy
    depends_on:                        # Start order dependency
      bitcoin:
        condition: service_healthy     # Wait for bitcoin to be healthy
      loki:
        condition: service_started
    volumes:
      - ./configs/alloy:/etc/alloy:ro
      - /var/run/docker.sock:/var/run/docker.sock:ro  # Docker socket access
    ports:
      - "12345:12345"
    restart: unless-stopped
    user: root                         # Run as root (for docker socket)
    networks:
      - monitoring

  loki:
    image: grafana/loki:latest
    container_name: loki
    ports:
      - "3100:3100"
    volumes:
      - ./configs/loki:/etc/loki:ro
      - loki-data:/loki
    command: -config.file=/etc/loki/loki-config.yaml
    healthcheck:
      test: ["CMD", "wget", "--spider", "http://localhost:3100/ready"]
      interval: 30s
      timeout: 10s
      retries: 3
    restart: unless-stopped
    networks:
      - monitoring

  grafana:
    image: grafana/grafana:latest
    container_name: grafana
    depends_on:
      loki:
        condition: service_healthy
    ports:
      - "3000:3000"
    volumes:
      - grafana-data:/var/lib/grafana
      - ./configs/grafana:/etc/grafana/provisioning:ro
    environment:
      - GF_SECURITY_ADMIN_PASSWORD=luganodes2024
      - GF_USERS_ALLOW_SIGN_UP=false
    restart: unless-stopped
    networks:
      - monitoring

volumes:                               # Named volumes declaration
  bitcoin-data:                        # Docker manages location
  loki-data:
  grafana-data:
  alloy-data:

networks:                              # Networks declaration
  monitoring:                          # Custom network name
    driver: bridge                     # Network driver
```

### Compose File Breakdown

#### Service Definition
```yaml
services:
  service-name:                  # How to reference in compose commands
    image: repo/name:tag         # Image to use
    container_name: actual-name  # Name in `docker ps`
```

#### Port Mapping
```yaml
ports:
  - "host:container"             # HOST_PORT:CONTAINER_PORT
  - "8080:80"                    # Access via localhost:8080
  - "38332:38332"                # Same port on host and container
```

#### Volumes
```yaml
volumes:
  - named-volume:/path           # Named volume (managed by Docker)
  - ./host/path:/container/path  # Bind mount (host directory)
  - ./config:/etc/app:ro         # Read-only bind mount
```

#### Environment Variables
```yaml
environment:
  - VAR_NAME=value               # Simple format
  - DATABASE_URL=postgres://...
```

#### Healthcheck
```yaml
healthcheck:
  test: ["CMD", "curl", "http://localhost/health"]  # Command to run
  interval: 30s                  # How often to check
  timeout: 10s                   # Max time for check
  retries: 3                     # Failures before unhealthy
  start_period: 60s              # Grace period on startup
```

#### Depends On
```yaml
depends_on:
  other-service:
    condition: service_started   # Wait for start
    condition: service_healthy   # Wait for healthy status
```

#### Restart Policy
```yaml
restart: no                      # Never restart (default)
restart: always                  # Always restart if stops
restart: on-failure              # Restart on non-zero exit
restart: unless-stopped          # Restart unless manually stopped
```

---

## 🎓 Docker Interview Scenarios

### Scenario 1: Container Won't Start

```bash
# Step 1: Check if container exists
docker ps -a | grep bitcoin
│       │   │  │    │
│       │   │  │    └─ Filter for bitcoin
│       │   │  └─ Include stopped containers
│       │   └─ List containers
│       └─ compose
└─ docker

# Step 2: Check logs for errors
docker logs bitcoin-signet
docker logs --tail 100 bitcoin-signet

# Step 3: Try to start manually
docker start bitcoin-signet
# If fails, check why:
docker inspect bitcoin-signet | jq '.[0].State'

# Step 4: Check port conflicts
ss -tulpn | grep 38332
# If port in use, kill that process or change port

# Step 5: Check volume issues
docker volume inspect bitcoin-data
# Check permissions of mountpoint

# Step 6: Recreate container
docker compose down
docker compose up -d bitcoin
```

### Scenario 2: High Resource Usage

```bash
# Check resource usage
docker stats --no-stream
│       │     │
│       │     └─ One snapshot
│       └─ stats
└─ docker

# Find resource hogs
docker stats --format "table {{.Container}}\t{{.CPUPerc}}\t{{.MemUsage}}" --no-stream | sort -k2 -rn

# Check container logs for issues
docker logs --tail 1000 bitcoin-signet | grep -i error

# Limit resources in compose
services:
  bitcoin:
    deploy:
      resources:
        limits:
          cpus: '2.0'
          memory: 2G
        reservations:
          cpus: '1.0'
          memory: 1G

# Restart with limits
docker compose down && docker compose up -d
```

### Scenario 3: Network Issues Between Containers

```bash
# Check network
docker network ls
docker network inspect luganode25_default

# Verify containers are on network
docker network inspect luganode25_default | jq '.[0].Containers'

# Test connectivity from one container to another
docker exec grafana-alloy ping -c 2 loki
docker exec grafana-alloy curl http://loki:3100/ready

# Check DNS resolution
docker exec grafana-alloy nslookup loki
docker exec grafana-alloy getent hosts loki

# Reconnect container to network
docker network disconnect luganode25_default grafana-alloy
docker network connect luganode25_default grafana-alloy

# Recreate network
docker compose down
docker network rm luganode25_default
docker compose up -d
```

### Scenario 4: Volume Data Issues

```bash
# Check volume exists
docker volume ls | grep bitcoin
│       │      │  │    │
│       │      │  │    └─ Filter
│       │      │  └─ Pipe to grep
│       │      └─ List volumes
│       └─ volume
└─ docker

# Inspect volume
docker volume inspect bitcoin-data
# Check "Mountpoint" - that's actual location

# Check volume contents
docker run --rm -v bitcoin-data:/data alpine ls -la /data
│       │   │   │ │             │     │      │    │
│       │   │   │ │             │     │      │    └─ Command
│       │   │   │ │             │     │      └─ Image
│       │   │   │ │             │     └─ Container path
│       │   │   │ │             └─ Volume name
│       │   │   │ └─ Mount volume
│       │   │   └─ Remove container after exit
│       │   └─ run
│       └─ compose
└─ docker

# Backup volume
docker run --rm \
  -v bitcoin-data:/source:ro \
  -v $(pwd)/backup:/backup \
  alpine tar czf /backup/bitcoin-data-$(date +%Y%m%d).tar.gz /source

# Restore volume
docker run --rm \
  -v bitcoin-data:/target \
  -v $(pwd)/backup:/backup \
  alpine tar xzf /backup/bitcoin-data-20241022.tar.gz -C /target

# Remove volume (CAREFUL!)
docker compose down -v  # Removes volumes
```

### Scenario 5: Image Update

```bash
# Pull latest image
docker compose pull bitcoin
│       │       │    │
│       │       │    └─ Service name
│       │       └─ pull
│       └─ compose
└─ docker

# Recreate with new image
docker compose up -d --force-recreate bitcoin
│       │       │  │  │                │
│       │       │  │  │                └─ Service
│       │       │  │  └─ Force recreate
│       │       │  └─ Detached
│       │       └─ up
│       └─ compose
└─ docker

# Verify new image
docker images | grep bitcoin
docker inspect bitcoin-signet | jq '.[0].Image'

# Rollback if needed
docker tag bitcoin/bitcoin:latest bitcoin/bitcoin:previous
docker compose down
# Change docker-compose.yml to use previous tag
docker compose up -d
```

---

## 🏆 Docker Interview Questions You'll Nail

### Basic Questions

**Q: What's the difference between an image and a container?**
```
Image = Template/Blueprint (stored on disk)
Container = Running instance of image (active process)

Like: Recipe vs Cooked meal
     Class vs Object
     Program vs Process
```

**Q: How do you make container data persistent?**
```bash
# Use volumes
docker run -v my-data:/app/data nginx

# Your project example:
# bitcoin-data volume persists blockchain data
# Even if container is removed, data remains
```

**Q: How do containers communicate?**
```bash
# On same network, use service names
docker exec grafana curl http://loki:3100/ready
# Docker DNS resolves 'loki' to container IP
```

### Intermediate Questions

**Q: Explain Docker networking modes**
```
1. bridge (default):
   - Isolated network for containers
   - Containers can talk to each other
   - Your project uses this

2. host:
   - Container uses host's network directly
   - No isolation, best performance

3. none:
   - No networking
   - Completely isolated

4. container:
   - Share network with another container
```

**Q: How do you debug a container that's crashing?**
```bash
# 1. Check logs
docker logs --tail 100 bitcoin-signet

# 2. Check exit code
docker inspect bitcoin-signet | jq '.[0].State'

# 3. Try to start with shell
docker run -it --entrypoint /bin/bash bitcoin/bitcoin

# 4. Check resource limits
docker stats bitcoin-signet

# 5. Check dependencies
docker compose logs
```

**Q: What's the difference between CMD and ENTRYPOINT?**
```dockerfile
# CMD: Default command, can be overridden
CMD ["nginx", "-g", "daemon off;"]
# Override: docker run image echo "hello"

# ENTRYPOINT: Always runs, args appended
ENTRYPOINT ["nginx"]
# Override args: docker run image -g "daemon off;"

# Both together:
ENTRYPOINT ["bitcoind"]
CMD ["-signet", "-rest"]
# Can override CMD but ENTRYPOINT always runs
```

### Advanced Questions

**Q: How would you optimize Docker image size?**
```dockerfile
# Multi-stage builds
FROM golang:1.21 AS builder
WORKDIR /app
COPY . .
RUN go build -o app

FROM alpine:latest          # Smaller base image
COPY --from=builder /app/app /app
CMD ["/app"]

# Your project uses optimized images:
# grafana/alloy:latest is already optimized
```

**Q: Explain Docker layer caching**
```
Each Dockerfile instruction creates a layer
Layers are cached and reused

FROM ubuntu              # Layer 1 (cached)
RUN apt update          # Layer 2 (cached if same)
COPY . /app             # Layer 3 (new if files changed)
RUN npm install         # Layer 4 (runs again if Layer 3 changed)

Optimization: Order instructions from least to most frequently changing
```

**Q: How do you handle secrets in Docker?**
```bash
# DON'T DO THIS (your demo does for simplicity):
environment:
  - PASSWORD=luganodes2024  # Bad: visible in docker inspect

# BETTER OPTIONS:
# 1. Docker secrets (Swarm mode)
# 2. Environment file not in git
# 3. HashiCorp Vault
# 4. Kubernetes secrets
# 5. Docker buildx secret mounts

# Example:
docker run -e PASSWORD_FILE=/run/secrets/password app
```

---

## 📋 Docker Quick Reference Cheat Sheet

### Container Lifecycle
```bash
docker run IMAGE                      # Create + Start
docker start CONTAINER                # Start stopped
docker stop CONTAINER                 # Stop running
docker restart CONTAINER              # Restart
docker pause CONTAINER                # Pause processes
docker unpause CONTAINER              # Resume
docker rm CONTAINER                   # Remove stopped
docker rm -f CONTAINER                # Force remove
```

### Inspection & Debugging
```bash
docker ps                             # Running containers
docker ps -a                          # All containers
docker logs CONTAINER                 # View logs
docker logs -f CONTAINER              # Follow logs
docker inspect CONTAINER              # Full details
docker stats                          # Resource usage
docker exec -it CONTAINER bash        # Interactive shell
docker top CONTAINER                  # Running processes
docker port CONTAINER                 # Port mappings
```

### Images
```bash
docker images                         # List images
docker pull IMAGE                     # Download image
docker rmi IMAGE                      # Remove image
docker tag SOURCE TARGET              # Tag image
docker history IMAGE                  # Image layers
docker image prune                    # Remove unused
```

### Volumes
```bash
docker volume ls                      # List volumes
docker volume create NAME             # Create volume
docker volume inspect NAME            # Volume details
docker volume rm NAME                 # Remove volume
docker volume prune                   # Remove unused
```

### Networks
```bash
docker network ls                     # List networks
docker network create NAME            # Create network
docker network inspect NAME           # Network details
docker network connect NET CONT       # Connect container
docker network disconnect NET CONT    # Disconnect
docker network rm NAME                # Remove network
```

### Docker Compose
```bash
docker compose up                     # Start all
docker compose up -d                  # Start detached
docker compose down                   # Stop + remove
docker compose down -v                # Stop + remove volumes
docker compose ps                     # List services
docker compose logs                   # View logs
docker compose logs -f                # Follow logs
docker compose exec SERVICE CMD       # Run command
docker compose restart                # Restart all
docker compose pull                   # Pull images
```

### System Cleanup
```bash
docker system df                      # Disk usage
docker system prune                   # Remove unused
docker system prune -a                # Remove ALL unused
docker container prune                # Remove stopped containers
docker image prune                    # Remove dangling images
docker volume prune                   # Remove unused volumes
docker network prune                  # Remove unused networks
```

---

## 🎯 Practice Exercises for Interview

### Exercise 1: Recreate Your Bitcoin Service Manually
```bash
# Without docker-compose, create bitcoin container
cd ~/luganode2025/luganode25

# 1. Create network
docker network create bitcoin-net

# 2. Create volume
docker volume create bitcoin-test-data

# 3. Run container
docker run -d \
  --name bitcoin-test \
  --network bitcoin-net \
  -p 38334:38332 \
  -v bitcoin-test-data:/root/.bitcoin \
  -e BITCOIN_NETWORK=signet \
  bitcoindevproject/bitcoin:latest \
  bitcoind -signet -rest -rpcuser=test -rpcpassword=test

# 4. Verify
docker ps | grep bitcoin-test
docker logs bitcoin-test
docker exec bitcoin-test bitcoin-cli -signet getblockchaininfo

# 5. Cleanup
docker stop bitcoin-test
docker rm bitcoin-test
docker volume rm bitcoin-test-data
docker network rm bitcoin-net
```

### Exercise 2: Debug Container Issues
```bash
# Create a broken container
docker run -d --name broken-nginx -p 80:80 nginx
# Oops! Port 80 might be in use

# Debug process:
# 1. Check if started
docker ps -a | grep broken

# 2. Check logs
docker logs broken-nginx

# 3. Check port conflict
ss -tulpn | grep :80

# 4. Fix: Use different port
docker rm broken-nginx
docker run -d --name fixed-nginx -p 8080:80 nginx

# 5. Verify
curl http://localhost:8080

# 6. Cleanup
docker stop fixed-nginx && docker rm fixed-nginx
```

### Exercise 3: Volume Backup & Restore
```bash
# Backup your bitcoin data
docker run --rm \
  -v luganode25_bitcoin-data:/source:ro \
  -v $(pwd):/backup \
  alpine tar czf /backup/bitcoin-backup.tar.gz -C /source .

# Restore to new volume
docker volume create bitcoin-restored
docker run --rm \
  -v bitcoin-restored:/target \
  -v $(pwd):/backup \
  alpine tar xzf /backup/bitcoin-backup.tar.gz -C /target

# Verify
docker run --rm -v bitcoin-restored:/data alpine ls -la /data

# Cleanup
docker volume rm bitcoin-restored
rm bitcoin-backup.tar.gz
```

---

## 🚀 Final Docker Confidence Builders

### You Already Master Docker!
Your project demonstrates:
1. ✅ Multi-container orchestration (4 services)
2. ✅ Volume management (persistent data)
3. ✅ Network configuration (container communication)
4. ✅ Health checks (service monitoring)
5. ✅ Port mapping (external access)
6. ✅ Environment configuration (settings management)
7. ✅ Dependency management (depends_on)
8. ✅ Restart policies (high availability)

### Before Your Interview: 5-Minute Docker Drill
```bash
cd ~/luganode2025/luganode25

# Rapid fire commands (say what each does out loud)
docker compose ps                                          # Check services
docker logs --tail 20 bitcoin-signet                      # Recent logs
docker stats --no-stream                                  # Resource usage
docker exec bitcoin-signet bitcoin-cli -signet getinfo   # Execute command
docker inspect bitcoin-signet | jq '.[0].State.Status'   # Container status
docker network inspect luganode25_default                 # Network info
docker volume inspect luganode25_bitcoin-data            # Volume details
```

### Interview Power Phrases
- "I use Docker Compose to orchestrate my Bitcoin monitoring stack"
- "I implement healthchecks to ensure service reliability"
- "I use named volumes for data persistence across container restarts"
- "My containers communicate via Docker's internal DNS on a custom bridge network"
- "I can troubleshoot issues using docker logs, inspect, and exec commands"

---

**You're Docker-ready!** You've built a production-grade multi-container application. That's more than most developers ever do. 🐳🎉

Good luck with your interview! 🚀