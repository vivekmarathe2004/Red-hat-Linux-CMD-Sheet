# Scenario: Disk Full

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

## Interview Answer

“I check filesystem space and inode usage, identify the growing directory with `du`, then clean safely or extend storage depending on the root cause.”

