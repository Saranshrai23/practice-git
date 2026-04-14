"# 📄 Disk & Ulimit Management

## 🚀 Overview
This guide explains basic disk management and ulimit configuration in Linux (Ubuntu).  
It helps in monitoring disk usage and controlling system resource limits to avoid system failures.

---

## 🎯 Purpose
- Monitor disk usage
- Prevent disk full issues
- Control system resources using ulimit
- Improve system stability

---

## 📌 Scope
- Ubuntu-based systems
- Dev / Test / Production environments
- Beginner to Intermediate DevOps use cases

---

## ⚙️ Prerequisites
- Ubuntu system (18.04 or later)
- sudo/root access
- Basic Linux command-line knowledge

---

## 💻 Disk Management

### Check Disk Usage
```bash
df -h
```

### Check Folder Size
```bash
du -sh /var/*
```

### Check Disks & Partitions
```bash
lsblk
```

### Find Large Files
```bash
du -ah /var | sort -rh | head -20
```

### Check Inode Usage
```bash
df -i
```

### Clean Up Space
```bash
# Clear APT cache
sudo apt-get clean

# Remove old packages
sudo apt-get autoremove

# Clear systemd logs
sudo journalctl --vacuum-size=100M

# Delete old temp files
find /tmp -mtime +7 -delete
```

---

## ⚡ Ulimit (Resource Limits)

### Check Current Limits
```bash
ulimit -a
```

### Check Specific Limits
```bash
ulimit -n    # Open files
ulimit -u    # Max processes
```

### Temporary Limit Increase
```bash
ulimit -n 65535
```

### Permanent Limit Configuration
```bash
sudo nano /etc/security/limits.conf
```

Add:
```
* soft nofile 65535
* hard nofile 65535
* soft nproc 4096
* hard nproc 4096
```

**Note:** Logout/Login required for changes to take effect

### For Systemd Services
```bash
sudo nano /etc/systemd/system/myapp.service
```

Add under `[Service]`:
```ini
[Service]
LimitNOFILE=65535
LimitNPROC=4096
```

Then reload:
```bash
sudo systemctl daemon-reload
sudo systemctl restart myapp
```

---

## 🧠 Use Cases

### Scenario 1: Disk Full
```bash
# Check which partition is full
df -h

# Find what's using space
du -sh /*

# Clean package cache
sudo apt-get clean

# Check for deleted files still using space
lsof +L1
```

### Scenario 2: Too Many Open Files Error
```bash
# Check current limit
ulimit -n

# Temporarily increase (testing)
ulimit -n 65535

# Make it permanent
echo \"* soft nofile 65535\" | sudo tee -a /etc/security/limits.conf
echo \"* hard nofile 65535\" | sudo tee -a /etc/security/limits.conf
```

### Scenario 3: Can't Create Files (Inode Exhaustion)
```bash
# Check inode usage
df -i

# Find directories with many files
for dir in /*; do echo \"$dir: $(find $dir -type f 2>/dev/null | wc -l)\"; done

# Clean cache/temp files
find /var/cache -type f -delete
```

---

## 🛠️ Troubleshooting

| Issue | Command to Check | Solution |
|-------|-----------------|----------|
| Disk full | `df -h` | `du -sh /*` → Clean logs/cache |
| System slow | `df -h && df -i` | Check disk/inode usage |
| Too many open files | `ulimit -n` | Increase nofile limit |
| Cannot fork process | `ulimit -u` | Increase nproc limit |
| Changes not applied | `ulimit -a` | Re-login or restart service |
| Disk full but du shows space | `lsof +L1` | Restart process holding deleted files |

---

## 🔍 Quick Commands Reference

### Disk Monitoring
```bash
df -h              # Disk usage (human-readable)
df -i              # Inode usage
du -sh /path       # Directory size
lsof +L1           # Deleted files still in use
ncdu /             # Interactive disk analyzer
```

### Ulimit Commands
```bash
ulimit -a          # Show all limits
ulimit -n          # Show max open files
ulimit -u          # Show max processes
ulimit -n 65535    # Set open files (temporary)
```

### Emergency Cleanup
```bash
sudo journalctl --vacuum-size=100M
sudo apt-get clean && sudo apt-get autoremove
find /tmp -mtime +7 -delete
truncate -s 0 /var/log/large.log
```

---

## ✅ Best Practices

✅ Regularly monitor disk usage (set alerts at 80%)  
✅ Clean unnecessary files/logs periodically  
✅ Avoid setting unlimited resource values  
✅ Always test ulimit changes in non-production first  
✅ Document all configuration changes  
✅ Use log rotation (logrotate) to prevent disk full  
✅ Set appropriate limits based on application needs  

---

## 📊 Important Ulimit Parameters

| Parameter | Meaning | Recommended Value |
|-----------|---------|------------------|
| `nofile` | Max open files | 65535 |
| `nproc` | Max processes | 4096 |
| `core` | Core dump size | 0 (disabled) |
| `memlock` | Locked memory | unlimited (for databases) |

---

## 🎓 For DevOps Freshers

### What to Know for Interviews

**Q: Difference between `df` and `du`?**  
A: `df` shows disk space per partition, `du` shows size of specific directories/files.

**Q: What is an inode?**  
A: Metadata of a file (permissions, owner, location). If inodes are full, you can't create new files even with free space.

**Q: How to fix \"Too many open files\" error?**  
A: Increase ulimit using `ulimit -n 65535` (temporary) or edit `/etc/security/limits.conf` (permanent).

**Q: Soft vs Hard limit?**  
A: Soft = default limit, Hard = maximum limit. Users can increase soft up to hard, but only root can change hard limit.

---

## 📚 References

| Resource | Link |
|----------|------|
| Linux df command | `man df` or https://linux.die.net/man/1/df |
| Linux du command | `man du` or https://linux.die.net/man/1/du |
| ulimit guide | https://ss64.com/bash/ulimit.html |
| limits.conf | `man limits.conf` or https://linux.die.net/man/5/limits.conf |
| Ubuntu disk docs | https://help.ubuntu.com/community/LinuxFilesystemsExplained |

---

## 📝 Author

**Saransh Rai**  
Created: 2026-04-13  
Version: 1.0

---

## 📄 License

This documentation is free to use for educational purposes.

---

## 🤝 Contributing
