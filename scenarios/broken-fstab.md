# Scenario: Broken fstab

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

## Interview Answer

“For fstab boot issues, I compare `/etc/fstab` with `blkid` and `lsblk`, then test with `mount -a` before rebooting.”

