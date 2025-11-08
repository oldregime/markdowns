# Fedora Linux Terminal & Package Management Guide

## 🎯 Welcome to Fedora!

Fedora is a cutting-edge Linux distribution that uses **RPM** packages and **DNF** as its package manager (unlike Ubuntu's APT). Here's everything you need to know!

---

## 📦 Package Management in Fedora

### Understanding Fedora's Package Ecosystem

```
Fedora Package Sources:
│
├─ DNF (Default Package Manager)
│  └─ Official Fedora Repositories (.rpm packages)
│
├─ Flatpak (Sandboxed Apps)
│  └─ Flathub (Universal Linux Apps)
│
├─ AppImage (Portable Apps)
│  └─ Self-contained executables
│
└─ Snap (Optional)
   └─ Canonical's universal packages
```

---

## 🔧 DNF - Fedora's Package Manager

### What is DNF?

**DNF** (Dandified YUM) is Fedora's modern package manager that replaces the older YUM.

**Template:**
```bash
sudo dnf [OPTIONS] COMMAND [PACKAGE]
│     │   │         │       │
│     │   │         │       └─ Package name
│     │   │         └─ Action (install, remove, update, etc.)
│     │   └─ Options
│     └─ dnf command
└─ sudo (run as admin)
```

### Essential DNF Commands

#### Update System
```bash
# Update package list and upgrade all packages
sudo dnf upgrade                  # Update all packages
│     │   │
│     │   └─ upgrade command
│     └─ dnf
└─ sudo

# Alternative (same thing)
sudo dnf update                   # Same as upgrade

# Update specific package
sudo dnf upgrade firefox

# Check for updates without installing
sudo dnf check-update
│     │   │
│     │   └─ check-update command
│     └─ dnf
└─ sudo
```

#### Install Packages
```bash
# Install single package
sudo dnf install htop
│     │   │       │
│     │   │       └─ Package name
│     │   └─ install command
│     └─ dnf
└─ sudo

# Install multiple packages
sudo dnf install vim git curl wget
│     │   │       │   │   │    │
│     │   │       └─ Multiple packages separated by space
│     │   └─ install
│     └─ dnf
└─ sudo

# Install without confirmation prompt
sudo dnf install -y docker
│     │   │       │ │
│     │   │       │ └─ Package
│     │   │       └─ y = assume yes
│     │   └─ install
│     └─ dnf
└─ sudo

# Install from specific repository
sudo dnf install package-name --enablerepo=repo-name
```

#### Search Packages
```bash
# Search for package
dnf search docker                 # Search by name and description
│   │      │
│   │      └─ Search term
│   └─ search command
└─ dnf (no sudo needed for search)

# Search only in package names
dnf search --names-only docker

# Get package information
dnf info docker                   # Detailed package info
│   │    │
│   │    └─ Package name
│   └─ info command
└─ dnf

# List all available packages
dnf list available                # All installable packages
dnf list installed                # Installed packages
dnf list updates                  # Available updates
```

#### Remove Packages
```bash
# Remove package
sudo dnf remove firefox           # Remove package
│     │   │      │
│     │   │      └─ Package to remove
│     │   └─ remove command
│     └─ dnf
└─ sudo

# Remove with dependencies
sudo dnf autoremove firefox       # Remove package + unused deps
│     │   │          │
│     │   │          └─ Package
│     │   └─ autoremove (removes orphaned dependencies)
│     └─ dnf
└─ sudo

# Clean up unused dependencies
sudo dnf autoremove               # Remove all orphaned packages
```

#### Package Groups
```bash
# List available groups
dnf group list                    # Show package groups
│   │     │
│   │     └─ list groups
│   └─ group command
└─ dnf

# Install group
sudo dnf group install "Development Tools"
│     │   │     │       │
│     │   │     │       └─ Group name (use quotes if spaces)
│     │   │     └─ install group
│     │   └─ group
│     └─ dnf
└─ sudo

# Common groups
sudo dnf group install "C Development Tools and Libraries"
sudo dnf group install "System Tools"
sudo dnf group install "Virtualization"
```

#### History & Rollback
```bash
# View DNF history
sudo dnf history                  # Show recent transactions
│     │   │
│     │   └─ history command
│     └─ dnf
└─ sudo

# View specific transaction
sudo dnf history info 5           # Details of transaction #5

# Undo last transaction
sudo dnf history undo last        # Rollback last install/remove
│     │   │       │    │
│     │   │       │    └─ last (or transaction ID)
│     │   │       └─ undo command
│     │   └─ history
│     └─ dnf
└─ sudo

# Redo transaction
sudo dnf history redo 5           # Repeat transaction #5
```

#### Cache Management
```bash
# Clean cache
sudo dnf clean all                # Remove cached packages
│     │   │     │
│     │   │     └─ all (packages, metadata, etc.)
│     │   └─ clean command
│     └─ dnf
└─ sudo

# Build cache
sudo dnf makecache                # Download package metadata
│     │   │
│     │   └─ makecache (refresh cache)
│     └─ dnf
└─ sudo
```

### DNF Configuration

**Config file location:**
```bash
# Main DNF config
sudo nano /etc/dnf/dnf.conf
│     │    │
│     │    └─ DNF config file
│     └─ nano (text editor)
└─ sudo

# Useful settings to add:
max_parallel_downloads=10         # Faster downloads
fastestmirror=True                # Use fastest mirror
defaultyes=True                   # Auto-yes to prompts
```

---

## 📱 Flatpak - Universal Linux Apps

### What is Flatpak?

**Flatpak** packages apps in sandboxed containers, making them work across all Linux distros. Think of it as "Docker for desktop apps."

### Setting Up Flatpak

```bash
# Flatpak is pre-installed on Fedora
# Verify installation
flatpak --version
│        │
│        └─ Check version
└─ flatpak command

# Add Flathub repository (if not added)
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo
│        │         │                │       │
│        │         │                │       └─ Flathub repo URL
│        │         │                └─ Remote name
│        │         └─ Only add if doesn't exist
│        └─ remote-add (add repository)
└─ flatpak

# Verify Flathub is added
flatpak remotes                   # List repositories
│        │
│        └─ remotes command
└─ flatpak
```

### Essential Flatpak Commands

#### Search & Install Apps
```bash
# Search for apps
flatpak search spotify            # Search Flathub
│        │      │
│        │      └─ Search term
│        └─ search command
└─ flatpak

# Install app
flatpak install flathub com.spotify.Client
│        │       │       │
│        │       │       └─ App ID (reverse domain format)
│        │       └─ Repository name
│        └─ install command
└─ flatpak

# Install from URL
flatpak install https://flathub.org/repo/appstream/org.gimp.GIMP.flatpakref
```

#### List & Run Apps
```bash
# List installed apps
flatpak list                      # All installed Flatpaks
│        │
│        └─ list command
└─ flatpak

# List only applications (not runtimes)
flatpak list --app
│        │    │
│        │    └─ --app flag
│        └─ list
└─ flatpak

# Run Flatpak app
flatpak run com.spotify.Client
│        │   │
│        │   └─ App ID
│        └─ run command
└─ flatpak

# Run with verbose output
flatpak run -v com.spotify.Client
│        │   │
│        │   └─ v = verbose
│        └─ run
└─ flatpak
```

#### Update & Remove Apps
```bash
# Update all Flatpaks
flatpak update                    # Update all apps
│        │
│        └─ update command
└─ flatpak

# Update specific app
flatpak update com.spotify.Client

# Remove app
flatpak uninstall com.spotify.Client
│        │         │
│        │         └─ App ID
│        └─ uninstall command
└─ flatpak

# Remove with data
flatpak uninstall --delete-data com.spotify.Client
│        │         │             │
│        │         │             └─ App ID
│        │         └─ Delete user data too
│        └─ uninstall
└─ flatpak

# Remove unused runtimes
flatpak uninstall --unused        # Clean up old runtimes
│        │         │
│        │         └─ Remove orphaned dependencies
│        └─ uninstall
└─ flatpak
```

#### Manage Permissions
```bash
# View app permissions
flatpak info --show-permissions com.spotify.Client
│        │    │                  │
│        │    │                  └─ App ID
│        │    └─ Show permissions
│        └─ info command
└─ flatpak

# Override permissions (requires Flatseal app)
flatpak install flathub com.github.tchx84.Flatseal  # GUI permission manager
```

### Popular Flatpak Apps

```bash
# Browsers
flatpak install flathub org.mozilla.firefox
flatpak install flathub com.google.Chrome
flatpak install flathub com.brave.Browser

# Communication
flatpak install flathub com.discordapp.Discord
flatpak install flathub org.telegram.desktop
flatpak install flathub com.slack.Slack

# Media
flatpak install flathub com.spotify.Client
flatpak install flathub org.videolan.VLC
flatpak install flathub com.obsproject.Studio     # OBS Studio

# Development
flatpak install flathub com.visualstudio.code     # VS Code
flatpak install flathub org.gimp.GIMP             # Image editor
flatpak install flathub org.inkscape.Inkscape     # Vector graphics

# Gaming
flatpak install flathub com.valvesoftware.Steam
flatpak install flathub com.heroicgameslauncher.hgl
```

---

## 🖼️ AppImage - Portable Applications

### What is AppImage?

**AppImage** files are self-contained executables that run without installation. Like a Windows `.exe` but for Linux.

### Using AppImages

```bash
# Download AppImage (example: Obsidian)
cd ~/Downloads
wget https://github.com/obsidianmd/obsidian-releases/releases/download/v1.7.7/Obsidian-1.7.7.AppImage
│     │
│     └─ Download URL
└─ wget (download tool)

# Make AppImage executable
chmod +x Obsidian-1.7.7.AppImage
│      │  │
│      │  └─ AppImage file
│      └─ +x (add execute permission)
└─ chmod

# Run AppImage
./Obsidian-1.7.7.AppImage
│ │
│ └─ AppImage file
└─ ./ (current directory)

# Create desktop entry (optional)
mkdir -p ~/.local/share/applications
│      │  │
│      │  └─ Applications directory
│      └─ p = create parents
└─ mkdir

# Create .desktop file
cat > ~/.local/share/applications/obsidian.desktop << EOF
[Desktop Entry]
Type=Application
Name=Obsidian
Exec=/home/divyansh/Downloads/Obsidian-1.7.7.AppImage
Icon=/home/divyansh/Downloads/obsidian-icon.png
Categories=Office;
EOF

# Update desktop database
update-desktop-database ~/.local/share/applications
│                        │
│                        └─ Applications directory
└─ update-desktop-database
```

### Managing AppImages

```bash
# Create AppImages directory
mkdir -p ~/Applications
cd ~/Applications

# Move AppImage there
mv ~/Downloads/Obsidian-1.7.7.AppImage ~/Applications/

# Create symlink for easy access
sudo ln -s ~/Applications/Obsidian-1.7.7.AppImage /usr/local/bin/obsidian
│     │   │  │                                    │
│     │   │  │                                    └─ Link name
│     │   │  └─ Target file
│     │   └─ s = symbolic link
│     └─ ln (link command)
└─ sudo

# Now run from anywhere
obsidian

# Remove AppImage
rm ~/Applications/Obsidian-1.7.7.AppImage
rm /usr/local/bin/obsidian        # Remove symlink
rm ~/.local/share/applications/obsidian.desktop  # Remove desktop entry
```

---

## 🚫 .deb Files (Ubuntu/Debian Packages)

### The Problem

**Fedora cannot directly install `.deb` files** because:
- Fedora uses **RPM** packages (`.rpm`)
- Ubuntu/Debian use **DEB** packages (`.deb`)
- They have different package formats and dependency management

### Solutions

#### Option 1: Find RPM Alternative
```bash
# Search for RPM version
dnf search package-name
│   │      │
│   │      └─ Package to search
│   └─ search
└─ dnf

# Example: Instead of Chrome .deb
sudo dnf install google-chrome-stable  # From Google's RPM repo
```

#### Option 2: Convert .deb to .rpm (Not Recommended)
```bash
# Install alien (converter tool)
sudo dnf install alien
│     │   │       │
│     │   │       └─ Conversion tool
│     │   └─ install
│     └─ dnf
└─ sudo

# Convert .deb to .rpm
sudo alien -r package.deb         # Creates package.rpm
│     │     │ │
│     │     │ └─ .deb file
│     │     └─ r = convert to RPM
│     └─ alien
└─ sudo

# Install converted RPM
sudo dnf install package.rpm

# ⚠️ Warning: This often breaks! Dependencies might not match.
```

#### Option 3: Use Flatpak Instead
```bash
# Most apps available as .deb are also on Flathub
flatpak search app-name
flatpak install flathub com.app.Name

# Example: VS Code
flatpak install flathub com.visualstudio.code
# Instead of downloading vscode.deb
```

#### Option 4: Extract .deb Manually
```bash
# Extract .deb contents (for viewing only)
ar -x package.deb                 # Extract .deb archive
│  │  │
│  │  └─ .deb file
│  └─ x = extract
└─ ar (archive tool)

# This creates:
# - data.tar.xz (actual files)
# - control.tar.xz (package metadata)

# Extract data
tar -xf data.tar.xz
│   │  │
│   │  └─ Archive file
│   └─ xf = extract file
└─ tar

# Manually copy files (advanced users only)
# ⚠️ Not recommended - breaks package management
```

---

## 🎛️ Third-Party Repositories

### RPM Fusion (Essential for Fedora)

**RPM Fusion** provides multimedia codecs and proprietary software not in official repos.

```bash
# Enable RPM Fusion Free
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm
│     │   │       │
│     │   │       └─ RPM Fusion Free repo RPM
│     │   └─ install
│     └─ dnf
└─ sudo

# Enable RPM Fusion Nonfree (proprietary software)
sudo dnf install https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm

# Install multimedia codecs
sudo dnf install gstreamer1-plugins-{bad-\*,good-\*,base} gstreamer1-plugin-openh264 gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel

sudo dnf install lame\* --exclude=lame-devel

sudo dnf group upgrade --with-optional Multimedia

# Install ffmpeg (video/audio processing)
sudo dnf install ffmpeg ffmpeg-libs
```

### Google Chrome Repository

```bash
# Add Google Chrome repo
sudo dnf install fedora-workstation-repositories
│     │   │       │
│     │   │       └─ Repository package
│     │   └─ install
│     └─ dnf
└─ sudo

# Enable Chrome repo
sudo dnf config-manager --set-enabled google-chrome
│     │   │             │              │
│     │   │             │              └─ Repo name
│     │   │             └─ Enable repo
│     │   └─ config-manager
│     └─ dnf
└─ sudo

# Install Chrome
sudo dnf install google-chrome-stable
```

### VS Code Repository

```bash
# Add Microsoft repo key
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
│     │   │        │
│     │   │        └─ GPG key URL
│     │   └─ import key
│     └─ rpm
└─ sudo

# Add VS Code repo
sudo sh -c 'echo -e "[code]\nname=Visual Studio Code\nbaseurl=https://packages.microsoft.com/yumrepos/vscode\nenabled=1\ngpgcheck=1\ngpgkey=https://packages.microsoft.com/keys/microsoft.asc" > /etc/yum.repos.d/vscode.repo'

# Install VS Code
sudo dnf install code
```

### Docker Repository

```bash
# Add Docker repo
sudo dnf config-manager --add-repo https://download.docker.com/linux/fedora/docker-ce.repo
│     │   │             │          │
│     │   │             │          └─ Docker repo URL
│     │   │             └─ Add repository
│     │   └─ config-manager
│     └─ dnf
└─ sudo

# Install Docker
sudo dnf install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin

# Start Docker
sudo systemctl start docker
sudo systemctl enable docker

# Add user to docker group
sudo usermod -aG docker $USER
```

---

## 🔍 Finding Software in Fedora

### Where to Look

```bash
# 1. Official Fedora Repos (DNF)
dnf search package-name
dnf info package-name

# 2. Flathub (Flatpak)
flatpak search app-name
# Or visit: https://flathub.org

# 3. RPM Fusion
dnf search package-name --enablerepo=rpmfusion-free
dnf search package-name --enablerepo=rpmfusion-nonfree

# 4. Snap Store (optional)
# Install snapd first:
sudo dnf install snapd
sudo snap install package-name

# 5. AppImage
# Visit: https://appimage.github.io/apps/
# Or app's official website
```

---

## 🛠️ Essential Fedora Tools

### Must-Install Packages

```bash
# Development tools
sudo dnf group install "Development Tools"
sudo dnf install git vim neovim htop ncdu
│     │   │       │   │   │      │    │
│     │   │       │   │   │      │    └─ Disk usage analyzer
│     │   │       │   │   │      └─ Better top
│     │   │       │   │   └─ Modern vim
│     │   │       │   └─ Classic vim
│     │   │       └─ Version control
│     │   └─ install
│     └─ dnf
└─ sudo

# System utilities
sudo dnf install btop fastfetch neofetch
│     │   │       │    │         │
│     │   │       │    │         └─ System info (old)
│     │   │       │    └─ System info (new)
│     │   │       └─ Better resource monitor
│     │   └─ install
│     └─ dnf
└─ sudo

# Network tools
sudo dnf install curl wget nmap telnet bind-utils
│     │   │       │    │    │    │      │
│     │   │       │    │    │    │      └─ DNS tools (dig, nslookup)
│     │   │       │    │    │    └─ Telnet client
│     │   │       │    │    └─ Port scanner
│     │   │       │    └─ Download tool
│     │   │       └─ URL transfer tool
│     │   └─ install
│     └─ dnf
└─ sudo

# Compression tools
sudo dnf install p7zip p7zip-plugins unrar
│     │   │       │     │              │
│     │   │       │     │              └─ RAR support
│     │   │       │     └─ 7zip plugins
│     │   │       └─ 7zip
│     │   └─ install
│     └─ dnf
└─ sudo

# Multimedia codecs (from RPM Fusion)
sudo dnf install ffmpeg vlc
```

### GUI Package Managers

```bash
# GNOME Software (pre-installed on Fedora Workstation)
# - Graphical app store
# - Supports DNF, Flatpak, RPM packages

# Install KDE Discover (for KDE users like you)
sudo dnf install plasma-discover
│     │   │       │
│     │   │       └─ KDE app store
│     │   └─ install
│     └─ dnf
└─ sudo

# Install GNOME Software (if not installed)
sudo dnf install gnome-software
```

---

## 🎯 Quick Reference: Package Management Comparison

### Fedora vs Ubuntu Command Equivalents

| Task | Ubuntu (APT) | Fedora (DNF) |
|------|-------------|-------------|
| Update package list | `sudo apt update` | `sudo dnf check-update` |
| Upgrade packages | `sudo apt upgrade` | `sudo dnf upgrade` |
| Install package | `sudo apt install pkg` | `sudo dnf install pkg` |
| Remove package | `sudo apt remove pkg` | `sudo dnf remove pkg` |
| Search package | `apt search pkg` | `dnf search pkg` |
| Package info | `apt show pkg` | `dnf info pkg` |
| List installed | `apt list --installed` | `dnf list installed` |
| Clean cache | `sudo apt clean` | `sudo dnf clean all` |
| Auto-remove | `sudo apt autoremove` | `sudo dnf autoremove` |

---

## 📋 Common Fedora Tasks

### System Update Routine

```bash
# Daily update routine
sudo dnf upgrade -y               # Update DNF packages
flatpak update -y                 # Update Flatpaks
│        │      │
│        │      └─ y = assume yes
│        └─ update
└─ flatpak

# Weekly cleanup
sudo dnf autoremove               # Remove orphaned packages
sudo dnf clean all                # Clean cache
flatpak uninstall --unused -y     # Remove unused Flatpak runtimes
```

### Install Common Apps

```bash
# Web browsers
sudo dnf install firefox          # Firefox (DNF)
flatpak install flathub com.google.Chrome  # Chrome (Flatpak)

# Media players
flatpak install flathub org.videolan.VLC
flatpak install flathub com.spotify.Client

# Office suite
flatpak install flathub org.libreoffice.LibreOffice

# Communication
flatpak install flathub com.discordapp.Discord
flatpak install flathub org.telegram.desktop

# Development
flatpak install flathub com.visualstudio.code
sudo dnf install git vim docker
```

### Troubleshooting

```bash
# Fix broken dependencies
sudo dnf distro-sync              # Sync with repos
│     │   │
│     │   └─ distro-sync (fix version mismatches)
│     └─ dnf
└─ sudo

# Rebuild RPM database
sudo rpm --rebuilddb
│     │   │
│     │   └─ Rebuild database
│     └─ rpm
└─ sudo

# Check for duplicate packages
sudo dnf repoquery --duplicates
│     │   │         │
│     │   │         └─ Find duplicates
│     │   └─ repoquery
│     └─ dnf
└─ sudo

# Fix Flatpak issues
flatpak repair                    # Repair installations
│        │
│        └─ repair command
└─ flatpak
```

---

## 🚀 Pro Tips for Fedora

### Speed Up DNF

```bash
# Edit DNF config
sudo nano /etc/dnf/dnf.conf

# Add these lines:
max_parallel_downloads=10
fastestmirror=True
deltarpm=True
```

### Create Command Aliases

```bash
# Edit bash config
nano ~/.bashrc

# Add aliases:
alias update='sudo dnf upgrade -y && flatpak update -y'
alias install='sudo dnf install'
alias remove='sudo dnf remove'
alias search='dnf search'

# Reload bash
source ~/.bashrc
│       │
│       └─ Reload config
└─ source
```

### Enable DNF Automatic Updates

```bash
# Install automatic updates
sudo dnf install dnf-automatic
│     │   │       │
│     │   │       └─ Automatic updater
│     │   └─ install
│     └─ dnf
└─ sudo

# Configure
sudo nano /etc/dnf/automatic.conf
# Set: apply_updates = yes

# Enable service
sudo systemctl enable --now dnf-automatic.timer
│     │         │      │     │
│     │         │      │     └─ Timer unit
│     │         │      └─ Start immediately
│     │         └─ enable
│     └─ systemctl
└─ sudo
```

---

## 🎓 Your First Steps on Fedora

### Day 1 Setup Script

```bash
#!/bin/bash
# Save as setup-fedora.sh and run: bash setup-fedora.sh

echo "🚀 Setting up Fedora..."

# Update system
echo "📦 Updating system..."
sudo dnf upgrade -y

# Enable RPM Fusion
echo "📡 Enabling RPM Fusion..."
sudo dnf install https://mirrors.rpmfusion.org/free/fedora/rpmfusion-free-release-$(rpm -E %fedora).noarch.rpm -y
sudo dnf install https://mirrors.rpmfusion.org/nonfree/fedora/rpmfusion-nonfree-release-$(rpm -E %fedora).noarch.rpm -y

# Install essential tools
echo "🛠️ Installing essential tools..."
sudo dnf install -y vim git htop btop fastfetch curl wget

# Install multimedia codecs
echo "🎵 Installing multimedia codecs..."
sudo dnf install -y gstreamer1-plugins-{bad-\*,good-\*,base} gstreamer1-plugin-openh264 gstreamer1-libav --exclude=gstreamer1-plugins-bad-free-devel
sudo dnf install -y ffmpeg

# Setup Flatpak
echo "📱 Setting up Flatpak..."
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# Install common Flatpak apps
echo "📲 Installing Flatpak apps..."
flatpak install flathub -y \
  org.videolan.VLC \
  com.spotify.Client \
  com.discordapp.Discord \
  com.visualstudio.code

# Configure DNF
echo "⚡ Optimizing DNF..."
echo "max_parallel_downloads=10" | sudo tee -a /etc/dnf/dnf.conf
echo "fastestmirror=True" | sudo tee -a /etc/dnf/dnf.conf

echo "✅ Setup complete! Restart your terminal."
```

---

**Welcome to Fedora!** You're now equipped to manage packages like a pro! 🎉🐧

Remember:
- **DNF** for system packages
- **Flatpak** for apps
- **AppImage** for portable apps
- **Avoid .deb files** - find RPM or Flatpak alternatives instead

Good luck exploring Fedora! 🚀