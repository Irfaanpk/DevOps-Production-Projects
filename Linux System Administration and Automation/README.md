# Linux System Administration and Automation

## Project: Automated Production Linux Web Server

## 1. Project Overview

This project focuses on building, securing, managing, monitoring, and automating a production-style Linux web server environment.

Starting with a fresh Ubuntu Linux server, the project progressively implements:

* Linux user and group management
* SSH administration and hardening
* Package management
* Linux filesystem management
* LVM storage
* Linux networking
* Host firewall
* Nginx web server
* Application deployment
* systemd service management
* Process management
* Log management
* Bash automation
* Scheduled tasks
* System health monitoring
* Backup and restore
* Linux security auditing
* Failure simulation
* Troubleshooting and recovery

The final objective is to create a Linux server that can be **configured, secured, monitored, maintained, backed up, and recovered using automation**.

---

# 2. Architecture

```text
                         Administrator
                              |
                              | SSH
                              v
                    +---------------------+
                    |     Ubuntu Server   |
                    |                     |
                    |  SSH                |
                    |  Users & Groups     |
                    |  Sudo               |
                    |  UFW Firewall       |
                    |  systemd            |
                    |  Bash Automation    |
                    +----------+----------+
                               |
                               v
                         +-----------+
                         |   Nginx   |
                         | :80/:443  |
                         +-----+-----+
                               |
                               v
                       +---------------+
                       | Web Application|
                       |    :8080       |
                       +-------+-------+
                               |
              +----------------+----------------+
              |                |                |
              v                v                v
         System Logs      Application       Backup
         journald         Logs              Storage
              |                |                |
              +----------------+----------------+
                               |
                               v
                      Automation Scripts
                               |
             +-----------------+-----------------+
             |                 |                 |
             v                 v                 v
        Health Check       Backup Script    Security Audit
```

---

# 3. Architecture Explanation

### Administrator

The administrator connects to the Linux server using SSH.

```text
Administrator → SSH → Ubuntu Server
```

SSH will later be hardened to use secure key-based authentication.

### Ubuntu Server

The Ubuntu server is the central component of the project.

It will contain:

* Users
* Groups
* Application
* Nginx
* systemd services
* Firewall
* Logs
* Storage
* Automation scripts

### Nginx

Nginx acts as the web server and reverse proxy.

```text
Client
   |
   v
Nginx
   |
   v
Application
```

### Application

A simple web application will run as a dedicated non-root Linux user.

The application will be managed by systemd.

### systemd

systemd will manage the application lifecycle.

```text
Application
     |
     v
systemd
     |
     +-- Start
     +-- Stop
     +-- Restart
     +-- Enable at boot
     +-- Automatic recovery
```

### UFW

UFW provides host-level firewall protection.

Only required ports will be exposed.

### Logs

Linux and application logs will be collected and managed using:

* journald
* Nginx logs
* application logs
* logrotate

### Automation

Bash scripts will automate:

* Health checks
* Disk monitoring
* Process monitoring
* Log analysis
* Security audits
* Backup
* Restore

---

# 4. Project Objectives

By completing this project, you will be able to:

* Deploy and configure a Linux server
* Manage Linux users and groups
* Configure sudo permissions
* Secure SSH
* Manage Linux packages
* Configure Linux networking
* Manage filesystems and storage
* Configure LVM
* Configure a Linux firewall
* Deploy Nginx
* Deploy a web application
* Create systemd services
* Automate service recovery
* Analyze Linux logs
* Automate Linux administration using Bash
* Schedule administrative tasks
* Implement backups
* Perform restoration
* Perform Linux security audits
* Troubleshoot common Linux failures

---

# 5. Lab Environment

## Recommended Environment

| Component       | Requirement             |
| --------------- | ----------------------- |
| OS              | Ubuntu Server 24.04 LTS |
| CPU             | 2 vCPU                  |
| RAM             | 4 GB                    |
| OS Disk         | 30 GB                   |
| Additional Disk | 10–20 GB                |
| Network         | Internet connectivity   |
| Access          | SSH                     |
| User            | sudo-enabled user       |

You can run the lab on:

* AWS EC2
* Azure VM
* GCP Compute Engine
* VMware
* VirtualBox
* Hyper-V

For this project, the actual Linux administration remains the same regardless of where the VM is hosted.

---

# 6. Prerequisites

You should have basic knowledge of:

* Linux commands
* IP addresses
* TCP/UDP ports
* SSH
* DNS
* HTTP/HTTPS
* Basic Bash
* Basic networking

---

# 7. Initial Server Setup

Connect to your Ubuntu server:

```bash
ssh username@SERVER_IP
```

Verify the operating system:

```bash
cat /etc/os-release
```

Check kernel:

```bash
uname -r
```

Check hostname:

```bash
hostname
```

Check CPU:

```bash
lscpu
```

Check memory:

```bash
free -h
```

Check storage:

```bash
lsblk
```

Check filesystem:

```bash
df -h
```

Check IP address:

```bash
ip addr
```

Check routing:

```bash
ip route
```

---

# 8. Update the Server

Update package repositories:

```bash
sudo apt update
```

Upgrade installed packages:

```bash
sudo apt upgrade -y
```

Install essential administration tools:

```bash
sudo apt install -y \
curl \
wget \
git \
vim \
nano \
htop \
tree \
jq \
unzip \
zip \
rsync \
net-tools \
dnsutils \
lsof \
ncdu \
netcat-openbsd
```

Verify:

```bash
git --version
curl --version
htop --version
```

---

# 9. Configure Hostname

Set a meaningful hostname:

```bash
sudo hostnamectl set-hostname linux-web-01
```

Verify:

```bash
hostnamectl
```

Check:

```bash
hostname
```

---

# 10. Configure Time Synchronization

Check current configuration:

```bash
timedatectl
```

Enable NTP:

```bash
sudo timedatectl set-ntp true
```

Verify:

```bash
timedatectl status
```

Correct system time is important for:

* Authentication
* Logs
* Troubleshooting
* Certificates
* Scheduled jobs

---

# 11. Linux User and Group Management

## Create an Administration Group

```bash
sudo groupadd linuxadmins
```

Create a DevOps administrator:

```bash
sudo adduser devops
```

Add the user to the group:

```bash
sudo usermod -aG linuxadmins devops
```

Add sudo privileges:

```bash
sudo usermod -aG sudo devops
```

Verify:

```bash
id devops
```

---

# 12. Create Application User

Create a system user for the application:

```bash
sudo useradd \
--system \
--create-home \
--shell /usr/sbin/nologin \
appuser
```

Verify:

```bash
id appuser
```

The application should run as:

```text
appuser
```

and **not as root**.

---

# 13. Configure Application Directories

Create the application structure:

```bash
sudo mkdir -p /opt/webapp
sudo mkdir -p /opt/webapp/bin
sudo mkdir -p /opt/webapp/config
sudo mkdir -p /opt/webapp/data
sudo mkdir -p /var/log/webapp
```

Change ownership:

```bash
sudo chown -R appuser:appuser /opt/webapp
sudo chown -R appuser:appuser /var/log/webapp
```

Verify:

```bash
ls -la /opt/webapp
```

---

# 14. Configure SSH Key Authentication

On your local machine:

```bash
ssh-keygen -t ed25519
```

Copy the public key to the server:

```bash
ssh-copy-id devops@SERVER_IP
```

Test:

```bash
ssh devops@SERVER_IP
```

Verify:

```bash
whoami
```

Expected:

```text
devops
```

---

# 15. SSH Hardening

Create a backup of the SSH configuration:

```bash
sudo cp /etc/ssh/sshd_config \
/etc/ssh/sshd_config.backup
```

Open configuration:

```bash
sudo vim /etc/ssh/sshd_config
```

Configure appropriate security settings:

```text
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
```

Validate configuration:

```bash
sudo sshd -t
```

If there is no output, the syntax is valid.

Restart SSH:

```bash
sudo systemctl restart ssh
```

**Important:** Keep your existing SSH session open and test a second SSH connection before closing it.

---

# 16. Configure UFW Firewall

Install UFW:

```bash
sudo apt install -y ufw
```

Set default policies:

```bash
sudo ufw default deny incoming
```

```bash
sudo ufw default allow outgoing
```

Allow SSH:

```bash
sudo ufw allow 22/tcp
```

Enable firewall:

```bash
sudo ufw enable
```

Check:

```bash
sudo ufw status verbose
```

Expected:

```text
22/tcp    ALLOW
```

---

# 17. Install Nginx

Install:

```bash
sudo apt install -y nginx
```

Check service:

```bash
sudo systemctl status nginx
```

Enable at boot:

```bash
sudo systemctl enable nginx
```

Start:

```bash
sudo systemctl start nginx
```

Verify:

```bash
curl http://localhost
```

Check listening ports:

```bash
sudo ss -tulpn
```

Allow HTTP:

```bash
sudo ufw allow 80/tcp
```

Allow HTTPS:

```bash
sudo ufw allow 443/tcp
```

---

# 18. Deploy the Web Application

Create the application:

```bash
sudo -u appuser mkdir -p /opt/webapp
```

Create a simple application:

```bash
sudo -u appuser vim /opt/webapp/app.py
```

The application should expose:

```text
/
```

and:

```text
/health
```

The health endpoint should return:

```text
OK
```

Install the required application runtime.

For a simple Python implementation:

```bash
sudo apt install -y python3 python3-venv
```

Create a virtual environment:

```bash
sudo -u appuser python3 -m venv /opt/webapp/venv
```

Install Flask:

```bash
sudo -u appuser /opt/webapp/venv/bin/pip install flask
```

---

# 19. Create systemd Service

Create:

```bash
sudo vim /etc/systemd/system/webapp.service
```

The service should define:

```text
Description
User
WorkingDirectory
Environment
ExecStart
Restart
RestartSec
```

Reload systemd:

```bash
sudo systemctl daemon-reload
```

Start:

```bash
sudo systemctl start webapp
```

Enable at boot:

```bash
sudo systemctl enable webapp
```

Check:

```bash
sudo systemctl status webapp
```

---

# 20. Test Application

Check application port:

```bash
sudo ss -tulpn
```

Test locally:

```bash
curl http://localhost:8080
```

Test health endpoint:

```bash
curl http://localhost:8080/health
```

Expected:

```text
OK
```

---

# 21. Configure Nginx Reverse Proxy

Configure Nginx:

```bash
sudo vim /etc/nginx/sites-available/webapp
```

Traffic flow:

```text
Client
  |
  | HTTP/HTTPS
  v
Nginx :80/:443
  |
  | Reverse Proxy
  v
Application :8080
```

Enable configuration:

```bash
sudo ln -s \
/etc/nginx/sites-available/webapp \
/etc/nginx/sites-enabled/webapp
```

Remove default configuration if required:

```bash
sudo rm /etc/nginx/sites-enabled/default
```

Test:

```bash
sudo nginx -t
```

Reload:

```bash
sudo systemctl reload nginx
```

Test:

```bash
curl http://localhost
```

---

# 22. Implement Application Logging

Create:

```bash
sudo mkdir -p /var/log/webapp
```

Configure the application to write logs to:

```text
/var/log/webapp/application.log
```

Set ownership:

```bash
sudo chown -R appuser:appuser /var/log/webapp
```

Monitor logs:

```bash
tail -f /var/log/webapp/application.log
```

---

# 23. Configure Log Rotation

Install:

```bash
sudo apt install -y logrotate
```

Create:

```bash
sudo vim /etc/logrotate.d/webapp
```

Configure rotation based on:

* Daily rotation
* Compression
* Retention
* Missing log handling

Test:

```bash
sudo logrotate -d /etc/logrotate.d/webapp
```

---

# 24. Linux Process Management

List processes:

```bash
ps aux
```

Interactive monitoring:

```bash
htop
```

Find application:

```bash
pgrep -af python
```

Find Nginx:

```bash
pgrep -af nginx
```

Check process tree:

```bash
pstree
```

Check resource consumption:

```bash
top
```

---

# 25. Linux Storage Management

Check disks:

```bash
lsblk
```

Check filesystem:

```bash
df -h
```

Check inode usage:

```bash
df -i
```

Check disk usage:

```bash
du -sh /var/*
```

Find large directories:

```bash
sudo du -xhd1 / | sort -h
```

---

# 26. Implement LVM

If an additional disk is available:

Check:

```bash
lsblk
```

Identify the additional disk.

Create physical volume:

```bash
sudo pvcreate /dev/sdb
```

Create volume group:

```bash
sudo vgcreate webapp-vg /dev/sdb
```

Create logical volume:

```bash
sudo lvcreate -L 8G -n webapp-lv webapp-vg
```

Create filesystem:

```bash
sudo mkfs.ext4 /dev/webapp-vg/webapp-lv
```

Create mount point:

```bash
sudo mkdir -p /data/webapp
```

Mount:

```bash
sudo mount /dev/webapp-vg/webapp-lv /data/webapp
```

Verify:

```bash
df -h
```

---

# 27. Configure Persistent Mount

Get UUID:

```bash
sudo blkid /dev/webapp-vg/webapp-lv
```

Edit:

```bash
sudo vim /etc/fstab
```

Add the appropriate UUID entry.

Test:

```bash
sudo mount -a
```

Verify:

```bash
df -h
```

---

# 28. Linux Networking Administration

Check interfaces:

```bash
ip addr
```

Check routes:

```bash
ip route
```

Check DNS:

```bash
resolvectl status
```

Test DNS:

```bash
dig google.com
```

Test connectivity:

```bash
ping -c 4 8.8.8.8
```

Test HTTP:

```bash
curl -I https://google.com
```

Check listening ports:

```bash
sudo ss -tulpn
```

Check a specific port:

```bash
sudo ss -lntp | grep 8080
```

---

# 29. Create Network Troubleshooting Script

Create:

```bash
sudo vim /opt/scripts/network-check.sh
```

The script should check:

```text
Network interface
Default gateway
DNS
Internet connectivity
Application port
Nginx port
Application endpoint
```

Run:

```bash
sudo /opt/scripts/network-check.sh
```

---

# 30. Create Server Health Check

Create:

```bash
sudo vim /opt/scripts/health-check.sh
```

The script should validate:

```text
CPU
Memory
Disk
Inodes
Network
DNS
Nginx
Application
systemd
HTTP endpoint
```

Make executable:

```bash
sudo chmod +x /opt/scripts/health-check.sh
```

Run:

```bash
sudo /opt/scripts/health-check.sh
```

---

# 31. Create Disk Monitoring Script

Create:

```bash
sudo vim /opt/scripts/disk-monitor.sh
```

Check:

```bash
df -h
df -i
```

Implement warning and critical thresholds.

Run:

```bash
sudo /opt/scripts/disk-monitor.sh
```

---

# 32. Create Process Monitoring Script

Create:

```bash
sudo vim /opt/scripts/process-monitor.sh
```

The script should check:

* Application process
* Nginx
* High CPU processes
* High memory processes
* Zombie processes

Run:

```bash
sudo /opt/scripts/process-monitor.sh
```

---

# 33. Linux Log Analysis

View system logs:

```bash
sudo journalctl
```

View recent logs:

```bash
sudo journalctl -n 100
```

Follow logs:

```bash
sudo journalctl -f
```

View Nginx errors:

```bash
sudo tail -f /var/log/nginx/error.log
```

View access logs:

```bash
sudo tail -f /var/log/nginx/access.log
```

View SSH logs:

```bash
sudo journalctl -u ssh
```

---

# 34. Create Log Analysis Automation

Create:

```bash
sudo vim /opt/scripts/log-analyzer.sh
```

Analyze:

```text
Failed SSH attempts
Nginx 4xx
Nginx 5xx
Application errors
Service failures
Authentication failures
```

Run:

```bash
sudo /opt/scripts/log-analyzer.sh
```

---

# 35. Implement Backup

Create:

```bash
sudo vim /opt/scripts/backup.sh
```

Back up:

```text
/opt/webapp
/etc/nginx
/etc/systemd/system/webapp.service
/opt/scripts
```

Create compressed backup:

```bash
sudo tar -czf \
/opt/backups/webapp-$(date +%F).tar.gz \
/opt/webapp \
/etc/nginx \
/etc/systemd/system/webapp.service \
/opt/scripts
```

Verify:

```bash
ls -lh /opt/backups
```

---

# 36. Implement Restore

Create:

```bash
sudo vim /opt/scripts/restore.sh
```

List backup:

```bash
ls -lh /opt/backups
```

Extract:

```bash
sudo tar -xzf /opt/backups/BACKUP_FILE.tar.gz -C /
```

Restart services:

```bash
sudo systemctl restart webapp
sudo systemctl restart nginx
```

Run health check:

```bash
sudo /opt/scripts/health-check.sh
```

---

# 37. Schedule Backups

Create a systemd timer or cron job.

For cron:

```bash
sudo crontab -e
```

Example:

```text
0 2 * * * /opt/scripts/backup.sh
```

This runs the backup every day at 2 AM.

Check cron:

```bash
sudo systemctl status cron
```

---

# 38. Linux Security Audit

Create:

```bash
sudo vim /opt/scripts/security-audit.sh
```

Check:

```text
SSH configuration
Root login
Password authentication
Firewall
Open ports
Users
Sudo users
Running services
File permissions
Failed authentication attempts
Package updates
```

Run:

```bash
sudo /opt/scripts/security-audit.sh
```

---

# 39. Failure Testing

Now deliberately introduce failures.

## Test 1 — Application Failure

```bash
sudo systemctl stop webapp
```

Check:

```bash
sudo systemctl status webapp
```

Verify systemd recovery if restart policy is configured.

---

## Test 2 — Nginx Failure

```bash
sudo systemctl stop nginx
```

Run:

```bash
sudo /opt/scripts/health-check.sh
```

The health check should identify the failure.

---

## Test 3 — Disk Usage

Create temporary test data:

```bash
sudo fallocate -l 1G /data/webapp/test-file
```

Check:

```bash
df -h
```

Remove:

```bash
sudo rm /data/webapp/test-file
```

---

## Test 4 — Firewall Failure

Temporarily block the application port and verify that the health check identifies the connectivity problem.

Restore the rule after testing.

---

## Test 5 — Configuration Failure

Modify an Nginx configuration incorrectly.

Test:

```bash
sudo nginx -t
```

Fix the configuration and reload Nginx.

---

# 40. Troubleshooting Workflow

When something fails, follow this order:

```text
1. Check service
       |
       v
2. Check process
       |
       v
3. Check listening port
       |
       v
4. Check firewall
       |
       v
5. Check logs
       |
       v
6. Check configuration
       |
       v
7. Test locally
       |
       v
8. Test remotely
```

Useful commands:

```bash
systemctl status SERVICE
journalctl -u SERVICE
ps aux
ss -tulpn
ip addr
ip route
df -h
free -h
top
curl
dig
ufw status
```

---

# 41. Automate the Entire Server Setup

After manually completing the lab, convert the configuration into reusable Bash scripts.

Create:

```text
scripts/
├── 01-system-bootstrap.sh
├── 02-user-setup.sh
├── 03-ssh-hardening.sh
├── 04-firewall-setup.sh
├── 05-nginx-install.sh
├── 06-application-deploy.sh
├── 07-systemd-setup.sh
├── 08-monitoring.sh
├── 09-backup.sh
└── 10-security-audit.sh
```

The scripts should be:

* Repeatable
* Documented
* Error-aware
* Idempotent where practical
* Safe to execute on a fresh server

---

# 42. Create Master Bootstrap Script

Create:

```bash
vim bootstrap.sh
```

The workflow should be:

```text
Fresh Ubuntu Server
        |
        v
System Bootstrap
        |
        v
User Configuration
        |
        v
SSH Hardening
        |
        v
Firewall
        |
        v
Nginx
        |
        v
Application
        |
        v
systemd
        |
        v
Monitoring Scripts
        |
        v
Backup
        |
        v
Security Audit
        |
        v
Health Check
```

Run:

```bash
sudo ./bootstrap.sh
```

---

# 43. Final Validation

Run the following checks.

### System

```bash
hostnamectl
uptime
free -h
df -h
```

### Network

```bash
ip addr
ip route
ss -tulpn
```

### Security

```bash
sudo ufw status
sudo sshd -t
```

### Services

```bash
systemctl status nginx
systemctl status webapp
```

### Application

```bash
curl http://localhost
curl http://localhost/health
```

### Logs

```bash
journalctl -u webapp -n 50
```

### Automation

```bash
sudo /opt/scripts/health-check.sh
sudo /opt/scripts/security-audit.sh
```

### Backup

```bash
ls -lh /opt/backups
```

---

# Expected Final Result

At the end of the lab, you should have:

```text
Fresh Ubuntu Server
        |
        v
Configured Users & Permissions
        |
        v
Hardened SSH
        |
        v
Configured Firewall
        |
        v
Configured Storage
        |
        v
Nginx Web Server
        |
        v
Production-style Application
        |
        v
systemd Service
        |
        v
Automated Health Checks
        |
        v
Log Management
        |
        v
Backup & Restore
        |
        v
Security Audit
        |
        v
Bash Automation
        |
        v
Failure Testing
        |
        v
Automated Recovery
```
