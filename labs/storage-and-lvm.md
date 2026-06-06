# Lab: Storage And LVM

## Scenario Context

Practice this lab on a disposable RHEL VM. Treat it like a small work ticket: understand the goal, make the change, verify it, and clean up after yourself.

By the end, you should be able to explain what changed, where the configuration lives, and how you would troubleshoot the same task if it failed.

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

## Common Lab Mistakes

- Copying placeholders such as `<user>`, `<device>`, or `<service>` without replacing them.
- Forgetting to verify the result after each task.
- Leaving test users, packages, services, or mounts behind after cleanup.
- Practicing only the success path and never checking logs when something fails.

## Interview Takeaway

Know PV, VG, LV, filesystem, mountpoint, UUID, and why XFS can grow but not shrink.

