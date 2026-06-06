# Configuration Files

> **Cheatsheet** | [Home](../README.md) | [Section Index](README.md) | [Docs](../docs/README.md)

## How To Use This Sheet

Use this as a quick lookup after you understand the related concept. Tables are kept here because speed matters, but production work still requires verification and careful placeholder replacement.

## Read Before Copying

Cheatsheets are fast, but they are not a substitute for understanding. Replace placeholders, check the target host, and verify every change.

## Purpose

Quick lookup for service configuration files and validation commands.

| Service | Main Config | Validation |
| --- | --- | --- |
| systemd | `/etc/systemd/system/` | `sudo systemctl daemon-reload` |
| SSH | `/etc/ssh/sshd_config` | `sudo sshd -t` |
| firewalld | `/etc/firewalld/` | `sudo firewall-cmd --check-config` |
| SELinux | `/etc/selinux/config` | `getenforce` |
| NetworkManager | `/etc/NetworkManager/system-connections/` | `nmcli connection show` |
| Apache | `/etc/httpd/conf/httpd.conf` | `sudo apachectl configtest` |
| Nginx | `/etc/nginx/nginx.conf` | `sudo nginx -t` |
| MariaDB | `/etc/my.cnf` | `sudo systemctl restart mariadb` |
| PostgreSQL | `/var/lib/pgsql/data/postgresql.conf` | `sudo -iu postgres psql -c "SHOW config_file;"` |
| BIND | `/etc/named.conf` | `sudo named-checkconf` |
| DHCP | `/etc/dhcp/dhcpd.conf` | `sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf` |
| NFS | `/etc/exports` | `sudo exportfs -rav` |
| Samba | `/etc/samba/smb.conf` | `testparm` |
| vsftpd | `/etc/vsftpd/vsftpd.conf` | `sudo systemctl restart vsftpd` |
| Postfix | `/etc/postfix/main.cf` | `sudo postfix check` |
| Chrony | `/etc/chrony.conf` | `chronyc tracking` |
| Rsyslog | `/etc/rsyslog.conf` | `sudo rsyslogd -N1` |

## Safe Edit Pattern

```bash
sudo cp -a <config-file> <config-file>.bak.$(date +%F)
sudo vi <config-file>
sudo <validation-command>
sudo systemctl reload <service> || sudo systemctl restart <service>
```

## Page Navigation

[Cheatsheets Index](README.md) | [Core Docs](../docs/README.md)
