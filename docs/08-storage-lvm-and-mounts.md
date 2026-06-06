# Storage, LVM, And Mounts

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

## RHEL 9 / RHEL 10 Notes

XFS is commonly used for RHEL filesystems. XFS can grow online but cannot be shrunk.

