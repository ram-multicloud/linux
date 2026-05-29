# Linux Patching Overview

Linux patching is the process of applying security fixes, bug fixes, and feature updates to a Linux system. Regular patching is critical to:

- Protect against security vulnerabilities (CVEs)
- Improve system stability and performance
- Maintain compliance with organizational policies

## Common Package Managers by Distro

| Distro             | Package Manager | Update Command               |
|--------------------|-----------------|------------------------------|
| Ubuntu / Debian    | `apt`           | `sudo apt update && sudo apt upgrade` |
| RHEL / CentOS 7    | `yum`           | `sudo yum update`            |
| RHEL / CentOS 8+   | `dnf`           | `sudo dnf update`            |
| SUSE / openSUSE    | `zypper`        | `sudo zypper update`         |
| Arch Linux         | `pacman`        | `sudo pacman -Syu`           |


## Patching – With Examples

### Step 1: Update the Package Index

Before installing any updates, refresh the list of available packages from all configured repositories.

```bash
sudo apt update
```

### Step 2: Review Available Upgrades

Check which packages have updates available before applying them.

```bash
apt list --upgradable
```

### Step 3: Apply All Available Updates

```bash
sudo apt upgrade -y
```


### Step 4: Full Upgrade (handles dependency changes)

`dist-upgrade` or `full-upgrade` handles cases where packages need to be added or removed to satisfy new dependencies.

```bash
sudo apt full-upgrade -y
```

---

### Step 5: Patch a Specific Package

To update only a specific package (e.g., `curl`):

```bash
sudo apt install --only-upgrade curl
```


### Step 6: Apply Security Patches Only

To apply only security-related updates (using `unattended-upgrades`):

```bash
sudo apt install unattended-upgrades -y
sudo unattended-upgrade --dry-run -v    # Preview security patches
sudo unattended-upgrade -v              # Apply security patches
```


### Step 7: Remove Unused Packages

After patching, clean up packages that are no longer needed.

```bash
sudo apt autoremove -y
sudo apt autoclean
```


### Step 8: Check if a Reboot is Required

Some kernel or core library updates require a system reboot.

```bash
cat /var/run/reboot-required
```

List which packages triggered the reboot requirement:
```bash
cat /var/run/reboot-required.pkgs
```

```bash
sudo reboot
```

### Complete One-Line Patch Command

```bash
sudo apt update && sudo apt full-upgrade -y && sudo apt autoremove -y && sudo apt autoclean
```

---

## 3. Automated Patching

### Enable Unattended Security Updates

```bash
sudo apt install unattended-upgrades -y
sudo dpkg-reconfigure --priority=low unattended-upgrades
```


### Schedule Patching with Cron

Run full patch every Sunday at 2 AM:
```bash
crontab -e
```
Add:
```
0 2 * * 0 sudo apt update && sudo apt full-upgrade -y && sudo apt autoremove -y >> /var/log/auto-patch.log 2>&1
```

---


# WSL on Windows

WSL (Windows Subsystem for Linux) lets you run a Linux environment directly on Windows without a virtual machine.

### Prerequisites

- Windows 10 version 2004+ (Build 19041+) or Windows 11
- Virtualization enabled in BIOS/UEFI

### Method 1: One-Command Install (Recommended)

Open **PowerShell** or **Command Prompt** as Administrator and run:

```powershell
wsl --install
```

This installs WSL 2 with Ubuntu as the default distribution. Restart your PC when prompted.

---

### Method 2: Manual Step-by-Step Install

**Step 1 – Enable WSL feature:**
```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
```

**Step 2 – Enable Virtual Machine Platform:**
```powershell
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

**Step 3 – Restart your PC.**

**Step 4 – Set WSL 2 as default:**
```powershell
wsl --set-default-version 2
```

**Step 5 – Install Ubuntu from Microsoft Store or CLI:**
```powershell
wsl --install -d Ubuntu
```

Other available distros:
```powershell
wsl --list --online
```
```
NAME                            FRIENDLY NAME
Ubuntu                          Ubuntu
Ubuntu-22.04                    Ubuntu 22.04 LTS
Ubuntu-24.04                    Ubuntu 24.04 LTS
Debian                          Debian GNU/Linux
kali-linux                      Kali Linux Rolling
```

---

### Launch WSL

```powershell
wsl
# or
ubuntu
```

On first launch, create a UNIX username and password.

---

### Verify WSL Version

```powershell
wsl --list --verbose
```

```
  NAME            STATE           VERSION
* Ubuntu          Running         2
```

---

### Upgrade a Distro from WSL 1 to WSL 2

```powershell
wsl --set-version Ubuntu 2
```

---

## 5. WSL Post-Setup Configuration

### Update Ubuntu Immediately After Install

```bash
sudo apt update && sudo apt upgrade -y
```

### Install Essential Tools

```bash
sudo apt install -y git curl wget vim build-essential net-tools
```

### Access Windows Files from WSL

Windows drives are mounted under `/mnt/`:

```bash
ls /mnt/c/Users/YourName/
cd /mnt/d/Projects/
```

### Access WSL Files from Windows Explorer

In Windows Explorer address bar, type:
```
\\wsl$\Ubuntu\home\yourusername
```

### Configure WSL Settings (`.wslconfig`)

Create or edit `C:\Users\YourName\.wslconfig` to tune WSL 2 resources:

```ini
[wsl2]
memory=4GB          # Limit RAM usage
processors=2        # Limit CPU cores
swap=2GB
localhostForwarding=true
```

Restart WSL to apply:
```powershell
wsl --shutdown
wsl
```

---

## 6. Patching Ubuntu Inside WSL

Patching inside WSL follows the exact same steps as a native Ubuntu machine.

```bash
# Open WSL terminal and run:
sudo apt update && sudo apt full-upgrade -y && sudo apt autoremove -y
```
