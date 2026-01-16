---
layout: post
title: "Linux Basics to Advanced: Complete Tutorial"
date: 2018-01-08 10:00:00 +0000
categories: [linux, operating-system, tutorial, devops]
author: Amal
image: /assets/images/posts/2018-01-15-linux-basics-to-advanced.svg
---



# Linux Basics to Advanced: A Complete Tutorial

## Introduction

Linux is the backbone of modern infrastructure. Whether you're a system administrator, developer, or DevOps engineer, mastering Linux is essential. This guide takes you from basic commands to advanced system administration.

## Part 1: Linux Basics

### What is Linux?

Linux is a free, open-source Unix-like operating system. It powers servers, desktops, mobile devices, and embedded systems worldwide.

### Basic File System Structure

```bash
/          # Root directory
/home      # User home directories
/etc       # System configuration files
/var       # Variable data (logs, caches)
/usr       # User programs and libraries
/bin       # Essential command binaries
/sbin      # System binaries
/lib       # System libraries
/tmp       # Temporary files
/opt       # Optional software packages
```

### Essential Commands

```bash
# Navigation
pwd              # Print working directory
cd /path/to/dir  # Change directory
ls -la           # List files with details
tree             # Display directory tree

# File Operations
touch filename           # Create empty file
cp source dest           # Copy file
mv source dest           # Move/rename file
rm filename              # Delete file
cat filename             # Display file contents
less filename            # View file interactively

# Permissions
chmod 755 filename       # Change file permissions
chown user:group file    # Change ownership
sudo command             # Execute as superuser
```

### User and Group Management

```bash
# User Management
useradd username         # Add new user
userdel username         # Delete user
passwd username          # Change password
id username              # Display user info
whoami                   # Current user

# Group Management
groupadd groupname       # Create group
groupdel groupname       # Delete group
usermod -aG group user   # Add user to group
```

## Part 2: Intermediate Linux

### Working with Processes

```bash
# Process Management
ps aux                   # List all processes
ps aux | grep process    # Find specific process
top                      # Real-time process monitor
htop                     # Interactive process viewer
kill PID                 # Terminate process
kill -9 PID              # Force kill process
bg                       # Run in background
fg                       # Bring to foreground
```

### System Information

```bash
# System Info
uname -a                 # System information
lsb_release -a          # Linux version
hostnamectl             # Hostname info
uptime                  # System uptime
df -h                   # Disk space usage
du -sh *                # Directory sizes
free -h                 # Memory usage
vmstat                  # Virtual memory stats
```

### Network Configuration

```bash
# Network Commands
ifconfig                # Display network interfaces
ip addr show            # Show IP addresses
ping host              # Test connectivity
netstat -tuln          # List open ports
ss -tuln               # Socket statistics
route -n               # Display routing table
traceroute host        # Trace network path
```

### File Permissions Deep Dive

```bash
# Permission Format: rwxrwxrwx
# Owner | Group | Others
# r=4, w=2, x=1

chmod 644 file          # rw-r--r--
chmod 755 file          # rwxr-xr-x
chmod 700 file          # rwx------
chmod -R 755 dir        # Recursive

# Symbolic notation
chmod u+x file          # Add execute for owner
chmod g-w file          # Remove write for group
chmod o+r file          # Add read for others
chmod a+x file          # Add execute for all
```

## Part 3: Advanced Linux

### Text Processing and Filtering

```bash
# grep - Search text patterns
grep "pattern" file
grep -i "pattern" file          # Case-insensitive
grep -r "pattern" /path         # Recursive search
grep -v "pattern" file          # Invert match

# sed - Stream editor
sed 's/old/new/' file           # Replace first occurrence
sed 's/old/new/g' file          # Replace all occurrences
sed -i 's/old/new/g' file       # Edit in place

# awk - Text processing
awk '{print $1}' file           # Print first column
awk -F: '{print $1}' file       # Use colon as delimiter
awk '$3 > 100 {print}' file     # Conditional print
```

### Advanced File Operations

```bash
# File compression and archiving
tar -czf archive.tar.gz dir/    # Create gzip archive
tar -xzf archive.tar.gz         # Extract gzip archive
zip -r archive.zip dir/         # Create zip file
unzip archive.zip               # Extract zip file

# Find with advanced options
find /path -name "*.txt"        # Find by name
find /path -type f -size +1G    # Find large files
find /path -mtime -7            # Modified in last 7 days
find /path -exec command {} \;  # Execute command on results
```

### System Services and Daemons

```bash
# systemd - Modern init system
systemctl start service         # Start service
systemctl stop service          # Stop service
systemctl restart service       # Restart service
systemctl enable service        # Enable on boot
systemctl disable service       # Disable on boot
systemctl status service        # Check status
systemctl list-units --all      # List all units
journalctl -u service -n 50     # View service logs
```

### Package Management

```bash
# Debian/Ubuntu (apt)
apt update                      # Update package list
apt upgrade                     # Upgrade packages
apt install package             # Install package
apt remove package              # Remove package
apt search package              # Search packages

# RedHat/CentOS (yum)
yum install package             # Install package
yum update                      # Update all packages
yum remove package              # Remove package
yum search package              # Search packages
```

### Networking - Advanced Configuration

```bash
# Configure network interfaces
sudo nano /etc/network/interfaces  # Debian/Ubuntu
sudo nano /etc/sysconfig/network-scripts/ifcfg-eth0  # CentOS

# Manual IP configuration
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip addr del 192.168.1.100/24 dev eth0
sudo ip link set eth0 up
sudo ip link set eth0 down

# DNS Configuration
cat /etc/resolv.conf            # View DNS servers
sudo systemctl restart systemd-resolved  # Restart DNS

# Firewall (ufw - Ubuntu, firewalld - CentOS)
sudo ufw enable
sudo ufw allow 22/tcp
sudo ufw deny 80/tcp
sudo firewall-cmd --permanent --add-service=http
```

### Log Management

```bash
# System Logs
tail -f /var/log/syslog         # Follow system log
tail -n 100 /var/log/auth.log   # Last 100 lines
journalctl -f                   # Follow systemd journal
journalctl --since "2 hours ago"  # Logs from last 2 hours
journalctl -u service_name      # Specific service logs
```

### Performance Tuning

```bash
# CPU and Memory
nproc                           # Number of processors
cat /proc/cpuinfo               # CPU details
cat /proc/meminfo               # Memory details
watch -n 1 'free -h'            # Monitor memory every second

# Disk I/O
iostat -x 1                     # I/O statistics
iotop                           # I/O usage by process
lsblk                           # Block devices
parted -l                       # Partition info
```

### Advanced Shell Scripting

```bash
#!/bin/bash

# Error handling
set -e          # Exit on error
set -u          # Exit on undefined variable
set -o pipefail # Exit if pipe fails

# Logging function
log() {
    echo "[$(date +'%Y-%m-%d %H:%M:%S')] $1"
}

# Check if root
if [[ $EUID -ne 0 ]]; then
    log "This script must be run as root"
    exit 1
fi

# Array operations
declare -a array=("item1" "item2" "item3")
for item in "${array[@]}"; do
    log "Processing: $item"
done
```

## Summary

You now have a solid foundation in Linux from basics to advanced concepts. Key takeaways:

- **Basics**: Commands, file systems, permissions
- **Intermediate**: Processes, networking, package management
- **Advanced**: Performance tuning, security, automation

Continue practicing these concepts in a Linux environment to master them!
