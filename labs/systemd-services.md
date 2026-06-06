# Lab: systemd Services

## Scenario Context

Practice this lab on a disposable RHEL VM. Treat it like a small work ticket: understand the goal, make the change, verify it, and clean up after yourself.

By the end, you should be able to explain what changed, where the configuration lives, and how you would troubleshoot the same task if it failed.

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

## Common Lab Mistakes

- Copying placeholders such as `<user>`, `<device>`, or `<service>` without replacing them.
- Forgetting to verify the result after each task.
- Leaving test users, packages, services, or mounts behind after cleanup.
- Practicing only the success path and never checking logs when something fails.

## Interview Takeaway

Be ready to explain `start` vs `enable`, `restart` vs `reload`, and why `journalctl -u <service> -b` is a first troubleshooting command.

