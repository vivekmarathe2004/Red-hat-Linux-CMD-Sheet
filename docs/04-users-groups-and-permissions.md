# Users, Groups, And Permissions

## Purpose

Manage local users, groups, passwords, sudo access, file ownership, permissions, and ACLs.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/passwd` | User account records |
| `/etc/shadow` | Password hashes and aging |
| `/etc/group` | Group records |
| `/etc/sudoers` | Main sudo policy |
| `/etc/sudoers.d/` | Drop-in sudo policy files |
| `/etc/login.defs` | Login defaults |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Add user | `sudo useradd <user>` | Creates account |
| Set password | `sudo passwd <user>` | Interactive |
| Delete user | `sudo userdel <user>` | Warning: account removal |
| Delete with home | `sudo userdel -r <user>` | Warning: removes files |
| Add group | `sudo groupadd <group>` | Local group |
| Add to group | `sudo usermod -aG <group> <user>` | User must re-login |
| Lock account | `sudo usermod -L <user>` | Locks password auth |
| Unlock account | `sudo usermod -U <user>` | Unlocks password auth |
| Change owner | `sudo chown <user>:<group> <path>` | File ownership |
| Change mode | `sudo chmod 0640 <file>` | Permission bits |
| Set ACL | `sudo setfacl -m u:<user>:rw <file>` | Extra access |
| View ACL | `getfacl <file>` | ACL details |
| Edit sudo | `sudo visudo` | Syntax checking |

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

## Verify

```bash
id <user>
getent passwd <user>
getent group <group>
sudo -l -U <user>
namei -l <path>
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| User cannot sudo | `sudo -l -U <user>` | Add to `wheel` or sudoers drop-in |
| Group change not active | `id` | Log out and back in |
| ACL ignored | `getfacl <path>` | Check parent dirs and mount options |

## RHEL 9 / RHEL 10 Notes

Local account commands are stable. Enterprise environments often use IdM, LDAP, or AD integration instead of local-only users.

