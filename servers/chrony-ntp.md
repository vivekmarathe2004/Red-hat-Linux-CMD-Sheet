# Chrony / NTP

> **Server Recipe** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Synchronize system time with Chrony and optionally serve time to clients.

## Architecture Notes

Think of this service in layers: package, configuration, systemd unit, listening socket, firewall rule, SELinux policy, logs, and client test. A failure in any layer can look like the service is down.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/chrony.conf` | Main Chrony config |
| `/var/lib/chrony/` | Chrony state |
| `/var/log/chrony/` | Chrony logs if enabled |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Install**: `sudo dnf install chrony` - Time sync
- **Enable**: `sudo systemctl enable --now chronyd` - Start at boot
- **Sources**: `chronyc sources -v` - Time sources
- **Tracking**: `chronyc tracking` - Sync status
- **Step clock**: `sudo chronyc makestep` - Correct large offset
- **Open NTP**: `sudo firewall-cmd --add-service=ntp --permanent` - If serving clients

## Safe Change Pattern

Back up config files, validate syntax when a validator exists, reload instead of restart when safe, and test from both localhost and a remote client.

## Configuration Workflow

```bash
sudo dnf install chrony
sudo vi /etc/chrony.conf
sudo systemctl enable --now chronyd
sudo systemctl restart chronyd
```

Server pool example:

```text
pool <ntp-pool-or-server> iburst
```

Allow clients example:

```text
allow <client-cidr>
```

## Verify

```bash
timedatectl
chronyc tracking
chronyc sources -v
```

## Common Service Mistakes

- Opening a firewall port before confirming the service is listening.
- Restarting a service before validating the config file.
- Forgetting SELinux labels or booleans for custom paths and proxy behavior.
- Testing only from localhost when the real users connect remotely.

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Unsynchronized**: check `chronyc sources -v`, then Fix NTP source/firewall.
- **Large offset**: check `chronyc tracking`, then Use `chronyc makestep` in maintenance.
- **Clients cannot sync**: check Firewall, then Open NTP and add `allow` CIDR.

## RHEL 9 / RHEL 10 Notes

Chrony is the preferred NTP implementation on RHEL.

## Page Navigation

[Servers Index](README.md) | [Web Lab](../labs/web-server.md) | [Service Scenario](../scenarios/service-down.md)
