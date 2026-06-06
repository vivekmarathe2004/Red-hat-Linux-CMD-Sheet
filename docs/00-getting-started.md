# Getting Started

> **Core Doc** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Baseline commands for discovering a RHEL system, confirming version details, and working safely.

## Why It Matters

This topic affects real server behavior. If you can explain the purpose, inspect the current state, make a safe change, and verify it, you are doing administrator work rather than memorizing syntax.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/redhat-release` | Human-readable RHEL release |
| `/etc/os-release` | OS identity variables |
| `/etc/hostname` | Persistent hostname |
| `/etc/motd` | Message shown at login |
| `/var/log/` | System and service logs |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Show release**: `cat /etc/redhat-release` - Quick version check
- **Show OS variables**: `cat /etc/os-release` - Useful in scripts
- **Show kernel**: `uname -r` - Running kernel only
- **Show architecture**: `uname -m` - Example: `x86_64`, `aarch64`
- **Show host info**: `hostnamectl` - Hostname, OS, kernel
- **Show uptime**: `uptime` - Load average included
- **Show current user**: `id` - UID, GID, groups
- **Run as root**: `sudo <command>` - Preferred over root shell
- **Root shell**: `sudo -i` - Use carefully
- **Command help**: `man <command>` - Manual pages
- **Locate command**: `command -v <command>` - Shows executable path

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

## Try It In A VM

Run the workflow on a disposable RHEL VM. Change one setting, verify the result, then undo or document what changed. This builds the habit you need for production systems.

## Verify

```bash
hostnamectl
uptime
who
last -n 5
```

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Command not found**: check `command -v <command>`, then Install package or use full path.
- **Permission denied**: check `id`, then Use `sudo` or fix ownership/mode.
- **Hostname did not persist**: check `hostnamectl`, then Use `hostnamectl set-hostname`.

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For Getting Started, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

Core discovery commands are the same. Expect package versions, default crypto policies, and some package names to differ.

## Page Navigation

Previous | [Docs Index](README.md) | [Next](01-installation-and-subscription.md)
