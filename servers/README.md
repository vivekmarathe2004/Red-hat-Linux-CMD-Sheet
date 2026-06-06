# Server Recipes

Practical setup recipes for common RHEL server roles.

Each recipe follows the same general pattern: install, configure, enable, firewall or SELinux notes, verify, and troubleshoot.

## Web

- [Apache HTTPD](apache-httpd.md)
- [Nginx](nginx.md)

## Databases

- [MariaDB / MySQL](mariadb-mysql.md)
- [PostgreSQL](postgresql.md)

## Network Services

- [DNS with BIND](dns-bind.md)
- [DHCP](dhcp.md)
- [Chrony / NTP](chrony-ntp.md)

## File Sharing

- [NFS](nfs.md)
- [Samba](samba.md)
- [FTP with vsftpd](ftp-vsftpd.md)

## Operations

- [Mail with Postfix](mail-postfix.md)
- [Rsyslog](rsyslog.md)
- [Cockpit](cockpit.md)
- [IdM / FreeIPA overview](idm-freeipa-overview.md)

## Before Exposing A Service

```bash
systemctl status <service>
sudo ss -tulpn
sudo firewall-cmd --list-all
getenforce
sudo ausearch -m AVC -ts recent
```

