# Ports And Services

## How To Use This Sheet

Use this as a quick lookup after you understand the related concept. Tables are kept here because speed matters, but production work still requires verification and careful placeholder replacement.

## Purpose

Common service names, ports, packages, and firewall services.

| Role | Package | systemd Service | Port | Firewall Service |
| --- | --- | --- | --- | --- |
| SSH | `openssh-server` | `sshd` | 22/tcp | `ssh` |
| HTTPD | `httpd` | `httpd` | 80/tcp, 443/tcp | `http`, `https` |
| Nginx | `nginx` | `nginx` | 80/tcp, 443/tcp | `http`, `https` |
| MariaDB | `mariadb-server` | `mariadb` | 3306/tcp | `mysql` |
| PostgreSQL | `postgresql-server` | `postgresql` | 5432/tcp | `postgresql` |
| DNS | `bind` | `named` | 53/tcp, 53/udp | `dns` |
| DHCP | `dhcp-server` | `dhcpd` | 67/udp | `dhcp` |
| NFS | `nfs-utils` | `nfs-server` | 2049/tcp | `nfs` |
| Samba | `samba` | `smb`, `nmb` | 445/tcp, 139/tcp | `samba` |
| FTP | `vsftpd` | `vsftpd` | 21/tcp | `ftp` |
| SMTP | `postfix` | `postfix` | 25/tcp | `smtp` |
| NTP | `chrony` | `chronyd` | 123/udp | `ntp` |
| Cockpit | `cockpit` | `cockpit.socket` | 9090/tcp | `cockpit` |
| Rsyslog TCP | `rsyslog` | `rsyslog` | 514/tcp | Custom port |
| Rsyslog UDP | `rsyslog` | `rsyslog` | 514/udp | Custom port |

## Useful Checks

```bash
sudo ss -tulpn
sudo firewall-cmd --get-services
sudo firewall-cmd --list-all
systemctl status <service>
```

