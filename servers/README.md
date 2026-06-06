# Server Recipes

These pages show how to deploy common RHEL server roles safely. Each recipe explains what the service does, which files matter, how to configure it, how to verify it, and how to troubleshoot it.

## Deployment Mindset

Before exposing any service, answer:

- Is the service installed and enabled?
- Is it listening on the expected address and port?
- Is firewalld allowing the traffic?
- Is SELinux allowing the service behavior?
- Do logs show a clean startup?

## Recipes

### Web

- [Apache HTTPD](apache-httpd.md)
- [Nginx](nginx.md)

### Databases

- [MariaDB / MySQL](mariadb-mysql.md)
- [PostgreSQL](postgresql.md)

### Network Services

- [DNS with BIND](dns-bind.md)
- [DHCP](dhcp.md)
- [Chrony / NTP](chrony-ntp.md)

### File Sharing

- [NFS](nfs.md)
- [Samba](samba.md)
- [FTP with vsftpd](ftp-vsftpd.md)

### Operations

- [Mail with Postfix](mail-postfix.md)
- [Rsyslog](rsyslog.md)
- [Cockpit](cockpit.md)
- [IdM / FreeIPA overview](idm-freeipa-overview.md)

## Standard Verification

```bash
systemctl status <service>
sudo ss -tulpn
sudo firewall-cmd --list-all
getenforce
sudo ausearch -m AVC -ts recent
```

