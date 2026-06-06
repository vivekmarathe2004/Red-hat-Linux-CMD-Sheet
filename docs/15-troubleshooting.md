# Troubleshooting

> **Core Doc** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Use a repeatable flow for diagnosing boot, network, service, storage, package, and security issues.

## Why It Matters

This topic affects real server behavior. If you can explain the purpose, inspect the current state, make a safe change, and verify it, you are doing administrator work rather than memorizing syntax.

## Important Files

| Path | Purpose |
| --- | --- |
| `/var/log/messages` | General logs |
| `/var/log/secure` | Auth logs |
| `/var/log/audit/audit.log` | Audit and SELinux |
| `/etc/fstab` | Mount failures |
| `/etc/yum.repos.d/` | Repo failures |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Failed services**: `systemctl --failed` - First stop
- **Boot logs**: `journalctl -b` - Current boot
- **Previous boot**: `journalctl -b -1` - Last boot
- **Critical logs**: `journalctl -p err -b` - Errors
- **Kernel ring**: `dmesg -T` - Hardware/kernel
- **Network**: `ip addr; ip route` - Address and route
- **DNS**: `dig <name>` - Resolver test
- **Ports**: `ss -tulpn` - Listening services
- **Disk**: `lsblk -f; df -hT` - Storage state
- **SELinux denials**: `ausearch -m AVC -ts recent` - Access denials
- **Package check**: `dnf check` - Dependency health

## Configuration Workflow

```bash
# Basic triage

hostnamectl
systemctl --failed
journalctl -p err -b
ip addr
ip route
df -hT
free -h

# Service-specific triage

systemctl status <service>
journalctl -u <service> -b
sudo ss -tulpn | grep <port>
sudo firewall-cmd --list-all
```

## Try It In A VM

Run the workflow on a disposable RHEL VM. Change one setting, verify the result, then undo or document what changed. This builds the habit you need for production systems.

## Verify

```bash
systemctl is-system-running
systemctl --failed
journalctl -p err -b
```

## Troubleshooting

| Symptom | Commands | Direction |
| --- | --- | --- |
| Cannot boot normally | `journalctl -xb`, `systemctl --failed` | Fix failed mount, service, or driver |
| Network unreachable | `ip addr`, `ip route`, `nmcli device status` | Fix interface, route, gateway |
| Service unreachable | `systemctl status`, `ss -tulpn`, `firewall-cmd --list-all` | Start service or open firewall |
| Permission issue | `ls -l`, `namei -l`, `ls -Z` | Fix Unix permissions or SELinux context |
| Package issue | `dnf repolist`, `dnf check`, `dnf history` | Fix repo or transaction |

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For Troubleshooting, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

Use the same diagnostic flow on both versions. Interpret results using the package and service versions installed on the target system.

## Page Navigation

[Previous](14-automation-shell-and-cron.md) | [Docs Index](README.md) | Next
