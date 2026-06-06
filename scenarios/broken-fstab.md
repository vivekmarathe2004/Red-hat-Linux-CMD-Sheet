# Scenario: Broken fstab

## What This Usually Means

This symptom tells you one layer of the system is not matching the expected state. Do not guess from the error message alone.

Collect evidence from service state, logs, ports, firewall, SELinux, DNS, storage, and package state until the failing layer is clear.

## Symptoms

- System boots into emergency mode.
- `mount -a` fails.
- A recently added mount does not mount.

## Likely Causes

- Bad UUID.
- Wrong filesystem type.
- Missing mountpoint.
- Syntax error.
- Disk unavailable.

## Decision Flow

Start broad, then narrow down. If the service is not running, read service logs. If it is running but unreachable, check listen address and firewall. If permissions look correct but access fails, check SELinux. If names fail but IPs work, check DNS.

## Diagnostic Flow

```bash
cat /etc/fstab
sudo blkid
lsblk -f
findmnt
sudo mount -a
```

## Fix Options

- Correct the UUID.
- Create the mountpoint.
- Fix filesystem type.
- Add `nofail` only when boot should continue without the mount.

```bash
sudo cp -a /etc/fstab /etc/fstab.bak.$(date +%F-%H%M)
sudo vi /etc/fstab
sudo mount -a
```

## Verification

```bash
findmnt <mountpoint>
df -hT <mountpoint>
systemctl --failed
```

## What To Remember

A good troubleshooting answer is not just a fix. It explains the evidence that led to the fix and the command used to verify recovery.

## Interview Answer

"For fstab boot issues, I compare `/etc/fstab` with `blkid` and `lsblk`, then test with `mount -a` before rebooting."

