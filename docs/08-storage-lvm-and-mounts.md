# Storage, LVM, And Mounts

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Inspect disks, create filesystems, configure LVM, and persist mounts.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/fstab` | Persistent mount table |
| `/etc/lvm/lvm.conf` | LVM configuration |
| `/dev/mapper/` | Device mapper paths |
| `/proc/mounts` | Current mounts |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| List block devices | `lsblk -f` | Filesystems and UUIDs |
| Disk usage | `df -hT` | Type and space |
| Partition tool | `sudo parted <device>` | Destructive if misused |
| Create XFS | `sudo mkfs.xfs <device>` | Warning: erases data |
| Create ext4 | `sudo mkfs.ext4 <device>` | Warning: erases data |
| Mount now | `sudo mount <device> <mountpoint>` | Temporary unless in fstab |
| Show PVs | `sudo pvs` | LVM physical volumes |
| Show VGs | `sudo vgs` | LVM volume groups |
| Show LVs | `sudo lvs` | Logical volumes |
| Create PV | `sudo pvcreate <device>` | Warning: overwrites metadata |
| Create VG | `sudo vgcreate <vg> <device>` | LVM group |
| Create LV | `sudo lvcreate -n <lv> -L <size> <vg>` | Logical volume |
| Extend LV | `sudo lvextend -r -L +<size> /dev/<vg>/<lv>` | Grow LV and filesystem |

## Configuration Workflow

```bash
# Warning: this destroys existing data on <device>
sudo pvcreate <device>
sudo vgcreate <vg> <device>
sudo lvcreate -n <lv> -L <size> <vg>
sudo mkfs.xfs /dev/<vg>/<lv>

sudo mkdir -p <mountpoint>
sudo mount /dev/<vg>/<lv> <mountpoint>

# Persist by UUID
sudo blkid /dev/<vg>/<lv>
sudo vi /etc/fstab
sudo mount -a
```

Example `/etc/fstab` line:

```text
UUID=<uuid> <mountpoint> xfs defaults 0 0
```

## Verify

```bash
lsblk -f
df -hT <mountpoint>
findmnt <mountpoint>
sudo lvs
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Boot mount failure | `sudo mount -a` | Fix `/etc/fstab` syntax or UUID |
| Cannot grow filesystem | `df -T` | Use filesystem-appropriate grow command |
| Device missing | `lsblk` | Check hypervisor, SAN, multipath, or rescan |

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For Storage, LVM, And Mounts, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

XFS is commonly used for RHEL filesystems. XFS can grow online but cannot be shrunk.

