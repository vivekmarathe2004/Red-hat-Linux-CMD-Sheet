# Samba

## Purpose

Share files with SMB/CIFS clients such as Windows and Linux systems.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/samba/smb.conf` | Main Samba config |
| `/var/lib/samba/` | Samba state |
| `/var/log/samba/` | Samba logs |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install | `sudo dnf install samba samba-client` | Server and client |
| Enable | `sudo systemctl enable --now smb nmb` | Start services |
| Test config | `testparm` | Syntax and effective config |
| Add SMB user | `sudo smbpasswd -a <user>` | User must exist locally |
| List shares | `smbclient -L //<server> -U <user>` | Client test |
| Open firewall | `sudo firewall-cmd --add-service=samba --permanent` | SMB ports |

## Configuration Workflow

```bash
sudo dnf install samba samba-client
sudo mkdir -p /srv/samba/<share>
sudo chown <user>:<group> /srv/samba/<share>
sudo semanage fcontext -a -t samba_share_t "/srv/samba(/.*)?"
sudo restorecon -Rv /srv/samba

sudo vi /etc/samba/smb.conf
testparm
sudo smbpasswd -a <user>
sudo systemctl enable --now smb nmb
sudo firewall-cmd --add-service=samba --permanent
sudo firewall-cmd --reload
```

Share example:

```ini
[share]
    path = /srv/samba/share
    browseable = yes
    writable = yes
    valid users = <user>
```

## Verify

```bash
testparm
systemctl status smb
smbclient -L //localhost -U <user>
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Login fails | `pdbedit -L` | Add SMB password |
| Access denied | `ls -lZ` | Fix Unix permissions and SELinux context |
| Not visible | Firewall and service | Open Samba and start `smb` |

## RHEL 9 / RHEL 10 Notes

SMB protocol defaults can change for security. Avoid enabling obsolete SMB versions.

