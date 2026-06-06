# Firewall And SELinux

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Manage host firewall rules with firewalld and enforce access control with SELinux.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/firewalld/` | Firewalld persistent config |
| `/usr/lib/firewalld/services/` | Packaged service definitions |
| `/etc/selinux/config` | SELinux mode at boot |
| `/var/log/audit/audit.log` | SELinux AVC denials |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Firewall state | `sudo firewall-cmd --state` | firewalld runtime |
| List zone | `sudo firewall-cmd --list-all` | Default zone |
| List services | `sudo firewall-cmd --get-services` | Known services |
| Add service | `sudo firewall-cmd --add-service=<service> --permanent` | Persistent |
| Add port | `sudo firewall-cmd --add-port=<port>/<proto> --permanent` | Persistent |
| Reload firewall | `sudo firewall-cmd --reload` | Apply permanent rules |
| SELinux mode | `getenforce` | Enforcing, Permissive, Disabled |
| Set permissive now | `sudo setenforce 0` | Temporary |
| Restore labels | `sudo restorecon -Rv <path>` | Fix contexts |
| List booleans | `getsebool -a` | SELinux booleans |
| Set boolean | `sudo setsebool -P <boolean> on` | Persistent |
| Add port context | `sudo semanage port -a -t <type> -p tcp <port>` | Needs policycoreutils tools |

## Configuration Workflow

```bash
# Open a service safely
sudo firewall-cmd --add-service=<service> --permanent
sudo firewall-cmd --reload

# Install SELinux tools if semanage is missing
sudo dnf install policycoreutils-python-utils

# Fix web content labels
sudo semanage fcontext -a -t httpd_sys_content_t "/srv/www(/.*)?"
sudo restorecon -Rv /srv/www
```

## Verify

```bash
sudo firewall-cmd --list-all
getenforce
sudo ausearch -m AVC -ts recent
ls -Z <path>
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Service works locally but not remotely | `firewall-cmd --list-all` | Add service or port |
| SELinux blocks service | `ausearch -m AVC -ts recent` | Fix label, boolean, or port type |
| `semanage` missing | `command -v semanage` | Install `policycoreutils-python-utils` |

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For Firewall And SELinux, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

Keep SELinux enforcing when possible. Use labels, booleans, and policy tools instead of disabling SELinux.

