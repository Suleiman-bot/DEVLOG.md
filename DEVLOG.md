# DEVLOG

---

## [2025-08-23]
### Task
Started DEVLOG for documenting troubleshooting steps and fixes.

### Issue
None — setup.

### Commands Tried

### Solution / Fix
Created initial DEVLOG file.

### Notes
This file will track commands, issues, and fixes to avoid repeating mistakes.

## [2025-08-20]
### Command
`VBoxManage modifyvm "testServer" --autostart-enabled on`

### Purpose
Configure a VirtualBox VM to start automatically with the host.

### Context
Attempted to enable autostart for VM "testServer", but the error showed the VM name wasn’t found.

---

## [2025-08-20]
### Command
`VBoxManage list vms`

### Purpose
List all registered VirtualBox VMs with their names and UUIDs.

### Context
Used to confirm the exact VM name before retrying the autostart command.

---

## [2025-08-22]
### Command
sudo systemctl daemon-reload
sudo systemctl restart ticketing-docker
sudo systemctl status ticketing-docker


### Purpose
Reload systemd configs, restart a service, and check its status.

### Context
Applied when testing the `ticketing-docker.service` unit to ensure the Docker ticketing stack was running correctly.

---

## [2025-08-18]
### Command
`git push -u origin main`

### Purpose
Push local commits to the `main` branch on the remote repository.

### Context
Attempted GitHub push but got “Invalid username or token” error because PAT (personal access token) was required instead of password authentication.

---

## [2025-08-18]
### Command
`docker compose -f /home/abdul/plane/docker-compose.override.yml logs -f proxy`

### Purpose
Follow live logs of the `proxy` service defined in a custom Docker Compose override file.

### Context
Used while debugging proxy startup issues and database migration wait state.

[2025-08-20]
Command

VBoxManage modifyvm "testServer" --autostart-enabled on

Purpose

Configure a VirtualBox VM named testServer to start automatically with the host.

Context

First attempt to enable autostart for the VM, but failed because the VM name was not correctly matched.

[2025-08-20]
Command

VBoxManage list vms

Purpose

List all registered VirtualBox VMs with their names and UUIDs.

Context

Used to confirm the exact VM name or UUID to fix the autostart error.

[2025-08-20]
Command

sudo VBoxManage modifyvm "testServer {5f0d8390-a982-4a85-b4a3-e457ec19e1a6}" --autostart-enabled on

Purpose

Attempt to enable autostart for the VM using its full name plus UUID.

Context

Tried after the first command failed, but VirtualBox still couldn’t find the VM with that name.

[2025-08-20]
Command

VBoxManage modifyvm "<exact_vm_name>" --autostart-enabled on

Purpose

Enable autostart for the VM using the correct VM name from VBoxManage list vms.

Context

Suggested correction after confirming that the VM name must match exactly as listed, without UUIDs or extra characters.

[2025-08-20]
Command

VBoxManage setproperty autostartdbpath /etc/vbox

Purpose

Set the global autostart database path for VirtualBox.

Context

Required step to enable the VirtualBox autostart service, since by default autostart is disabled until a DB path is configured.

[2025-08-20]
Command

sudo systemctl enable vboxautostart-service

Purpose

Enable the VirtualBox autostart system service so VMs marked for autostart will launch on host boot.

Context

Part of setting up automatic startup after power outage or host reboot.

[2025-08-20]
Command

sudo systemctl start vboxautostart-service

Purpose

Start the VirtualBox autostart service immediately without waiting for reboot.

Context

Used to activate the autostart feature right away to test if VM comes up automatically.

[2025-08-20]
Command

VBoxManage modifyvm "<exact_vm_name>" --autostop-type savestate

Purpose

Configure how the VM should shut down when the host goes off — here saving the state.

Context

Suggested so the VM state is preserved during host shutdowns/power outages and resumes cleanly on restart.


## [2025-08-23]
### Command
`npm run build`

### Purpose
Runs the React build process (`react-scripts build`) to create an optimized production build.

### Context
Used to generate static frontend files for deployment in `/projects/ticketing-form/build`.

---

## [2025-08-23]
### Command
`npm install -g serve`

### Purpose
Attempts to install the `serve` package globally to serve the React production build.

### Context
Needed a simple HTTP server to host the React build. Failed due to insufficient permissions.

---

## [2025-08-23]
### Command
`npx serve -s build -l 3000`

### Purpose
Uses `npx` to run `serve` without global installation, serving the build folder at port 3000.

### Context
Allowed testing of the React frontend at `http://localhost:3000` and the VM’s IP `192.168.0.3:3000`.

---

## [2025-08-23]
### Command
`which docker-compose`

### Purpose
Checks the location of the `docker-compose` binary.

### Context
Verifying whether the legacy `docker-compose` was installed. It was not found.

---

## [2025-08-23]
### Command
`which docker`

### Purpose
Shows the full path of the `docker` binary.

### Context
Needed the exact binary path when writing the `systemd` service unit. Result: `/usr/bin/docker`.

---

## [2025-08-23]
### Command
`sudo systemctl daemon-reload`

### Purpose
Reloads `systemd` to recognize new or modified unit files.

### Context
Run after creating or editing `ticketing-docker.service`.

---

## [2025-08-23]
### Command
`sudo systemctl enable ticketing-docker`

### Purpose
Enables the `ticketing-docker` service to start automatically at boot.

### Context
Ensures the Docker Compose stack for this project always starts when the VM restarts.

---

## [2025-08-23]
### Command
`sudo systemctl start ticketing-docker`

### Purpose
Starts the `ticketing-docker` service immediately.

### Context
Tested if the Docker Compose stack could be launched manually via systemd.

---

## [2025-08-23]
### Command
`sudo systemctl status ticketing-docker`

### Purpose
Displays the active status, logs, and errors of the `ticketing-docker` service.

### Context
Used multiple times to troubleshoot why the service failed (initially due to a bad `WorkingDirectory`).

---

## [2025-08-23]
### Command
`ls -ld /home/kasi/plane`

### Purpose
Lists details of the `/home/kasi/plane` directory.

### Context
Checked the path referenced in the unit file. Found the directory didn’t exist — the correct path was `/plane`.

---

## [2025-08-23]
### Command
`sudo systemctl restart ticketing-docker`

### Purpose
Restarts the `ticketing-docker` service to apply changes.

### Context
Used after fixing the `WorkingDirectory` in the systemd unit file to `/plane`.

---

## [2025-08-22]
### Command
`docker compose down`

### Purpose
Stops and removes running Docker containers defined in the Compose configuration.

### Context
I ran this to shut down the existing containers for the ticketing system before applying backend/frontend code changes.

---

## [2025-08-22]
### Command
`touch /plane/.env`

### Purpose
Creates an empty `.env` file in the `/plane` directory.

### Context
I ran this to silence Docker Compose warnings about a missing `.env` file when starting/stopping services.

---

## [2025-08-22]
### Command
`docker compose -f docker-compose.override.yml down -v`

### Purpose
Stops and removes containers, networks, volumes, and images defined in the `docker-compose.override.yml` file.

### Context
I ran this to completely clean up the Docker environment for the ticketing system before rebuilding with the updated backend code.

---

## [2025-08-22]
### Command
`docker compose -f docker-compose.override.yml up --build`

### Purpose
Builds images and starts containers defined in the `docker-compose.override.yml` file.

### Context
I ran this to rebuild and restart the ticketing system with the updated backend (`server.js` and `tickets.js`) that included attachment handling and static file serving.

## [2025-08-19]
### Command
`mv dockerfile.node /plane/api/`
### Purpose
Moves the `dockerfile.node` file into the `/plane/api` directory.
### Context
I reorganized my backend project structure so the Node.js Dockerfile sits in the correct API directory.

---

## [2025-08-19]
### Command
`curl http://localhost:8000/api/health`
### Purpose
Sends a test request to the backend health check endpoint.
### Context
I verified that my backend API was running correctly on port 8000.

---

## [2025-08-19]
### Command
`curl http://localhost:8000/api/tickets`
### Purpose
Fetches all tickets from the backend API.
### Context
I tested whether my backend could successfully return ticket data.

---

## [2025-08-19]
### Command
`curl http://localhost:8000/api/tickets/12345`
### Purpose
Fetches a specific ticket from the backend using ticket ID `12345`.
### Context
I tested ticket retrieval by ID from the backend API.

---

## [2025-08-19]
### Command
`git push -u origin main`
### Purpose
Pushes local changes to the remote repository on the `main` branch, and sets the upstream so future `git push` commands default to `origin main`.
### Context
I attempted to push my Plane project to GitHub but got an authentication error because GitHub no longer supports password authentication.

---

## [2025-08-19]
### Command
`git remote -v`
### Purpose
Displays the current remote repository URLs for fetch and push operations.
### Context
I used this to confirm that my local repo was pointing to the correct GitHub remote, and discovered it was pointing to the official `makeplane/plane.git` instead of my personal repo.

---

## [2025-08-19]
### Command
`git remote set-url origin https://github.com/Suleiman-bot/plane.git`
### Purpose
Changes the remote URL for the `origin` remote to point to my own GitHub repository.
### Context
I used this to fix the misconfigured remote so I can push to my own GitHub repo instead of Plane’s official repository.

---

## [2025-08-19]
### Command
`git config --global credential.helper store`
### Purpose
Configures Git to store credentials in plain text in the user’s home directory so I don’t have to re-enter my GitHub token each time.
### Context
I considered this option for saving my GitHub token after switching from password-based authentication.

---

## [2025-08-19]
### Command
`git config --global credential.helper cache`
### Purpose
Configures Git to temporarily cache credentials in memory so I don’t need to re-enter my GitHub token during the session.
### Context
I considered this as a safer alternative to permanently storing my GitHub token in plain text.

## [2025-08-20]
### Command
`ip a`

### Purpose
Displays all network interfaces and their assigned IP addresses.

### Context
I ran this on the host to verify network interfaces and check if the VM was assigned an IP after boot.

---

## [2025-08-20]
### Command
`ip r`

### Purpose
Shows the routing table and the default gateway used by the system.

### Context
I used this on the host to identify the gateway address that the VM should use in its static IP configuration.

---

## [2025-08-20]
### Command
`sudo netplan apply`

### Purpose
Applies the network configuration defined in Netplan YAML files.

### Context
I used this to apply changes made to the VM’s Netplan configuration for static IP assignment.

---

## [2025-08-20]
### Command
`sudo netplan generate`

### Purpose
Validates and generates the backend configuration for Netplan.

### Context
I ran this to check for syntax errors and generate the config before applying it on the VM.

---

## [2025-08-20]
### Command
`sudo apt update`

### Purpose
Updates the package list from Ubuntu repositories.

### Context
I used this before installing OpenSSH to make sure package information was up to date on the VM.

---

## [2025-08-20]
### Command
`sudo apt install openssh-server -y`

### Purpose
Installs the OpenSSH server package to enable SSH access to the VM.

### Context
I ran this on the VM so I could connect to it remotely from the host via SSH.

---

## [2025-08-20]
### Command
`sudo systemctl enable ssh`

### Purpose
Configures the SSH service to start automatically on boot.

### Context
I used this to ensure SSH remains enabled for future reboots of the VM.

---

## [2025-08-20]
### Command
`sudo systemctl start ssh`

### Purpose
Starts the SSH service immediately.

### Context
I ran this after installing OpenSSH server so the VM would accept SSH connections right away.

---

## [2025-08-20]
### Command
`ssh user@192.168.1.50`

### Purpose
Connects to the VM over the network using SSH.

### Context
I used this from the host machine to remotely access the VM after assigning it a static IP and enabling SSH.

## [2025-08-20]
### Command
network:
version: 2
renderer: networkd
ethernets:
enp0s3:
dhcp4: no
addresses: [192.168.1.50/24]
gateway4: 192.168.1.1
nameservers:
addresses: [8.8.8.8, 8.8.4.4]

csharp
Copy
Edit

### Purpose
Defines a static IP configuration for the VM using Netplan.  
- `dhcp4: no` disables DHCP.  
- `addresses` sets a static IP and subnet.  
- `gateway4` sets the default gateway.  
- `nameservers` configures DNS servers.

### Context
I created this YAML configuration on the VM to assign a static IP address instead of relying on DHCP

## [2025-08-19]
### Command
`wsl --install`

### Purpose
Installs Windows Subsystem for Linux (WSL) and sets up the default Linux distribution.

### Context
I ran this to attempt installing WSL directly from PowerShell, but it returned "Access is denied" due to corrupted or missing configuration.

---

## [2025-08-19]
### Command
`Get-FileHash "C:\Windows\System32\wsl.exe" -Algorithm SHA256`

### Purpose
Generates the SHA256 hash of the `wsl.exe` binary.

### Context
I ran this to verify the integrity of the `wsl.exe` executable while troubleshooting access issues with WSL.

---

## [2025-08-19]
### Command
`wsl.exe /repair`

### Purpose
Attempts to repair the WSL installation.

### Context
I ran this to fix potential corruption in the WSL installation, but it still returned "Access is denied."

---

## [2025-08-19]
### Command
`winget install --id Microsoft.WSL`

### Purpose
Uses Windows Package Manager (winget) to install the WSL package from Microsoft.

### Context
I ran this as an alternative method to reinstall WSL, but initially `winget` failed to run properly.

---

## [2025-08-19]
### Command
`sfc /scannow`

### Purpose
Runs the System File Checker to scan and repair corrupted system files.

### Context
I ran this after persistent "Access is denied" errors to fix underlying Windows system file issues that could affect WSL.

---

## [2025-08-19]
### Command
`wsl --status`

### Purpose
Displays the current status of the WSL installation and its configuration.

### Context
I ran this to confirm whether WSL was functioning properly after running SFC, but it still returned "Access is denied."

---

## [2025-08-19]
### Command
`winget --version`

### Purpose
Checks the installed version of Windows Package Manager (winget).

### Context
I ran this to confirm that winget was working correctly after earlier failures.

---

## [2025-08-19]
### Command
`winget install --id Microsoft.WSL --source msstore --accept-package-agreements --accept-source-agreements`

### Purpose
Attempts to install WSL via the Microsoft Store source in winget.

### Context
I ran this to try pulling WSL directly from the Microsoft Store, but no matching package was found.

---

## [2025-08-19]
### Command
`winget source list`

### Purpose
Lists all the available sources configured for winget.

### Context
I ran this to verify which repositories were available to winget after the Microsoft Store install attempt failed.

---

## [2025-08-19]
### Command
`winget install --id Microsoft.WSL --source winget --accept-package-agreements --accept-source-agreements`

### Purpose
Installs WSL via the default `winget` repository.

### Context
I ran this after verifying sources, and this time the installation succeeded, though it initially failed with exit code 1603 before completing successfully.

---

## [2025-08-19]
### Command
`wsl --install -d Ubuntu-22.04`

### Purpose
Installs Ubuntu 22.04 as the default WSL distribution.

### Context
I ran this to set up Ubuntu 22.04 specifically, but the installer defaulted to the latest "Ubuntu" (24.04) instead.

---

## [2025-08-19]
### Command
`wsl -l -v`

### Purpose
Lists installed WSL distributions and shows their version (WSL1 or WSL2).

### Context
I ran this to confirm which distributions were installed and verify that Ubuntu was running on WSL2.

---

## [2025-08-19]
### Command
`wsl -d Ubuntu`

### Purpose
Launches the Ubuntu WSL distribution.

### Context
I ran this to enter the Ubuntu shell after installation to verify that it was functioning correctly.

---

## [2025-08-19]
### Command
`cd ~`

### Purpose
Navigates to the user’s home directory inside Ubuntu.

### Context
I ran this to return to my home directory after logging into Ubuntu WSL.

---

## [2025-08-19]
### Command
`cd ~/plane`

### Purpose
Navigates into the `plane` directory inside the Ubuntu WSL environment.

### Context
I ran this to move into my project folder (`plane`) to begin working on it inside WSL.

## [2025-08-25]
### Command
http://localhost/frontend/

### Purpose
To access the frontend form served through the Caddy proxy container.

### Context
I ran this in my browser to test if the proxy configuration was correctly serving the frontend application after replacing the Caddyfile.


## [2025-08-25]
### Command
http://localhost/

### Purpose
To check if the frontend application was available directly at the root path via the proxy.

### Context
I tested this URL in my browser to confirm whether the form could be accessed without specifying `/frontend/`.


# GIT WORKFLOW REFERENCE

# 1. First-time setup (initialize repository)
git init
git branch -m main   # rename branch to main

# Configure Git identity (only once per machine)
git config --global user.name "Suleiman Bot"
git config --global user.email "abdulsalamsuleiman100@gmail.com"

# Add remote repository
git remote add origin https://github.com/Suleiman-bot/DEVLOG.md.git

# Stage file(s) and commit
git add DEVLOG.md
git commit -m "Add initial DEVLOG.md"

# Push to GitHub
git push -u origin main


# 2. Everyday workflow (after edits)
git status                  # check changes
git add DEVLOG.md           # stage updated file(s)
git commit -m "update DEVLOG.md with new entries"
git push                    # push changes to GitHub


# 3. Pull updates (if edited online or on another machine)
git pull

#to copy folder
cp -r /home/user/folder1 /home/user/backup/

#to copy folder verbose (showing files copied)
cp -rv /home/user/folder1 /home/user/backup/

#to copy wht does not exit already in destination folder
cp -ru /home/user/folder1 /home/user/backup/

#!/bin/bash

# Step 1: Pull changes for the outer repository (main repo)
echo "Pulling changes for the outer repository..."
git pull

# Step 2: Navigate to the submodule directory and pull its changes
echo "Pulling changes for the submodule..."
cd projects/ticketing-form  # Go into the submodule directory
git pull  # Pull latest changes from the submodule
cd ..  # Go back to the outer repository's root

# Step 3: Check if the submodule reference has changed (i.e., it points to a new commit)
echo "Checking if submodule reference has changed..."
git status  # This will show if the submodule reference has changed

# Step 4: If submodule reference has changed, commit the updated reference in the outer repository
if git status | grep -q "modified:   projects/ticketing-form"; then
    echo "Submodule reference has changed. Committing the update..."

    # Stage the updated submodule reference
    git add projects/ticketing-form

    # Commit the change to the outer repo (submodule reference update)
    git commit -m "Updated submodule reference to latest commit"

    # Push changes to the outer repository
    git push
else
    echo "No changes to submodule reference."
fi

# Step 5: Pushing changes from the outer repository (if there were any new commits)
echo "Pushing changes from the outer repository..."
git push

# Optional: If you need to push changes from the submodule too (commit and push in submodule)
echo "Pushing changes in the submodule..."
cd projects/ticketing-form  # Go into submodule directory
git add .  # Stage any changes in the submodule
git commit -m "Your commit message for submodule"  # Commit changes in submodule
git push  # Push changes in the submodule
cd ..  # Go back to the outer repository's root


