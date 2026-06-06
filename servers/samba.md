# Samba

> **Server Recipe** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Share files with SMB/CIFS clients such as Windows and Linux systems.

## Architecture Notes

Think of this service in layers: package, configuration, systemd unit, listening socket, firewall rule, SELinux policy, logs, and client test. A failure in any layer can look like the service is down.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/samba/smb.conf` | Main Samba config |
| `/var/lib/samba/` | Samba state |
| `/var/log/samba/` | Samba logs |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Install**: `sudo dnf install samba samba-client` - Server and client
- **Enable**: `sudo systemctl enable --now smb nmb` - Start services
- **Test config**: `testparm` - Syntax and effective config
- **Add SMB user**: `sudo smbpasswd -a <user>` - User must exist locally
- **List shares**: `smbclient -L //<server> -U <user>` - Client test
- **Open firewall**: `sudo firewall-cmd --add-service=samba --permanent` - SMB ports

## Safe Change Pattern

Back up config files, validate syntax when a validator exists, reload instead of restart when safe, and test from both localhost and a remote client.

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

## Common Service Mistakes

- Opening a firewall port before confirming the service is listening.
- Restarting a service before validating the config file.
- Forgetting SELinux labels or booleans for custom paths and proxy behavior.
- Testing only from localhost when the real users connect remotely.

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Login fails**: check `pdbedit -L`, then Add SMB password.
- **Access denied**: check `ls -lZ`, then Fix Unix permissions and SELinux context.
- **Not visible**: check Firewall and service, then Open Samba and start `smb`.

## RHEL 9 / RHEL 10 Notes

SMB protocol defaults can change for security. Avoid enabling obsolete SMB versions.

## Page Navigation

[Servers Index](README.md) | [Web Lab](../labs/web-server.md) | [Service Scenario](../scenarios/service-down.md)
