# Users, Groups, And Permissions

> **Core Doc** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Manage local users, groups, passwords, sudo access, file ownership, permissions, and ACLs.

## Why It Matters

This topic affects real server behavior. If you can explain the purpose, inspect the current state, make a safe change, and verify it, you are doing administrator work rather than memorizing syntax.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/passwd` | User account records |
| `/etc/shadow` | Password hashes and aging |
| `/etc/group` | Group records |
| `/etc/sudoers` | Main sudo policy |
| `/etc/sudoers.d/` | Drop-in sudo policy files |
| `/etc/login.defs` | Login defaults |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Add user**: `sudo useradd <user>` - Creates account
- **Set password**: `sudo passwd <user>` - Interactive
- **Delete user**: `sudo userdel <user>` - Warning: account removal
- **Delete with home**: `sudo userdel -r <user>` - Warning: removes files
- **Add group**: `sudo groupadd <group>` - Local group
- **Add to group**: `sudo usermod -aG <group> <user>` - User must re-login
- **Lock account**: `sudo usermod -L <user>` - Locks password auth
- **Unlock account**: `sudo usermod -U <user>` - Unlocks password auth
- **Change owner**: `sudo chown <user>:<group> <path>` - File ownership
- **Change mode**: `sudo chmod 0640 <file>` - Permission bits
- **Set ACL**: `sudo setfacl -m u:<user>:rw <file>` - Extra access
- **View ACL**: `getfacl <file>` - ACL details
- **Edit sudo**: `sudo visudo` - Syntax checking

## Configuration Workflow

```bash
# Create admin user

sudo useradd <user>
sudo passwd <user>
sudo usermod -aG wheel <user>

# Give a service group access to a directory

sudo groupadd <appgroup>
sudo chgrp <appgroup> /srv/<app>
sudo chmod 2770 /srv/<app>
```

## Try It In A VM

Run the workflow on a disposable RHEL VM. Change one setting, verify the result, then undo or document what changed. This builds the habit you need for production systems.

## Verify

```bash
id <user>
getent passwd <user>
getent group <group>
sudo -l -U <user>
namei -l <path>
```

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **User cannot sudo**: check `sudo -l -U <user>`, then Add to `wheel` or sudoers drop-in.
- **Group change not active**: check `id`, then Log out and back in.
- **ACL ignored**: check `getfacl <path>`, then Check parent dirs and mount options.

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For Users, Groups, And Permissions, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

Local account commands are stable. Enterprise environments often use IdM, LDAP, or AD integration instead of local-only users.

## Page Navigation

[Previous](03-filesystem-and-files.md) | [Docs Index](README.md) | [Next](05-systemd-services-and-boot.md)
