# FTP With vsftpd

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Install a basic FTP service with vsftpd where FTP is explicitly required.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/vsftpd/vsftpd.conf` | Main config |
| `/etc/vsftpd/user_list` | User allow/deny list depending on config |
| `/var/ftp/` | Common anonymous FTP root |
| `/var/log/xferlog` | Transfer log when enabled |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install | `sudo dnf install vsftpd` | FTP server |
| Enable | `sudo systemctl enable --now vsftpd` | Start at boot |
| Test config | `sudo systemctl restart vsftpd` | No built-in full validator |
| Open firewall | `sudo firewall-cmd --add-service=ftp --permanent` | FTP control |
| Logs | `sudo journalctl -u vsftpd` | Service logs |

## Configuration Workflow

```bash
sudo dnf install vsftpd
sudo cp -a /etc/vsftpd/vsftpd.conf /etc/vsftpd/vsftpd.conf.bak
sudo vi /etc/vsftpd/vsftpd.conf
sudo systemctl enable --now vsftpd
sudo firewall-cmd --add-service=ftp --permanent
sudo firewall-cmd --reload
```

For local user upload directories:

```bash
sudo setsebool -P ftpd_full_access on
```

Use this boolean only when the access model is understood.

## Verify

```bash
systemctl status vsftpd
sudo ss -tulpn | grep ':21'
ftp <server>
```

## Common Service Mistakes

- Opening a firewall port before confirming the service is listening.
- Restarting a service before validating the config file.
- Forgetting SELinux labels or booleans for custom paths and proxy behavior.
- Testing only from localhost when the real users connect remotely.

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Login denied | `/etc/vsftpd/user_list` | Check allow/deny behavior |
| Passive mode fails | Firewall/NAT | Configure passive ports and firewall |
| SELinux denial | `ausearch -m AVC -ts recent` | Use correct labels/booleans |

## RHEL 9 / RHEL 10 Notes

Prefer SFTP over FTP unless FTP is required for compatibility.

