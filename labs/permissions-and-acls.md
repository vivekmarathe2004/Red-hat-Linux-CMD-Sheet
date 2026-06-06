# Lab: Permissions And ACLs

> **Lab** | [Home](../README.md) | [Section Index](README.md) | [Docs](../docs/README.md) | [Scenarios](../scenarios/README.md)

## Scenario Context

Practice this lab on a disposable RHEL VM. Treat it like a small work ticket: understand the goal, make the change, verify it, and clean up after yourself.

By the end, you should be able to explain what changed, where the configuration lives, and how you would troubleshoot the same task if it failed.

## Objective

Practice Unix permissions, SGID shared directories, and ACL-based access.

## Why This Lab Matters

This lab turns a common admin task into muscle memory. The important part is not just reaching the final state, but proving that the system is actually configured correctly.

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

## Common Lab Mistakes

- Copying placeholders such as `<user>`, `<device>`, or `<service>` without replacing them.
- Forgetting to verify the result after each task.
- Leaving test users, packages, services, or mounts behind after cleanup.
- Practicing only the success path and never checking logs when something fails.

## Review Questions

After the lab, answer these out loud: what changed, which command changed it, where is it stored, how did you verify it, and what would fail if it were misconfigured?

## Interview Takeaway

Use `namei -l` when permissions look correct on the final file but access still fails.

## Page Navigation

[Previous](users-and-groups.md) | [Labs Index](README.md) | [Next](packages-and-repos.md)
