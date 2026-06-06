# Lab: systemd Services

## Objective

Manage services, read logs, enable boot startup, and inspect unit files.

## Requirements

- RHEL lab VM
- Any safe service such as `crond` or `sshd`

## Tasks

1. Check service status.
2. Stop and start a service.
3. Enable the service at boot.
4. Read service logs.
5. Inspect the unit file.

## Commands

```bash
systemctl status crond
sudo systemctl stop crond
systemctl status crond
sudo systemctl start crond
sudo systemctl enable crond
systemctl is-enabled crond
journalctl -u crond -b --no-pager
systemctl cat crond
systemctl --failed
```

## Verification

```bash
systemctl is-active crond
systemctl is-enabled crond
```

## Cleanup

```bash
sudo systemctl enable --now crond
```

## Interview Takeaway

Be ready to explain `start` vs `enable`, `restart` vs `reload`, and why `journalctl -u <service> -b` is a first troubleshooting command.

