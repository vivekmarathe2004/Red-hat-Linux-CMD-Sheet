# Cockpit

> **Server Recipe** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Enable Cockpit web console for browser-based system administration.

## Architecture Notes

Think of this service in layers: package, configuration, systemd unit, listening socket, firewall rule, SELinux policy, logs, and client test. A failure in any layer can look like the service is down.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/cockpit/` | Cockpit configuration |
| `/usr/share/cockpit/` | Cockpit modules |
| `/var/log/messages` | General logs |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Install**: `sudo dnf install cockpit` - Web console
- **Enable socket**: `sudo systemctl enable --now cockpit.socket` - Socket activation
- **Open firewall**: `sudo firewall-cmd --add-service=cockpit --permanent` - Port 9090
- **Status**: `systemctl status cockpit.socket` - Socket state
- **Logs**: `journalctl -u cockpit` - Service logs

## Safe Change Pattern

Back up config files, validate syntax when a validator exists, reload instead of restart when safe, and test from both localhost and a remote client.

## Configuration Workflow

```bash
sudo dnf install cockpit
sudo systemctl enable --now cockpit.socket
sudo firewall-cmd --add-service=cockpit --permanent
sudo firewall-cmd --reload
```

Access:

```text
https://<server>:9090
```

## Verify

```bash
systemctl status cockpit.socket
sudo ss -tulpn | grep ':9090'
curl -k https://localhost:9090
```

## Common Service Mistakes

- Opening a firewall port before confirming the service is listening.
- Restarting a service before validating the config file.
- Forgetting SELinux labels or booleans for custom paths and proxy behavior.
- Testing only from localhost when the real users connect remotely.

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Browser cannot connect**: check Firewall and socket, then Open cockpit service and start socket.
- **Login denied**: check User and PAM, then Confirm account and sudo policy.
- **Certificate warning**: check Browser, then Install trusted certificate if needed.

## RHEL 9 / RHEL 10 Notes

Cockpit modules available depend on installed packages and repositories.

## Page Navigation

[Servers Index](README.md) | [Web Lab](../labs/web-server.md) | [Service Scenario](../scenarios/service-down.md)
