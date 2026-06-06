# Filesystem And Files

## Purpose

Navigate, inspect, copy, move, archive, search, and protect files.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/fstab` | Persistent mounts |
| `/etc/updatedb.conf` | `locate` database config |
| `/tmp` | Temporary files |
| `/var/tmp` | Longer-lived temporary files |
| `/home` | User home directories |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| List files | `ls -la` | Include hidden files |
| Print directory | `pwd` | Current path |
| Copy | `cp -a <src> <dst>` | Preserve attributes |
| Move | `mv <src> <dst>` | Rename or move |
| Remove | `rm <path>` | Warning: destructive |
| Create directory | `mkdir -p <dir>` | Creates parents |
| View file | `less <file>` | Pager |
| Tail log | `tail -f <file>` | Follow updates |
| Search text | `grep -R "<text>" <dir>` | Recursive search |
| Find files | `find <dir> -name "<pattern>"` | Flexible search |
| Disk usage | `du -sh <path>` | Human-readable size |
| Filesystem space | `df -h` | Mounted filesystems |
| Archive | `tar -czf <file>.tar.gz <dir>` | Create gzip tarball |
| Extract | `tar -xzf <file>.tar.gz` | Extract archive |

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

## Verify

```bash
ls -ld /opt/<app>
stat /opt/<app>
df -h
du -sh /opt/<app>
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Permission denied | `namei -l <path>` | Fix parent permissions too |
| No space left | `df -h` and `du -xhd1 /` | Clean logs/cache or expand storage |
| File busy | `lsof <file>` | Stop process or choose maintenance window |

## RHEL 9 / RHEL 10 Notes

Core file commands are stable. Filesystem defaults and supported features can vary by installation profile and storage stack.

