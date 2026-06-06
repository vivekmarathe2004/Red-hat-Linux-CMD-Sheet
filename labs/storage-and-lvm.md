# Lab: Storage And LVM

## Objective

Create, mount, and extend an LVM logical volume.

## Requirements

- Disposable extra disk or virtual disk
- Root access

> Warning: Storage commands can destroy data. Use only a lab disk.

## Tasks

1. Identify the lab disk.
2. Create a physical volume, volume group, and logical volume.
3. Create an XFS filesystem.
4. Mount persistently by UUID.
5. Extend the logical volume.

## Commands

```bash
lsblk -f
sudo pvcreate <device>
sudo vgcreate <vg> <device>
sudo lvcreate -n <lv> -L <size> <vg>
sudo mkfs.xfs /dev/<vg>/<lv>
sudo mkdir -p <mountpoint>
sudo mount /dev/<vg>/<lv> <mountpoint>
sudo blkid /dev/<vg>/<lv>
sudo vi /etc/fstab
sudo mount -a
sudo lvextend -r -L +<size> /dev/<vg>/<lv>
```

## Verification

```bash
sudo pvs
sudo vgs
sudo lvs
df -hT <mountpoint>
findmnt <mountpoint>
```

## Cleanup

```bash
sudo umount <mountpoint>
sudo vi /etc/fstab
sudo lvremove /dev/<vg>/<lv>
sudo vgremove <vg>
sudo pvremove <device>
```

## Interview Takeaway

Know PV, VG, LV, filesystem, mountpoint, UUID, and why XFS can grow but not shrink.

