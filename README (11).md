
Action: file_editor create /app/SOP_Disk_and_Ulimit_Management.md --file-text "# SOP – Disk & Ulimit Management

## Standard Operating Procedure

---

## Document Details

| Author         | Created    | Version | Last updated by | Last Edited On | PRE Reviewer | L0 Reviewer | L1 Reviewer | L2 Reviewer |
| -------------- | ---------- | ------- | --------------- | -------------- | ------------ | ----------- | ----------- | ----------- |
| Bhawna Dangarh | 2026-04-13 | 1.0     | Bhawna Dangarh  | 2026-04-13     |              |             |             |             |

---

## 📑 Table of Contents

1. [Purpose](#1-purpose)
2. [Scope](#2-scope)
3. [Pre-requisites](#3-pre-requisites)
4. [Architecture](#4-architecture)
5. [Disk Management Commands](#5-disk-management-commands)
6. [Ulimit Configuration](#6-ulimit-configuration)
7. [Step-by-Step Configuration](#7-step-by-step-configuration)
8. [Monitoring](#8-monitoring)
9. [Disaster Recovery](#9-disaster-recovery)
10. [Troubleshooting](#10-troubleshooting)
11. [Best Practices](#11-best-practices)
12. [FAQs](#12-faqs)
13. [Contact Information](#13-contact-information)
14. [References](#14-references)

---

## 1. Purpose

To define procedures for monitoring disk space usage, managing disk quotas, configuring user/process limits (ulimits), and responding to disk capacity or limit-related alerts to ensure system stability, performance, and availability.

**Key Objectives:**
- Prevent disk space exhaustion
- Optimize system resource allocation
- Control process resource consumption
- Maintain system stability and performance
- Ensure compliance with capacity planning requirements

---

## 2. Scope

This SOP applies to:

- Ubuntu-based systems (server and desktop environments)
- Cloud and on-premise infrastructure
- Production, staging, and development environments
- Support Engineers, System Administrators, DevOps Engineers, and SRE teams

**Covered Topics:**
- Disk space monitoring and management
- Disk quota implementation
- Ulimit configuration for users and processes
- Emergency disk recovery procedures
- Resource limit optimization

---

## 3. Pre-requisites

### System Requirements

| Requirement           | Specification                              |
| --------------------- | ------------------------------------------ |
| **Operating System**  | Ubuntu 18.04 LTS or later                  |
| **Access Level**      | Root or sudo privileges                    |
| **Disk Tools**        | df, du, quota, fdisk (pre-installed)       |
| **Security Tools**    | /etc/security/limits.conf available        |

### Verify Installation

```bash
# Check disk utilities
df --version
du --version

# Check quota support
quota --version

# Verify limits configuration exists
ls -l /etc/security/limits.conf
```

---

## 4. Architecture

### 4.1 Disk Management Structure

| Component              | Purpose                                    |
| ---------------------- | ------------------------------------------ |
| **Filesystem**         | Logical storage structure (ext4, xfs)      |
| **Partitions**         | Physical disk divisions                    |
| **Mount Points**       | Directory access points for filesystems    |
| **Inodes**             | File metadata structures                   |
| **Quotas**             | Per-user/group storage limits              |

### 4.2 Ulimit Architecture

| Component                     | Description                              |
| ----------------------------- | ---------------------------------------- |
| **/etc/security/limits.conf** | System-wide user/group limits            |
| **/etc/security/limits.d/**   | Drop-in configuration files              |
| **PAM (Pluggable Auth)**      | Enforces limits at login                 |
| **Shell ulimit**              | Runtime limit viewing/setting            |

### 4.3 Key Ulimit Parameters

| Parameter   | Description                       | Example Value |
| ----------- | --------------------------------- | ------------- |
| `nofile`    | Maximum open files                | 65536         |
| `nproc`     | Maximum number of processes       | 4096          |
| `memlock`   | Maximum locked-in-memory (KB)     | unlimited     |
| `cpu`       | Maximum CPU time (minutes)        | unlimited     |
| `fsize`     | Maximum file size (KB)            | unlimited     |
| `core`      | Maximum core file size (blocks)   | 0             |
| `as`        | Maximum address space (KB)        | unlimited     |
| `stack`     | Maximum stack size (KB)           | 8192          |

### 4.4 Limit Types

| Type       | Description                                       |
| ---------- | ------------------------------------------------- |
| **soft**   | Default limit, can be increased by user up to hard limit |
| **hard**   | Maximum limit, requires root to modify            |
| **-**      | Sets both soft and hard limits simultaneously     |

---

## 5. Disk Management Commands

### 5.1 Disk Usage Monitoring

| Command                | Description                          | Example                              |
| ---------------------- | ------------------------------------ | ------------------------------------ |
| `df -h`                | Show disk space (human readable)     | `df -h`                              |
| `df -i`                | Show inode usage                     | `df -i`                              |
| `df -T`                | Show filesystem types                | `df -T`                              |
| `du -sh <dir>`         | Directory size summary               | `du -sh /var/log`                    |
| `du -h --max-depth=1`  | Size of subdirectories (1 level)     | `du -h --max-depth=1 /home`          |
| `du -ah \| sort -rh \| head -20` | Top 20 largest files/dirs  | `du -ah /var \| sort -rh \| head -20`|

### 5.2 Disk Space Analysis

| Command                             | Description                        | Example                              |
| ----------------------------------- | ---------------------------------- | ------------------------------------ |
| `ncdu /path`                        | Interactive disk usage analyzer    | `ncdu /var`                          |
| `find / -type f -size +100M`        | Find files larger than 100MB       | `find /home -type f -size +100M`     |
| `lsof +L1`                          | Find deleted but open files        | `lsof +L1`                           |
| `lsblk`                             | List block devices                 | `lsblk -f`                           |
| `fdisk -l`                          | List all partitions                | `sudo fdisk -l`                      |

### 5.3 Disk Quota Management

| Command                             | Description                        | Example                              |
| ----------------------------------- | ---------------------------------- | ------------------------------------ |
| `quotaon -av`                       | Enable quota on all filesystems    | `sudo quotaon -av`                   |
| `quotaoff -av`                      | Disable quota on all filesystems   | `sudo quotaoff -av`                  |
| `quota -u username`                 | Show user quota                    | `quota -u john`                      |
| `edquota -u username`               | Edit user quota                    | `sudo edquota -u john`               |
| `edquota -g groupname`              | Edit group quota                   | `sudo edquota -g developers`         |
| `setquota -u user <limits>`         | Set quota from command line        | `setquota -u john 1000000 1100000 0 0 /home` |
| `repquota -a`                       | Report quota usage                 | `sudo repquota -a`                   |
| `quotacheck -cugm /mount`           | Create/update quota files          | `sudo quotacheck -cugm /home`        |

### 5.4 Emergency Disk Operations

| Command                             | Description                        | Example                              |
| ----------------------------------- | ---------------------------------- | ------------------------------------ |
| `journalctl --vacuum-size=100M`     | Reduce systemd journal size        | `journalctl --vacuum-size=100M`      |
| `truncate -s 0 file.log`            | Empty a large log file             | `truncate -s 0 /var/log/app.log`     |
| `find /tmp -mtime +7 -delete`       | Delete files older than 7 days     | `find /tmp -mtime +7 -delete`        |
| `apt-get clean`                     | Clear APT cache                    | `sudo apt-get clean`                 |
| `apt-get autoremove`                | Remove unused packages             | `sudo apt-get autoremove`            |

---

## 6. Ulimit Configuration

### 6.1 Viewing Current Limits

| Command                | Description                          | Example                              |
| ---------------------- | ------------------------------------ | ------------------------------------ |
| `ulimit -a`            | Show all limits for current shell    | `ulimit -a`                          |
| `ulimit -n`            | Show max open files                  | `ulimit -n`                          |
| `ulimit -u`            | Show max user processes              | `ulimit -u`                          |
| `ulimit -Hn`           | Show hard limit for open files       | `ulimit -Hn`                         |
| `ulimit -Sn`           | Show soft limit for open files       | `ulimit -Sn`                         |
| `cat /proc/<pid>/limits` | Show limits for specific process   | `cat /proc/1234/limits`              |

### 6.2 Setting Temporary Limits

| Command                | Description                          | Example                              |
| ---------------------- | ------------------------------------ | ------------------------------------ |
| `ulimit -n 65536`      | Set max open files (current shell)   | `ulimit -n 65536`                    |
| `ulimit -u 4096`       | Set max user processes               | `ulimit -u 4096`                     |
| `ulimit -Hn 100000`    | Set hard limit for open files        | `ulimit -Hn 100000`                  |
| `ulimit -c unlimited`  | Enable core dumps (unlimited size)   | `ulimit -c unlimited`                |

### 6.3 Persistent Limit Configuration

**Configuration File:** `/etc/security/limits.conf`

**Syntax:**
```
<domain>  <type>  <item>  <value>
```

**Examples:**

```bash
# User-specific limits
john        soft    nofile  4096
john        hard    nofile  65536

# Group limits
@developers soft    nproc   2048
@developers hard    nproc   4096

# Wildcard (all users)
*           soft    core    0
*           hard    core    0

# Service account
nginx       soft    nofile  65536
nginx       hard    nofile  100000

# Database users
@database   soft    memlock unlimited
@database   hard    memlock unlimited
```

---

## 7. Step-by-Step Configuration

### 7.1 Enable Disk Quotas

**Step 1: Edit /etc/fstab**

```bash
sudo nano /etc/fstab
```

Add `usrquota,grpquota` to the filesystem options:

```
/dev/sda1  /home  ext4  defaults,usrquota,grpquota  0  2
```

**Step 2: Remount Filesystem**

```bash
sudo mount -o remount /home
```

**Step 3: Create Quota Database**

```bash
sudo quotacheck -cugm /home
```

**Step 4: Enable Quotas**

```bash
sudo quotaon -v /home
```

**Step 5: Set User Quota**

```bash
sudo edquota -u username
```

**Step 6: Verify Quota**

```bash
quota -v username
```

---

### 7.2 Configure Persistent Ulimits

**Step 1: Edit System-Wide Limits**

```bash
sudo nano /etc/security/limits.conf
```

**Step 2: Add Limit Entries**

```bash
# Example: Increase open file limits for all users
*    soft    nofile  65536
*    hard    nofile  100000

# Example: Set process limits for web server user
www-data    soft    nproc   4096
www-data    hard    nproc   8192
```

**Step 3: Create Service-Specific Limits**

```bash
sudo nano /etc/security/limits.d/nginx.conf
```

```bash
nginx    soft    nofile  65536
nginx    hard    nofile  100000
nginx    soft    nproc   4096
nginx    hard    nproc   8192
```

**Step 4: Apply Changes**

For PAM-managed sessions, users must log out and log back in. For systemd services:

```bash
sudo systemctl daemon-reload
sudo systemctl restart <service>
```

**Step 5: Verify Limits**

```bash
# Check as specific user
su - username -c 'ulimit -a'

# Check for running process
cat /proc/$(pidof nginx)/limits
```

---

### 7.3 Monitor and Alert on Disk Usage

**Step 1: Create Monitoring Script**

```bash
sudo nano /usr/local/bin/disk-monitor.sh
```

```bash
#!/bin/bash
THRESHOLD=90

df -H | grep -vE '^Filesystem|tmpfs|cdrom' | awk '{ print $5 \" \" $1 }' | while read output;
do
  usage=$(echo $output | awk '{ print $1}' | sed 's/%//g')
  partition=$(echo $output | awk '{ print $2 }')
  
  if [ $usage -ge $THRESHOLD ]; then
    echo \"ALERT: Disk usage on $partition is at ${usage}% (Threshold: ${THRESHOLD}%)\"
    # Send alert (email, Slack, etc.)
  fi
done
```

**Step 2: Make Executable**

```bash
sudo chmod +x /usr/local/bin/disk-monitor.sh
```

**Step 3: Schedule via Cron**

```bash
sudo crontab -e
```

```bash
# Run disk monitoring every hour
0 * * * * /usr/local/bin/disk-monitor.sh
```

---

## 8. Monitoring

### 8.1 Disk Monitoring Metrics

| Parameter              | Description                        | Priority | Threshold       |
| ---------------------- | ---------------------------------- | -------- | --------------- |
| Disk Utilization (%)   | Percentage of disk space used      | High     | > 90%           |
| Inode Utilization (%)  | Percentage of inodes used          | High     | > 90%           |
| Disk I/O Wait          | CPU time waiting for disk I/O      | Medium   | > 20%           |
| Read/Write Throughput  | Disk read/write performance        | Medium   | < 50 MB/s       |
| Quota Exceeded Events  | Number of quota violations         | High     | > 0             |

### 8.2 Ulimit Monitoring Metrics

| Parameter              | Description                        | Priority | Threshold       |
| ---------------------- | ---------------------------------- | -------- | --------------- |
| Open File Descriptors  | Current vs. limit                  | High     | > 80% of limit  |
| Process Count          | Number of processes per user       | Medium   | > 75% of limit  |
| Core Dumps             | Unexpected core file generation    | High     | > 0             |
| Memory Lock Violations | Failed memory lock attempts        | Medium   | > 5 per hour    |

### 8.3 Health Check Commands

| Check Type             | Command                                      |
| ---------------------- | -------------------------------------------- |
| Disk Space             | `df -h \| awk '$5+0 > 90 {print $0}'`        |
| Inode Usage            | `df -i \| awk '$5+0 > 90 {print $0}'`        |
| Top Disk Consumers     | `du -sh /var/* \| sort -rh \| head -10`      |
| Open Files per Process | `lsof -u username \| wc -l`                  |
| Current Ulimits        | `ulimit -a`                                  |
| Process Limits         | `cat /proc/<PID>/limits`                     |

### 8.4 Logging

| Log Type               | Location                                     |
| ---------------------- | -------------------------------------------- |
| Disk Errors            | `/var/log/syslog`, `dmesg \| grep -i disk`   |
| Quota Violations       | `/var/log/auth.log`, `/var/log/syslog`       |
| Ulimit Violations      | Application logs, `/var/log/messages`        |
| Filesystem Events      | `journalctl -u systemd-fsck*`                |

---

## 9. Disaster Recovery

### 9.1 Disk Full Emergency Procedures

**Immediate Actions:**

1. **Identify the Issue**
   ```bash
   df -h
   ```

2. **Find Large Files**
   ```bash
   du -ah /var | sort -rh | head -20
   find /var/log -type f -size +100M
   ```

3. **Clear Log Files**
   ```bash
   sudo truncate -s 0 /var/log/large_app.log
   journalctl --vacuum-size=100M
   ```

4. **Remove Package Caches**
   ```bash
   sudo apt-get clean
   sudo apt-get autoremove
   ```

5. **Find Deleted but Open Files**
   ```bash
   lsof +L1
   # Restart services holding deleted files
   sudo systemctl restart <service>
   ```

6. **Emergency Disk Expansion** (if on LVM or cloud)
   ```bash
   # Extend volume in cloud console
   # Then resize filesystem
   sudo resize2fs /dev/sda1
   ```

### 9.2 Inode Exhaustion Recovery

```bash
# Find directories with many small files
for dir in /*; do echo \"$dir: $(find $dir -type f | wc -l)\"; done

# Remove old temporary files
find /tmp -type f -mtime +7 -delete
find /var/tmp -type f -mtime +30 -delete
```

### 9.3 Ulimit Issue Recovery

**Too Many Open Files Error:**

```bash
# Check current limit
ulimit -n

# Temporarily increase (current session)
ulimit -n 65536

# Permanent fix
echo \"* soft nofile 65536\" | sudo tee -a /etc/security/limits.conf
echo \"* hard nofile 100000\" | sudo tee -a /etc/security/limits.conf

# For systemd services
sudo nano /etc/systemd/system/<service>.service.d/limits.conf
```

```ini
[Service]
LimitNOFILE=65536
```

```bash
sudo systemctl daemon-reload
sudo systemctl restart <service>
```

### 9.4 High Availability Considerations

- Use **RAID** or **LVM** for disk redundancy
- Implement **disk usage monitoring** with automated alerts
- Maintain **backup** of critical data on separate volumes
- Configure **log rotation** to prevent log-related disk exhaustion
- Document **emergency contact procedures** for disk expansion

---

## 10. Troubleshooting

| Issue                           | Cause                              | Solution                                                                 |
| ------------------------------- | ---------------------------------- | ------------------------------------------------------------------------ |
| **Disk full but du shows space** | Deleted files held by processes   | `lsof +L1` → Restart services holding deleted files                      |
| **Cannot create files**         | Inode exhaustion                   | `df -i` → Remove small/temp files                                        |
| **Quota exceeded**              | User reached storage limit         | `quota -v user` → Increase quota or clean user files                     |
| **Too many open files**         | ulimit nofile too low              | Increase `nofile` in `/etc/security/limits.conf`                         |
| **Cannot fork process**         | ulimit nproc too low               | Increase `nproc` limit or kill unnecessary processes                     |
| **Quota not enforcing**         | Quota not enabled on filesystem    | Add `usrquota,grpquota` to `/etc/fstab`, remount, run `quotaon`          |
| **Ulimit changes not applying** | User not logged out                | Re-login required for PAM limits; `systemctl restart` for services       |
| **Disk I/O very slow**          | High disk usage or failing disk    | Check `iostat -x 1`, run `smartctl -a /dev/sda`, consider hardware check |
| **Core dumps filling disk**     | core limit set to unlimited        | Set `core` limit to 0 or specific size in `limits.conf`                  |

---

## 11. Best Practices

### 11.1 Disk Management

✅ **Monitor disk usage proactively** with automated alerts at 80% and 90% thresholds  
✅ **Implement log rotation** using `logrotate` to prevent log-related disk exhaustion  
✅ **Use separate partitions** for `/var`, `/tmp`, `/home` to isolate issues  
✅ **Regularly clean package caches** and temporary files  
✅ **Set up disk quotas** for multi-user systems  
✅ **Monitor inode usage** in addition to disk space  
✅ **Document mount points** and filesystem layouts  
✅ **Use LVM** for flexible disk management in production  
✅ **Test disk recovery procedures** regularly  

### 11.2 Ulimit Configuration

✅ **Set appropriate limits** based on application requirements  
✅ **Always set both soft and hard limits** for clarity  
✅ **Use `/etc/security/limits.d/`** for service-specific configurations  
✅ **Document all limit changes** with comments explaining the reason  
✅ **Test limit changes** in staging before production  
✅ **Avoid setting unlimited values** without justification  
✅ **Monitor processes** approaching their limits  
✅ **For systemd services**, use `LimitNOFILE` directive in service files  
✅ **Coordinate with application owners** before changing limits  
✅ **Regularly review and optimize** limits based on actual usage  

### 11.3 General Recommendations

✅ **Automate monitoring** using tools like Nagios, Prometheus, or Zabbix  
✅ **Create runbooks** for common disk/limit issues  
✅ **Maintain change logs** for all configuration modifications  
✅ **Set up centralized logging** for limit violations  
✅ **Conduct regular capacity planning** reviews  
✅ **Implement backup strategies** for critical data  

---

## 12. FAQs

### Q1: What are the default ulimits on Ubuntu?

**Answer:** Default ulimits vary by distribution and version. Check with `ulimit -a`. Common defaults:
- `nofile` (open files): 1024
- `nproc` (processes): varies (often based on available memory)

### Q2: How do I check if disk quotas are enabled on a filesystem?

**Answer:**
```bash
mount | grep quota
# Or
cat /proc/mounts | grep quota
```

### Q3: What's the difference between soft and hard limits?

**Answer:**
- **Soft limit**: The default enforced limit. Users can temporarily exceed it up to the hard limit.
- **Hard limit**: The absolute maximum. Only root can modify it. Users cannot exceed this.

### Q4: Can I set ulimits for specific services managed by systemd?

**Answer:** Yes. Edit the service unit file or create a drop-in configuration:

```bash
sudo systemctl edit <service>
```

```ini
[Service]
LimitNOFILE=65536
LimitNPROC=4096
```

### Q5: Why are my ulimit changes not applying?

**Answer:** Common reasons:
- User hasn't logged out and back in (PAM limits require re-login)
- Service wasn't restarted (for systemd services)
- Syntax error in `/etc/security/limits.conf`
- System-wide hard limit is lower (check `/proc/sys/fs/file-max`)

### Q6: How do I increase the system-wide file descriptor limit?

**Answer:**
```bash
# Temporary (current session)
sudo sysctl -w fs.file-max=2097152

# Permanent
echo \"fs.file-max=2097152\" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p
```

### Q7: What should I do if disk is full but I can't find large files?

**Answer:** Check for deleted files still held by processes:
```bash
lsof +L1
```
Restart the services holding these files.

### Q8: Is there a graphical tool for disk usage analysis?

**Answer:** Yes, several options:
- **ncdu**: Terminal-based (recommended for servers)
- **Baobab**: GNOME Disk Usage Analyzer (GUI)
- **filelight**: KDE disk usage tool

```bash
sudo apt-get install ncdu
ncdu /
```

### Q9: How often should I monitor disk usage?

**Answer:**
- **Production systems**: Every 5-15 minutes (automated monitoring)
- **Critical systems**: Real-time alerts at 80%, 90%, 95% thresholds
- **Development systems**: Hourly or daily checks

### Q10: Can disk quotas affect system performance?

**Answer:** Minimal impact in most cases. However:
- Initial `quotacheck` can be I/O intensive on large filesystems
- Continuous quota accounting has negligible overhead on modern systems
- Performance impact is usually acceptable for the benefits gained

---

## 13. Contact Information

| Name           | Email Address         | Role                    |
| -------------- | --------------------- | ----------------------- |
| Bhawna Dangarh | bhawna@example.com    | System Administrator    |
| Support Team   | support@example.com   | Infrastructure Support  |

---

## 14. References

| Description                        | Link                                                                 |
| ---------------------------------- | -------------------------------------------------------------------- |
| Ubuntu Disk Management Guide       | https://help.ubuntu.com/community/LinuxFilesystemsExplained          |
| Linux Quota Documentation          | https://www.kernel.org/doc/html/latest/filesystems/quota.html        |
| Ulimit Man Page                    | `man ulimit` or https://ss64.com/bash/ulimit.html                   |
| Limits.conf Documentation          | `man limits.conf` or https://linux.die.net/man/5/limits.conf        |
| Systemd Resource Control           | https://www.freedesktop.org/software/systemd/man/systemd.resource-control.html |
| Disk Usage Analysis Best Practices | https://ubuntu.com/server/docs/storage                               |
| LVM Administration Guide           | https://tldp.org/HOWTO/LVM-HOWTO/                                    |

---

**Document Version History:**

| Version | Date       | Changes                          | Author         |
| ------- | ---------- | -------------------------------- | -------------- |
| 1.0     | 2026-04-13 | Initial SOP creation             | Bhawna Dangarh |

---

**Approval:**

| Role           | Name | Signature | Date |
| -------------- | ---- | --------- | ---- |
| Author         |      |           |      |
| Reviewer (L0)  |      |           |      |
| Reviewer (L1)  |      |           |      |
| Approver (L2)  |      |           |      |

---

**END OF DOCUMENT**
"
Observation: Create successful: /app/SOP_Disk_and_Ulimit_Management.md
