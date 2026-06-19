# Server Recipes

> **Server Index** | [Home](../README.md) | [Section Index](../README.md)

These pages show how to deploy common RHEL server roles safely. Each recipe explains what the service does, which files matter, how to configure it, how to verify it, and how to troubleshoot it.

## Deployment Mindset

Before exposing any service, answer:

- Is the service installed and enabled?
- Is it listening on the expected address and port?
- Is firewalld allowing the traffic?
- Is SELinux allowing the service behavior?
- Do logs show a clean startup?

## Standard Service Flow

Use the same rhythm for every recipe:

1. Install the package.
2. Enable and start the service.
3. Validate config syntax if the service provides a checker.
4. Confirm the process is listening.
5. Open the firewall only for the required service or port.
6. Check SELinux labels, booleans, or port types if access fails.
7. Verify locally and from a second host.

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
journalctl -u <service> -b --no-pager
sudo ss -tulpn
sudo firewall-cmd --list-all
getenforce
sudo ausearch -m AVC -ts recent
```

## Common Failure Split

| Symptom | First Check | Next Check |
| --- | --- | --- |
| Service will not start | `systemctl status <service>` | `journalctl -u <service> -b` |
| Works locally, not remotely | `sudo ss -tulpn` | firewalld, route, external firewall |
| Permission denied | `namei -l <path>` | `ls -lZ <path>`, AVC denials |
| Config change ignored | service config test | reload vs restart, included config path |

## Page Navigation

[Home](../README.md)
