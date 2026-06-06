# DNS With BIND

> **Server Recipe** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Configure BIND as an authoritative or caching DNS server.

## Architecture Notes

Think of this service in layers: package, configuration, systemd unit, listening socket, firewall rule, SELinux policy, logs, and client test. A failure in any layer can look like the service is down.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/named.conf` | Main BIND config |
| `/var/named/` | Zone files |
| `/etc/rndc.key` | RNDC control key |
| `/var/log/messages` | Common named logs |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Install**: `sudo dnf install bind bind-utils` - Server and tools
- **Enable**: `sudo systemctl enable --now named` - Start at boot
- **Check config**: `sudo named-checkconf` - Syntax
- **Check zone**: `sudo named-checkzone <zone> <file>` - Zone validation
- **Query**: `dig @localhost <name>` - DNS test
- **Reload**: `sudo rndc reload` - Reload zones
- **Open firewall**: `sudo firewall-cmd --add-service=dns --permanent` - TCP/UDP 53

## Safe Change Pattern

Back up config files, validate syntax when a validator exists, reload instead of restart when safe, and test from both localhost and a remote client.

## Configuration Workflow

```bash
sudo dnf install bind bind-utils
sudo systemctl enable --now named

sudo named-checkconf
sudo firewall-cmd --add-service=dns --permanent
sudo firewall-cmd --reload
sudo systemctl reload named
```

## Verify

```bash
systemctl status named
dig @localhost <domain>
sudo ss -tulpn | grep ':53'
sudo ss -ulpn | grep ':53'
```

## Common Service Mistakes

- Opening a firewall port before confirming the service is listening.
- Restarting a service before validating the config file.
- Forgetting SELinux labels or booleans for custom paths and proxy behavior.
- Testing only from localhost when the real users connect remotely.

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Zone not loading**: check `named-checkzone`, then Fix serial, SOA, or syntax.
- **Query refused**: check `named.conf` ACLs, then Fix `allow-query`.
- **SELinux file issue**: check `ls -Z /var/named`, then Restore labels.

## RHEL 9 / RHEL 10 Notes

BIND config concepts are stable. Validate all zone files before reload.

## Page Navigation

[Servers Index](README.md) | [Web Lab](../labs/web-server.md) | [Service Scenario](../scenarios/service-down.md)
