# Getting Started

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Baseline commands for discovering a RHEL system, confirming version details, and working safely.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/redhat-release` | Human-readable RHEL release |
| `/etc/os-release` | OS identity variables |
| `/etc/hostname` | Persistent hostname |
| `/etc/motd` | Message shown at login |
| `/var/log/` | System and service logs |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Show release | `cat /etc/redhat-release` | Quick version check |
| Show OS variables | `cat /etc/os-release` | Useful in scripts |
| Show kernel | `uname -r` | Running kernel only |
| Show architecture | `uname -m` | Example: `x86_64`, `aarch64` |
| Show host info | `hostnamectl` | Hostname, OS, kernel |
| Show uptime | `uptime` | Load average included |
| Show current user | `id` | UID, GID, groups |
| Run as root | `sudo <command>` | Preferred over root shell |
| Root shell | `sudo -i` | Use carefully |
| Command help | `man <command>` | Manual pages |
| Locate command | `command -v <command>` | Shows executable path |

## Configuration Workflow

```bash
# Confirm system identity
cat /etc/redhat-release
hostnamectl

# Set hostname
sudo hostnamectl set-hostname <hostname>

# Update packages
sudo dnf update

# Reboot if kernel, systemd, glibc, or critical services were updated
sudo systemctl reboot
```

## Verify

```bash
hostnamectl
uptime
who
last -n 5
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Command not found | `command -v <command>` | Install package or use full path |
| Permission denied | `id` | Use `sudo` or fix ownership/mode |
| Hostname did not persist | `hostnamectl` | Use `hostnamectl set-hostname` |

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For Getting Started, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

Core discovery commands are the same. Expect package versions, default crypto policies, and some package names to differ.

