# Filesystem And Files

> **Core Doc** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Navigate, inspect, copy, move, archive, search, and protect files.

## Why It Matters

This topic affects real server behavior. If you can explain the purpose, inspect the current state, make a safe change, and verify it, you are doing administrator work rather than memorizing syntax.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/fstab` | Persistent mounts |
| `/etc/updatedb.conf` | `locate` database config |
| `/tmp` | Temporary files |
| `/var/tmp` | Longer-lived temporary files |
| `/home` | User home directories |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **List files**: `ls -la` - Include hidden files
- **Print directory**: `pwd` - Current path
- **Copy**: `cp -a <src> <dst>` - Preserve attributes
- **Move**: `mv <src> <dst>` - Rename or move
- **Remove**: `rm <path>` - Warning: destructive
- **Create directory**: `mkdir -p <dir>` - Creates parents
- **View file**: `less <file>` - Pager
- **Tail log**: `tail -f <file>` - Follow updates
- **Search text**: `grep -R "<text>" <dir>` - Recursive search
- **Find files**: `find <dir> -name "<pattern>"` - Flexible search
- **Disk usage**: `du -sh <path>` - Human-readable size
- **Filesystem space**: `df -h` - Mounted filesystems
- **Archive**: `tar -czf <file>.tar.gz <dir>` - Create gzip tarball
- **Extract**: `tar -xzf <file>.tar.gz` - Extract archive

## Configuration Workflow

```bash
# Create an application directory

sudo mkdir -p /opt/<app>
sudo chown <user>:<group> /opt/<app>
sudo chmod 0750 /opt/<app>

# Copy config safely

sudo cp -a /etc/<file> /etc/<file>.bak.$(date +%F)
sudo vi /etc/<file>
```

## Try It In A VM

Run the workflow on a disposable RHEL VM. Change one setting, verify the result, then undo or document what changed. This builds the habit you need for production systems.

## Verify

```bash
ls -ld /opt/<app>
stat /opt/<app>
df -h
du -sh /opt/<app>
```

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Permission denied**: check `namei -l <path>`, then Fix parent permissions too.
- **No space left**: check `df -h` and `du -xhd1 /`, then Clean logs/cache or expand storage.
- **File busy**: check `lsof <file>`, then Stop process or choose maintenance window.

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For Filesystem And Files, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

Core file commands are stable. Filesystem defaults and supported features can vary by installation profile and storage stack.

## Page Navigation

[Previous](02-package-management-and-repos.md) | [Docs Index](README.md) | [Next](04-users-groups-and-permissions.md)
