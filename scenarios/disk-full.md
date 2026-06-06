# Scenario: Disk Full

> **Scenario** | [Home](../README.md) | [Section Index](README.md) | [Troubleshooting Doc](../docs/15-troubleshooting.md) | [Labs](../labs/README.md)

## What This Usually Means

This symptom tells you one layer of the system is not matching the expected state. Do not guess from the error message alone.

Collect evidence from service state, logs, ports, firewall, SELinux, DNS, storage, and package state until the failing layer is clear.

## Incident Story

A user or monitoring system reports a symptom. Your task is to confirm what is broken, identify the failing layer, fix the smallest safe thing, and prove recovery.

## Symptoms

- Applications fail to write files.
- Logins or services fail.
- `No space left on device` errors.

## Likely Causes

- Large logs.
- Package cache growth.
- Application uploads.
- Database growth.
- Wrong mount layout.
- Inode exhaustion.

## Decision Flow

Start broad, then narrow down. If the service is not running, read service logs. If it is running but unreachable, check listen address and firewall. If permissions look correct but access fails, check SELinux. If names fail but IPs work, check DNS.

## Diagnostic Flow

```bash
df -hT
df -ih
sudo du -xhd1 /
sudo du -xhd1 /var
sudo journalctl --disk-usage
lsblk -f
findmnt
```

## Fix Options

- Rotate or truncate excessive logs safely.
- Clean DNF cache.
- Move or archive application data.
- Extend filesystem or logical volume.

```bash
sudo dnf clean all
sudo journalctl --vacuum-time=7d
sudo lvextend -r -L +<size> /dev/<vg>/<lv>
```

## Verification

```bash
df -hT
df -ih
systemctl --failed
```

## What To Remember

A good troubleshooting answer is not just a fix. It explains the evidence that led to the fix and the command used to verify recovery.

## Prevention

After recovery, document the root cause and add a check, note, or monitoring rule that would make the same issue easier to catch next time.

## Interview Answer

"I check filesystem space and inode usage, identify the growing directory with `du`, then clean safely or extend storage depending on the root cause."

## Page Navigation

[Previous](web-403-502.md) | [Scenarios Index](README.md) | [Next](broken-fstab.md)
