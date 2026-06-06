# Firewall And SELinux

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

## RHEL 9 / RHEL 10 Notes

Keep SELinux enforcing when possible. Use labels, booleans, and policy tools instead of disabling SELinux.

