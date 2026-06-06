# Processes, Logs, And Monitoring

## Purpose

Inspect processes, resource usage, logs, sockets, and system health.

## Important Files

| Path | Purpose |
| --- | --- |
| `/var/log/messages` | General system messages |
| `/var/log/secure` | Authentication and security logs |
| `/var/log/audit/audit.log` | Audit and SELinux denials |
| `/run/log/journal/` | Volatile journal storage |
| `/var/log/journal/` | Persistent journal storage if enabled |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Process list | `ps aux` | All processes |
| Process tree | `pstree -p` | Parent-child view |
| Live monitor | `top` | Built in |
| Better monitor | `htop` | Install if available |
| Kill process | `sudo kill <pid>` | Graceful signal |
| Force kill | `sudo kill -9 <pid>` | Last resort |
| Journal boot logs | `journalctl -b` | Current boot |
| Unit logs | `journalctl -u <service>` | Service logs |
| Follow logs | `journalctl -f` | Live stream |
| Failed units | `systemctl --failed` | systemd failures |
| Sockets | `ss -tulpn` | Listening processes |
| Memory | `free -h` | RAM and swap |
| IO | `iostat -xz 1` | Install `sysstat` |

## Configuration Workflow

```bash
# Enable persistent journal logs
sudo mkdir -p /var/log/journal
sudo systemctl restart systemd-journald

# Install common monitoring tools
sudo dnf install sysstat lsof sos

# Enable sysstat collection
sudo systemctl enable --now sysstat
```

## Verify

```bash
journalctl --disk-usage
systemctl status systemd-journald
sar -u 1 3
ss -tulpn
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Service down | `systemctl status <service>` | Read unit logs and restart after fixing |
| High CPU | `top` or `ps -eo pid,ppid,cmd,%cpu --sort=-%cpu` | Identify process |
| Port conflict | `ss -tulpn` | Stop conflicting service or change port |

## RHEL 9 / RHEL 10 Notes

Journal and classic log files can both be present. Some services log primarily to journald.

