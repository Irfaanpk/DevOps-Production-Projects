# Linux System Administration and Automation

## 📖 Overview

This project focuses on **Linux system administration and automation for DevOps environments**.

Linux is the foundation of most modern DevOps infrastructure, cloud platforms, containers, Kubernetes nodes, CI/CD servers, and production workloads. This project covers the administration, automation, monitoring, troubleshooting, and hardening techniques required to operate Linux systems in real-world environments.

The goal is not simply to learn Linux commands, but to build practical experience managing Linux servers as a **DevOps engineer**.

---

## 🎯 Project Objectives

By completing this project, you will learn how to:

- Manage Linux users, groups, permissions, and privileges
- Configure and manage system services
- Work with processes and system resources
- Manage Linux networking
- Configure SSH securely
- Manage disks, partitions, filesystems, and mounts
- Automate administrative tasks using Bash
- Schedule recurring tasks using Cron and systemd timers
- Monitor CPU, memory, disk, and network utilization
- Troubleshoot Linux system and application issues
- Configure firewalls and basic system security
- Automate server configuration and maintenance
- Build production-style Linux administration workflows

---

## 🏗️ Project Architecture

```text
                    ┌──────────────────────┐
                    │      DevOps User     │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │      SSH Access      │
                    └──────────┬───────────┘
                               │
                               ▼
             ┌─────────────────────────────────┐
             │          Linux Server           │
             │                                 │
             │  ┌───────────────────────────┐  │
             │  │ Users & Permissions       │  │
             │  ├───────────────────────────┤  │
             │  │ Processes & Services      │  │
             │  ├───────────────────────────┤  │
             │  │ Networking & Firewall     │  │
             │  ├───────────────────────────┤  │
             │  │ Storage & Filesystems     │  │
             │  ├───────────────────────────┤  │
             │  │ Monitoring                │  │
             │  └───────────────────────────┘  │
             │                                 │
             │        Bash Automation          │
             └─────────────────────────────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ Monitoring / Alerts  │
                    └──────────────────────┘
````

---

## 🛠️ Technologies and Tools

| Technology     | Purpose                                    |
| -------------- | ------------------------------------------ |
| Linux          | Operating system and server administration |
| Ubuntu         | Primary Linux distribution                 |
| Bash           | Automation and scripting                   |
| SSH            | Secure remote administration               |
| systemd        | Service and system management              |
| Cron           | Scheduled task automation                  |
| Git            | Script and configuration version control   |
| UFW            | Host-based firewall                        |
| LVM            | Storage management                         |
| NFS            | Network file sharing                       |
| `journalctl`   | System and service log analysis            |
| `top` / `htop` | Resource monitoring                        |
| `ss`           | Network connection analysis                |
| `curl`         | Application and HTTP troubleshooting       |

---

# 📚 Project Sections

## 1. Linux System Fundamentals

Understand the Linux filesystem, shell environment, commands, processes, and system architecture.

### Topics

* Linux architecture
* Linux distributions
* Filesystem hierarchy
* `/etc`
* `/var`
* `/home`
* `/opt`
* `/tmp`
* `/usr`
* `/proc`
* `/sys`
* Environment variables
* Shells
* Standard input/output/error
* Pipes
* Redirection
* Command chaining

---

## 2. User and Permission Management

Manage users, groups, ownership, and access permissions.

### Topics

* User creation
* Group management
* User modification
* Password management
* `sudo`
* File ownership
* Linux permissions
* `chmod`
* `chown`
* `chgrp`
* Special permissions
* SUID
* SGID
* Sticky bit
* ACLs
* Least-privilege access

### Example

```bash
# Create a user
sudo useradd -m devops

# Create a group
sudo groupadd developers

# Add user to group
sudo usermod -aG developers devops

# Check user groups
groups devops

# Change ownership
sudo chown devops:developers application.log

# Change permissions
sudo chmod 640 application.log
```

---

## 3. Process Management

Learn how to inspect, control, and troubleshoot Linux processes.

### Topics

* Process IDs
* Parent and child processes
* Foreground and background processes
* Process states
* Signals
* `ps`
* `top`
* `htop`
* `kill`
* `pkill`
* `nice`
* `renice`
* Zombie processes
* Resource consumption

### Example

```bash
# List processes
ps aux

# Monitor processes
top

# Find a process
ps aux | grep nginx

# Terminate a process
kill <PID>

# Force terminate
kill -9 <PID>
```

---

## 4. Systemd and Service Management

Manage Linux services using systemd.

### Topics

* systemd architecture
* Units
* Services
* Targets
* Service dependencies
* Starting services
* Stopping services
* Restarting services
* Enabling services
* Service logs
* Failed services

### Example

```bash
# Check service status
sudo systemctl status nginx

# Start service
sudo systemctl start nginx

# Stop service
sudo systemctl stop nginx

# Restart service
sudo systemctl restart nginx

# Enable at boot
sudo systemctl enable nginx

# View service logs
sudo journalctl -u nginx
```

---

## 5. Linux Networking

Understand and troubleshoot Linux networking in cloud and DevOps environments.

### Topics

* IPv4
* IPv6
* Network interfaces
* IP addressing
* Routing
* Default gateway
* DNS
* Ports
* TCP
* UDP
* Network sockets
* SSH
* Network troubleshooting

### Useful Commands

```bash
ip addr

ip route

ip link

ss -tulpn

ping 8.8.8.8

curl https://example.com

dig example.com

nslookup example.com

traceroute example.com
```

---

## 6. SSH Administration

Configure secure remote administration of Linux servers.

### Topics

* SSH architecture
* SSH keys
* Public and private keys
* Password authentication
* Key-based authentication
* `authorized_keys`
* SSH configuration
* SSH hardening
* Port configuration
* Root login restrictions
* SSH troubleshooting

### Example

```bash
# Generate SSH key
ssh-keygen -t ed25519

# Connect to server
ssh user@server-ip

# Copy public key
ssh-copy-id user@server-ip
```

---

## 7. Storage and Filesystem Management

Manage Linux storage used by applications and infrastructure.

### Topics

* Disks
* Partitions
* Filesystems
* Mount points
* `/etc/fstab`
* LVM
* Logical volumes
* Volume groups
* Disk utilization
* Inodes
* NFS
* Storage troubleshooting

### Example

```bash
# List disks
lsblk

# Check filesystem usage
df -h

# Check inode usage
df -i

# Check directory size
du -sh /var/log

# Show mounted filesystems
mount
```

---

## 8. Log Management and Troubleshooting

Learn how to investigate system and application failures.

### Topics

* System logs
* Application logs
* `journalctl`
* `/var/log`
* Log rotation
* Error analysis
* Service failures
* Disk-related failures
* Network failures
* Application troubleshooting

### Example

```bash
# View system logs
journalctl

# View logs from current boot
journalctl -b

# View service logs
journalctl -u nginx

# Follow logs in real time
journalctl -u nginx -f
```

---

## 9. Linux Security and Hardening

Implement basic Linux security practices for production servers.

### Topics

* Least privilege
* SSH hardening
* User management
* Password policies
* File permissions
* Firewall configuration
* UFW
* Security updates
* Service minimization
* Audit logs
* Secure configuration

### Example

```bash
# Enable firewall
sudo ufw enable

# Allow SSH
sudo ufw allow 22/tcp

# Allow HTTP
sudo ufw allow 80/tcp

# Check firewall
sudo ufw status
```

---

## 10. Bash Automation

Automate repetitive administration tasks using Bash scripting.

### Topics

* Variables
* Conditions
* Loops
* Functions
* Arguments
* Exit codes
* Command substitution
* Arrays
* Error handling
* Logging
* Automation scripts

### Example

```bash
#!/bin/bash

LOG_FILE="/var/log/system-check.log"

echo "System health check started"

echo "Hostname: $(hostname)"
echo "Uptime: $(uptime)"
echo "Disk Usage:"
df -h

echo "Memory Usage:"
free -h

echo "System health check completed"
```

---

## 11. Scheduled Automation

Automate recurring administrative operations.

### Topics

* Cron
* Crontab
* systemd timers
* Scheduled backups
* Log cleanup
* Health checks
* Automated reports
* Maintenance tasks

### Example

```bash
# Edit cron jobs
crontab -e

# Run script every day at midnight
0 0 * * * /opt/scripts/system-health.sh
```

---

# 🚀 Advanced DevOps Lab

Build a complete automated Linux server management system.

## Scenario

You are responsible for managing a Linux application server.

The server should:

1. Create a dedicated application user
2. Configure SSH key-based authentication
3. Install required packages
4. Deploy an application
5. Configure a systemd service
6. Configure firewall rules
7. Monitor system resources
8. Collect application logs
9. Run scheduled health checks
10. Automatically generate health reports
11. Store configuration in Git
12. Automate the complete setup using Bash

---

## 🔄 Automation Flow

```text
Git Repository
      │
      ▼
Bash Automation Script
      │
      ├── Create Users
      ├── Install Packages
      ├── Configure SSH
      ├── Configure Firewall
      ├── Configure Application
      ├── Configure systemd
      ├── Configure Logging
      └── Configure Monitoring
              │
              ▼
        Linux Server
              │
              ▼
      Scheduled Health Check
              │
              ▼
        Logs / Reports
```

---

# 🧪 Troubleshooting Scenarios

The project should also include real-world troubleshooting exercises.

### Scenario 1 — High CPU Usage

Investigate a server experiencing high CPU utilization.

```bash
top
ps aux --sort=-%cpu | head
```

Identify the process and determine the root cause.

---

### Scenario 2 — Disk Full

A production application stops writing logs because the disk is full.

```bash
df -h
du -sh /*
du -sh /var/log/*
```

Identify large files and clean the system safely.

---

### Scenario 3 — Service Failure

An application service fails to start.

```bash
systemctl status application
journalctl -u application
```

Identify the configuration or dependency problem.

---

### Scenario 4 — Port Not Reachable

An application is running but users cannot access it.

Investigate:

```bash
ss -tulpn
sudo ufw status
ip route
curl localhost:<PORT>
```

Determine whether the issue is application, firewall, routing, or networking related.

---

### Scenario 5 — SSH Connection Failure

A DevOps engineer cannot connect to the server.

Investigate:

```bash
systemctl status ssh
ss -tulpn | grep :22
journalctl -u ssh
```

Check SSH configuration, firewall rules, authentication, and network connectivity.

---

# 📊 Monitoring Commands

Useful commands for Linux server monitoring:

```bash
uptime

free -h

df -h

df -i

top

htop

vmstat

iostat

sar

ss -s

ps aux
```

---

# 🔐 Security Checklist

* [ ] Disable unnecessary services
* [ ] Use SSH key authentication
* [ ] Restrict root login
* [ ] Apply least-privilege permissions
* [ ] Configure firewall rules
* [ ] Keep packages updated
* [ ] Monitor authentication logs
* [ ] Protect sensitive configuration files
* [ ] Remove unused accounts
* [ ] Monitor open ports

---

# 📁 Suggested Project Structure

```text
01-Linux-System-Administration-and-Automation/
│
├── README.md
│
├── scripts/
│   ├── system-health.sh
│   ├── disk-monitor.sh
│   ├── backup.sh
│   └── service-monitor.sh
│
├── configs/
│   ├── ssh/
│   ├── systemd/
│   └── ufw/
│
├── labs/
│   ├── user-management/
│   ├── process-management/
│   ├── networking/
│   ├── storage/
│   ├── troubleshooting/
│   └── security/
│
├── screenshots/
│
└── docs/
    ├── troubleshooting.md
    └── security.md
```

---

# 🎯 Expected Outcome

After completing this project, you should be able to confidently administer Linux servers used for:

* CI/CD platforms
* Docker hosts
* Kubernetes nodes
* Web servers
* Application servers
* Monitoring systems
* Cloud VMs
* DevOps automation platforms

You will also have a collection of **reusable Linux automation scripts and troubleshooting scenarios** that can be applied to later projects in this repository.

```

This first project establishes the **Linux foundation** that the later projects—Jenkins, Docker, Terraform, Kubernetes, DevSecOps, GitOps, and SRE—will build upon.
```
