# Lab: Troubleshooting Drill

## Objective

Practice a repeatable troubleshooting flow instead of guessing.

## Requirements

- RHEL lab VM
- One intentionally broken service or config

## Tasks

1. Check system health.
2. Check failed units.
3. Read logs.
4. Check network and ports.
5. Check firewall and SELinux.
6. Fix the root cause.

## Commands

```bash
hostnamectl
cat /etc/redhat-release
systemctl --failed
journalctl -p err -b --no-pager
systemctl status <service>
journalctl -u <service> -b --no-pager
ip addr
ip route
sudo ss -tulpn
sudo firewall-cmd --list-all
getenforce
sudo ausearch -m AVC -ts recent
df -hT
lsblk -f
```

## Verification

```bash
systemctl is-active <service>
curl http://localhost:<port>
systemctl --failed
```

## Cleanup

Undo the intentional break you created.

## Interview Takeaway

In interviews, describe your diagnostic order before commands. That shows judgment, not memorization.

