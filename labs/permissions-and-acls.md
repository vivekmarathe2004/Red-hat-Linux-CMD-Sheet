# Lab: Permissions And ACLs

## Objective

Practice Unix permissions, SGID shared directories, and ACL-based access.

## Requirements

- Two test users
- One test group

## Tasks

1. Create a shared directory under `/srv`.
2. Set group ownership and SGID.
3. Give one extra user access with ACL.
4. Diagnose permissions through the full path.

## Commands

```bash
sudo groupadd <projectgroup>
sudo useradd <user1>
sudo useradd <user2>
sudo usermod -aG <projectgroup> <user1>

sudo mkdir -p /srv/<project>
sudo chgrp <projectgroup> /srv/<project>
sudo chmod 2770 /srv/<project>
sudo setfacl -m u:<user2>:rwx /srv/<project>

ls -ld /srv/<project>
getfacl /srv/<project>
namei -l /srv/<project>
```

## Verification

```bash
sudo -u <user1> touch /srv/<project>/user1.txt
sudo -u <user2> touch /srv/<project>/user2.txt
ls -l /srv/<project>
```

## Cleanup

```bash
sudo rm -rf /srv/<project>
sudo userdel -r <user1>
sudo userdel -r <user2>
sudo groupdel <projectgroup>
```

## Interview Takeaway

Use `namei -l` when permissions look correct on the final file but access still fails.

