# Lab: Troubleshooting Drill

> **Lab** | [Home](../README.md) | [Section Index](README.md) | [Docs](../docs/README.md) | [Scenarios](../scenarios/README.md)

## Scenario Context

Practice this lab on a disposable RHEL VM. Treat it like a small work ticket: understand the goal, make the change, verify it, and clean up after yourself.

By the end, you should be able to explain what changed, where the configuration lives, and how you would troubleshoot the same task if it failed.

## Objective

Practice a repeatable troubleshooting flow instead of guessing.

## Why This Lab Matters

This lab turns a common admin task into muscle memory. The important part is not just reaching the final state, but proving that the system is actually configured correctly.

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

## Common Lab Mistakes

- Copying placeholders such as `<user>`, `<device>`, or `<service>` without replacing them.
- Forgetting to verify the result after each task.
- Leaving test users, packages, services, or mounts behind after cleanup.
- Practicing only the success path and never checking logs when something fails.

## Review Questions

After the lab, answer these out loud: what changed, which command changed it, where is it stored, how did you verify it, and what would fail if it were misconfigured?

## Interview Takeaway

In interviews, describe your diagnostic order before commands. That shows judgment, not memorization.

## Page Navigation

[Previous](database-server.md) | [Labs Index](README.md) | Next
