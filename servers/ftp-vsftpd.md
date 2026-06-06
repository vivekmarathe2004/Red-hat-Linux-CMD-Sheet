# FTP With vsftpd

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

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Login denied | `/etc/vsftpd/user_list` | Check allow/deny behavior |
| Passive mode fails | Firewall/NAT | Configure passive ports and firewall |
| SELinux denial | `ausearch -m AVC -ts recent` | Use correct labels/booleans |

## RHEL 9 / RHEL 10 Notes

Prefer SFTP over FTP unless FTP is required for compatibility.

