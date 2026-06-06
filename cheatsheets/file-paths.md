# File Paths

## How To Use This Sheet

Use this as a quick lookup after you understand the related concept. Tables are kept here because speed matters, but production work still requires verification and careful placeholder replacement.

## Purpose

Quick lookup for common RHEL filesystem locations.

| Path | Purpose |
| --- | --- |
| `/etc/` | System configuration |
| `/etc/redhat-release` | RHEL release text |
| `/etc/os-release` | OS metadata |
| `/etc/yum.repos.d/` | Repository files |
| `/etc/systemd/system/` | Local systemd unit files and overrides |
| `/usr/lib/systemd/system/` | Package-provided systemd units |
| `/etc/NetworkManager/system-connections/` | NetworkManager profiles |
| `/etc/ssh/sshd_config` | SSH server config |
| `/etc/firewalld/` | Persistent firewall config |
| `/etc/selinux/config` | SELinux boot mode |
| `/etc/fstab` | Persistent filesystem mounts |
| `/etc/passwd` | User accounts |
| `/etc/shadow` | Password hashes and aging |
| `/etc/group` | Groups |
| `/etc/sudoers` | Sudo policy |
| `/etc/sudoers.d/` | Sudo drop-ins |
| `/var/log/` | Logs |
| `/var/log/messages` | General system messages |
| `/var/log/secure` | Authentication logs |
| `/var/log/audit/audit.log` | Audit and SELinux logs |
| `/var/www/html/` | Apache default document root |
| `/usr/share/nginx/html/` | Nginx default document root |
| `/var/lib/mysql/` | MariaDB data |
| `/var/lib/pgsql/data/` | PostgreSQL data |
| `/var/named/` | BIND zone files |
| `/srv/` | Site-specific service data |
| `/opt/` | Optional application software |
| `/usr/local/bin/` | Local scripts and tools |
| `/tmp/` | Temporary files |
| `/var/tmp/` | Longer-lived temporary files |

## Dangerous Paths

Review carefully before deleting or formatting anything under these paths:

| Path | Risk |
| --- | --- |
| `/boot` | Bootloader and kernels |
| `/etc` | System config |
| `/home` | User data |
| `/root` | Root user data |
| `/var/lib` | Application and database state |
| `/dev` | Device nodes |

