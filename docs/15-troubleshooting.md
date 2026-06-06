# Troubleshooting

## Purpose

Use a repeatable flow for diagnosing boot, network, service, storage, package, and security issues.

## Important Files

| Path | Purpose |
| --- | --- |
| `/var/log/messages` | General logs |
| `/var/log/secure` | Auth logs |
| `/var/log/audit/audit.log` | Audit and SELinux |
| `/etc/fstab` | Mount failures |
| `/etc/yum.repos.d/` | Repo failures |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Failed services | `systemctl --failed` | First stop |
| Boot logs | `journalctl -b` | Current boot |
| Previous boot | `journalctl -b -1` | Last boot |
| Critical logs | `journalctl -p err -b` | Errors |
| Kernel ring | `dmesg -T` | Hardware/kernel |
| Network | `ip addr; ip route` | Address and route |
| DNS | `dig <name>` | Resolver test |
| Ports | `ss -tulpn` | Listening services |
| Disk | `lsblk -f; df -hT` | Storage state |
| SELinux denials | `ausearch -m AVC -ts recent` | Access denials |
| Package check | `dnf check` | Dependency health |

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

## RHEL 9 / RHEL 10 Notes

Use the same diagnostic flow on both versions. Interpret results using the package and service versions installed on the target system.

