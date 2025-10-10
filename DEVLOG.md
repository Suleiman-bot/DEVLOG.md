#!/bin/bash
# ==========================================================
# DEVLOG REFERENCE FILE
# Author: Abdulsalam Suleiman
# Description: Consolidated Development Operations Log & Reference
# Date generated: 2025-10-08
# ==========================================================

# ----------------------------------------------------------
# [2025-08-23] DEVLOG Initialization
# ----------------------------------------------------------
# Setup of DEVLOG for documenting troubleshooting steps, fixes,
# and operational references across environments.
# ----------------------------------------------------------

# No commands executed — initial setup entry.

# Notes:
# This file is used to track commands, issues, and resolutions to
# prevent repetition and streamline debugging workflow.

# ==========================================================
# [2025-08-20] VirtualBox Autostart Configuration
# ==========================================================

# Attempt to enable autostart for VM "testServer"
VBoxManage modifyvm "testServer" --autostart-enabled on
# → Failed: VM name not found. Verified names below.

VBoxManage list vms
# → Used to confirm the exact VM name before retrying autostart.

# Retry using VM name and UUID (failed)
sudo VBoxManage modifyvm "testServer {5f0d8390-a982-4a85-b4a3-e457ec19e1a6}" --autostart-enabled on

# Correct approach: Use *exact* VM name as shown in VBoxManage list
VBoxManage modifyvm "<exact_vm_name>" --autostart-enabled on

# Set global autostart database path
VBoxManage setproperty autostartdbpath /etc/vbox

# Enable and start VirtualBox autostart service
sudo systemctl enable vboxautostart-service
sudo systemctl start vboxautostart-service

# Define VM autostop behavior (preserve state)
VBoxManage modifyvm "<exact_vm_name>" --autostop-type savestate

# ==========================================================
# [2025-08-20] Network Configuration (VM)
# ==========================================================

# Display all interfaces and IPs
ip a

# Display routing table and default gateway
ip r

# Apply and validate Netplan configuration
sudo netplan generate
sudo netplan apply

# Example Netplan YAML for static IP
# /etc/netplan/01-netcfg.yaml
# ---------------------------
# network:
#   version: 2
#   renderer: networkd
#   ethernets:
#     enp0s3:
#       dhcp4: no
#       addresses: [192.168.1.50/24]
#       gateway4: 192.168.1.1
#       nameservers:
#         addresses: [8.8.8.8, 8.8.4.4]

# ==========================================================
# [2025-08-20] SSH Setup on VM
# ==========================================================

sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh

# Test remote SSH access
ssh user@192.168.1.50

# ==========================================================
# [2025-08-19] WSL & Windows Environment Troubleshooting
# ==========================================================

# Install WSL
wsl --install

# Repair and verify WSL binaries
Get-FileHash "C:\Windows\System32\wsl.exe" -Algorithm SHA256
wsl.exe /repair

# Install via winget if repair fails
winget install --id Microsoft.WSL
winget install --id Microsoft.WSL --source winget --accept-package-agreements --accept-source-agreements

# Verify and manage winget sources
winget source list
winget --version

# Run system integrity check
sfc /scannow

# Check WSL status and installed distros
wsl --status
wsl -l -v

# Install and launch Ubuntu distribution
wsl --install -d Ubuntu-22.04
wsl -d Ubuntu

# Navigate into project folder
cd ~
cd ~/plane

# ==========================================================
# [2025-08-19] GitHub Configuration & Fixes
# ==========================================================

# Initialize and configure Git identity
git init
git branch -m main
git config --global user.name "Suleiman Bot"
git config --global user.email "abdulsalamsuleiman100@gmail.com"

# Check and set correct remote URL
git remote -v
git remote set-url origin https://github.com/Suleiman-bot/plane.git

# Push to remote (using PAT instead of password)
git push -u origin main

# Store credentials
git config --global credential.helper cache   # temporary
# or
git config --global credential.helper store   # permanent

# Routine workflow
git status
git add DEVLOG.md
git commit -m "update DEVLOG.md with new entries"
git push

# Pull updates from remote
git pull

# ==========================================================
# [2025-08-19] Docker & Plane Project Operations
# ==========================================================

# Move backend Dockerfile into correct directory
mv dockerfile.node /plane/api/

# Check backend health and endpoints
curl http://localhost:8000/api/health
curl http://localhost:8000/api/tickets
curl http://localhost:8000/api/tickets/12345

# Clean and rebuild Docker environment
docker compose down
docker compose -f docker-compose.override.yml down -v
docker compose -f docker-compose.override.yml up --build

# Create empty .env to suppress warnings
touch /plane/.env

# ==========================================================
# [2025-08-22] Docker Service Integration with Systemd
# ==========================================================

# Reload and restart service
sudo systemctl daemon-reload
sudo systemctl restart ticketing-docker
sudo systemctl status ticketing-docker

# Enable auto-start at boot
sudo systemctl enable ticketing-docker

# Verify directory path used by service
ls -ld /home/kasi/plane   # Found incorrect path → corrected to /plane

# Restart service after path fix
sudo systemctl restart ticketing-docker

# ==========================================================
# [2025-08-23] React Frontend Build & Serve
# ==========================================================

# Build production React app
npm run build

# Install and run local static server
npm install -g serve         # (may require sudo)
npx serve -s build -l 3000   # Run build locally on port 3000

# Test in browser:
# http://localhost:3000
# http://192.168.0.3:3000

# ==========================================================
# [2025-08-25] Caddy Proxy Frontend Access Tests
# ==========================================================

# Access frontend through proxy
# Test URLs after replacing Caddyfile:
#   - http://localhost/frontend/
#   - http://localhost/

# ==========================================================
# [2025-08-19 → 2025-08-23] Git Submodule Operations
# ==========================================================

# Submodule update & sync helper script
# --------------------------------------
# Pull updates for both outer repo and submodule
git pull
cd projects/ticketing-form && git pull && cd ..

# If submodule reference changed:
git status
git add projects/ticketing-form
git commit -m "Updated submodule reference to latest commit"
git push

# Commit and push changes inside submodule
cd projects/ticketing-form
git add .
git commit -m "Update ticketing-form submodule"
git push
cd ../..

# Update submodule reference in parent repo
git add projects/ticketing-form
git commit -m "Updated submodule to latest commit"
git push

# Sync latest commit from remote submodule
git submodule update --remote --merge

# Configure tracked branch for submodule
git fetch
git branch -r
git config -f .gitmodules submodule.projects/ticketing-form.branch main
git submodule update --remote --merge

# Absorb submodule into main repo (remove linkage)
cd projects/ticketing-form
rm -rf .git
cd ../../
git rm --cached -r projects/ticketing-form
git add projects/ticketing-form
git commit -m "Absorbed submodule into main repo as regular files"

# Shortcut alias (custom)
git subup

# ==========================================================
# [2025-08-23] Utility Commands
# ==========================================================

# Copy directory
cp -r /home/user/folder1 /home/user/backup/

# Verbose copy
cp -rv /home/user/folder1 /home/user/backup/

# Copy only newer or missing files
cp -ru /home/user/folder1 /home/user/backup/

# ==========================================================
# [2025-08-27] MongoDB 6.0 Installation Reference
# ==========================================================

# MongoDB Installation Script for Ubuntu 22.04 (Jammy)
# ----------------------------------------------------
curl -fsSL https://pgp.mongodb.com/server-6.0.asc | \
    sudo gpg -o /usr/share/keyrings/mongodb-server-6.0.gpg --dearmor

echo "deb [ arch=amd64,arm64 signed-by=/usr/share/keyrings/mongodb-server-6.0.gpg ] \
https://repo.mongodb.org/apt/ubuntu jammy/mongodb-org/6.0 multiverse" | \
    sudo tee /etc/apt/sources.list.d/mongodb-org-6.0.list

sudo apt update
sudo apt install -y mongodb-org
sudo systemctl start mongod
sudo systemctl enable mongod
systemctl status mongod

# Mongo Shell Quick Reference
mongosh mongodb://localhost:27017
mongosh mongodb://192.168.0.3:27017
show dbs
use plane
show collections
db.tickets.find().pretty()

# ==========================================================
# END OF DEVLOG
# ==========================================================
