#  SOP: Managing Kernel Parameters using sysctl
<img width="267" height="223" alt="Screenshot 2025-10-29 193158" src="https://github.com/user-attachments/assets/37d3a082-532d-4678-86aa-9d6f3f5c543e" />




---

| **Author** | **Created on** | **Version** | **Last updated by** | **Last Edited On** | **Level** | **Reviewer** |
|-------------|----------------|--------------|---------------------|--------------------|------------|---------------|
| Saniya | 2025-10-28 | 1.0 | Saniya | 2025-10-31 | Internal Review | Team |
| Saniya | 2025-10-28 | 1.0 | Saniya | 2025-11-04 | Pre Review | Prashant |
| Saniya | 2025-10-28 | 1.0 | Saniya | 2025-11-05 | Pre Review | Aman |

---

##  Table of Contents
- [Introduction](#introduction)
- [Purpose](#purpose)
- [Prerequisites](#prerequisites)
- [View](#view)
- [Making Sysctl Changes Permanent](#making-sysctl-changes-permanent)
- [Apply](#apply)
- [Troubleshooting](#troubleshooting)
- [Contact Information](#contact-information)
- [References](#references)

---

##  Introduction

The `sysctl` command provides administrators with a safe and flexible way to **view, modify, and apply** these kernel parameters — either temporarily (runtime) or permanently (persisted across reboots).  .  

---

##  Purpose

This SOP describes the procedure to **view**, **modify**, and **persist** Linux kernel parameters using the `sysctl` command.  
It ensures system administrators can safely tune **system performance** or **security settings** while maintaining configuration consistency across reboots.

---


##  Prerequisites

- User must have **root** or **sudo** privileges.  
- Kernel parameter names and values to be modified should be **reviewed and approved** .
- Backup of `/etc/sysctl.conf` or custom configuration files should be taken before modification.

---

##  View:

**1.** **View all parameters**
```bash
sysctl -a
```
<img width="905" height="442" alt="image" src="https://github.com/user-attachments/assets/c53b19f7-35d3-4aa1-a515-804e0f380943" />


---

**2.** **Search for a specific parameter** 
```bash
sysctl net.ipv4.ip_forward 
sysctl net.ipv4.conf.all.rp_filter
```

---
<img width="652" height="48" alt="image" src="https://github.com/user-attachments/assets/46f2f9a4-c628-4e18-83a4-48cc447de95d" />

<img width="757" height="51" alt="image" src="https://github.com/user-attachments/assets/e0df3b7d-e93e-48ac-913d-279b57fcf40e" />

**3.**  **View directly from /proc**
```bash
cat /proc/sys/net/ipv4/ip_forward
```
<img width="751" height="45" alt="image" src="https://github.com/user-attachments/assets/37df6538-a2b0-42e9-819e-3d639c9330eb" />


### Temporarily Changing Kernel Parameters (Runtime)

Changes made using the sysctl -w command are applied immediately but not persistent across reboots.
```bash
sudo sysctl -w <parameter>=<value>
```
Examples:

1.Performance tuning 
```bash
sudo sysctl -w vm.swappiness=10
```
<img width="716" height="70" alt="image" src="https://github.com/user-attachments/assets/9ad3e196-68e3-417f-aa3d-96c4b05fa1fc" />

2.Security hardening 
```bash
sudo sysctl -w net.ipv4.conf.all.rp_filter=1
sudo sysctl -w net.ipv4.tcp_syncookies=1
```

---

## Making Sysctl Changes Permanent

Temporary changes revert after reboot. To make them permanent, modify sysctl configuration files.

### Option 1: Edit /etc/sysctl.conf (Global file) 
```bash
sudo vi /etc/sysctl.conf
```
<img width="1192" height="906" alt="image" src="https://github.com/user-attachments/assets/1c077ffd-90e4-4689-8494-57ba5947719b" />

Add or update parameters:
- net.ipv4.ip_forward=1
- net.ipv4.tcp_syncookies=1
                            
### Option 2: Use drop-in config directory
For better modularity, add parameter files under /etc/sysctl.d/ 
```bash
sudo vi /etc/sysctl.d/99-performance.conf
```
Add below content shown in image:
<img width="1040" height="456" alt="image" src="https://github.com/user-attachments/assets/6d205a02-8eb4-4028-a51a-97f920763c52" />

---

## Apply
Applying Changes Without Reboot
**After saving the configuration file, reload all sysctl settings.**
```bash
sudo sysctl --system
```
<img width="1012" height="815" alt="image" src="https://github.com/user-attachments/assets/e148fbbb-0f48-4188-8b93-c826c23fd895" />


Verify the applied settings
```bash
sysctl -a | grep <parameter>
```
<img width="851" height="236" alt="image" src="https://github.com/user-attachments/assets/c1f700ba-413c-4b0e-a73c-519d59df63cc" />


---

##  Troubleshooting

| **Issue** | **Cause** | **Solution** |
|------------|------------|--------------|
| `sysctl: permission denied` | Non-root user | Use `sudo` when executing `sysctl` commands. |
| Changes not persistent after reboot | Configuration not saved in `/etc/sysctl.conf` or `/etc/sysctl.d/` | Recheck the configuration file and run `sudo sysctl --system` to apply changes. |
| Parameter not found | Incorrect parameter name or kernel module missing | Verify available parameters with `sysctl -a` and ensure the correct module is loaded. |

---

## Contact Information
| **Name** | **Email** |
|-----------|-----------|
| Saniya | saniya.dhalayat.snaatak@mygurukulam.co |

---

##  References

| **Topic** | **Link** | 
|------------|-----------|
| Red Hat Sysctl Documentation | [Red Hat Documentation](https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/4/html/reference_guide/s1-proc-sysctl) 
| Ubuntu sysctl Man Page | [Ubuntu Man Page](https://manpages.ubuntu.com/manpages/jammy/en/man8/sysctl.8.html) | 
---

