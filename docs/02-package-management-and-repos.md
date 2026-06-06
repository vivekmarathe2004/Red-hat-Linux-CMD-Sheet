# Package Management And Repos

> **Core Doc** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Install, update, remove, search, and manage packages and repositories with `dnf`.

## Why It Matters

This topic affects real server behavior. If you can explain the purpose, inspect the current state, make a safe change, and verify it, you are doing administrator work rather than memorizing syntax.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/yum.repos.d/` | Repository files |
| `/var/cache/dnf/` | DNF cache |
| `/etc/dnf/dnf.conf` | Main DNF config |
| `/var/log/dnf.log` | DNF log |
| `/var/log/dnf.rpm.log` | RPM transaction log |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Update metadata**: `sudo dnf makecache` - Refresh cache
- **Search**: `dnf search <term>` - Searches names and summaries
- **Show package**: `dnf info <package>` - Package details
- **Install**: `sudo dnf install <package>` - Installs dependencies
- **Remove**: `sudo dnf remove <package>` - Review removals carefully
- **Update all**: `sudo dnf update` - Applies updates
- **List repos**: `dnf repolist --all` - Shows enabled and disabled
- **List installed**: `dnf list installed` - Installed RPMs
- **Find owner**: `rpm -qf <path>` - Which package owns a file
- **Package files**: `rpm -ql <package>` - File list
- **History**: `dnf history` - Transaction history
- **Undo transaction**: `sudo dnf history undo <id>` - Not always possible

## Configuration Workflow

```bash
# Enable Red Hat repositories through subscription-manager when possible

sudo subscription-manager repos --list
sudo subscription-manager repos --enable=<repo-id>

# Add a custom repo file

sudo dnf config-manager --add-repo <repo-url>

# Install plugin if config-manager is missing

sudo dnf install dnf-plugins-core

# Disable a repo

sudo dnf config-manager --set-disabled <repo-id>

# Clean cache

sudo dnf clean all
sudo dnf makecache
```

## Try It In A VM

Run the workflow on a disposable RHEL VM. Change one setting, verify the result, then undo or document what changed. This builds the habit you need for production systems.

## Verify

```bash
dnf repolist
dnf check
rpm -Va
```

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Package unavailable**: check `dnf repolist --all`, then Enable required repo.
- **Broken dependencies**: check `dnf repoquery --unsatisfied`, then Fix repo mix or remove conflict.
- **Bad metadata**: check `sudo dnf clean all`, then Rebuild cache.

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For Package Management And Repos, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

Both use `dnf`. Avoid legacy `yum` examples unless documenting compatibility; on modern RHEL, `yum` is a compatibility entry point for DNF behavior.

## Page Navigation

[Previous](01-installation-and-subscription.md) | [Docs Index](README.md) | [Next](03-filesystem-and-files.md)
