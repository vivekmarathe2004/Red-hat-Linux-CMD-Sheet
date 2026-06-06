# Package Management And Repos

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Install, update, remove, search, and manage packages and repositories with `dnf`.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/yum.repos.d/` | Repository files |
| `/var/cache/dnf/` | DNF cache |
| `/etc/dnf/dnf.conf` | Main DNF config |
| `/var/log/dnf.log` | DNF log |
| `/var/log/dnf.rpm.log` | RPM transaction log |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Update metadata | `sudo dnf makecache` | Refresh cache |
| Search | `dnf search <term>` | Searches names and summaries |
| Show package | `dnf info <package>` | Package details |
| Install | `sudo dnf install <package>` | Installs dependencies |
| Remove | `sudo dnf remove <package>` | Review removals carefully |
| Update all | `sudo dnf update` | Applies updates |
| List repos | `dnf repolist --all` | Shows enabled and disabled |
| List installed | `dnf list installed` | Installed RPMs |
| Find owner | `rpm -qf <path>` | Which package owns a file |
| Package files | `rpm -ql <package>` | File list |
| History | `dnf history` | Transaction history |
| Undo transaction | `sudo dnf history undo <id>` | Not always possible |

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

## Verify

```bash
dnf repolist
dnf check
rpm -Va
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Package unavailable | `dnf repolist --all` | Enable required repo |
| Broken dependencies | `dnf repoquery --unsatisfied` | Fix repo mix or remove conflict |
| Bad metadata | `sudo dnf clean all` | Rebuild cache |

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For Package Management And Repos, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

Both use `dnf`. Avoid legacy `yum` examples unless documenting compatibility; on modern RHEL, `yum` is a compatibility entry point for DNF behavior.

