# DHCP

> **Server Recipe** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Configure a DHCP server for IPv4 address assignment.

## Architecture Notes

Think of this service in layers: package, configuration, systemd unit, listening socket, firewall rule, SELinux policy, logs, and client test. A failure in any layer can look like the service is down.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/dhcp/dhcpd.conf` | DHCP server config |
| `/var/lib/dhcpd/dhcpd.leases` | Lease database |
| `/etc/sysconfig/dhcpd` | Interface options when used |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Install**: `sudo dnf install dhcp-server` - Server package
- **Enable**: `sudo systemctl enable --now dhcpd` - Start at boot
- **Test config**: `sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf` - Syntax
- **Logs**: `sudo journalctl -u dhcpd` - Service logs
- **Open firewall**: `sudo firewall-cmd --add-service=dhcp --permanent` - DHCP service

## Safe Change Pattern

Back up config files, validate syntax when a validator exists, reload instead of restart when safe, and test from both localhost and a remote client.

## Configuration Workflow

```bash
sudo dnf install dhcp-server
sudo vi /etc/dhcp/dhcpd.conf
sudo dhcpd -t -cf /etc/dhcp/dhcpd.conf
sudo systemctl enable --now dhcpd
sudo firewall-cmd --add-service=dhcp --permanent
sudo firewall-cmd --reload
```

Minimal subnet example:

```text
subnet 192.0.2.0 netmask 255.255.255.0 {
  range 192.0.2.100 192.0.2.200;
  option routers 192.0.2.1;
  option domain-name-servers 192.0.2.53;
  default-lease-time 600;
  max-lease-time 7200;
}
```

## Verify

```bash
systemctl status dhcpd
sudo journalctl -u dhcpd -f
sudo cat /var/lib/dhcpd/dhcpd.leases
```

## Common Service Mistakes

- Opening a firewall port before confirming the service is listening.
- Restarting a service before validating the config file.
- Forgetting SELinux labels or booleans for custom paths and proxy behavior.
- Testing only from localhost when the real users connect remotely.

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Service fails**: check `dhcpd -t -cf /etc/dhcp/dhcpd.conf`, then Fix syntax.
- **No leases**: check Network segment, then Confirm server is on correct subnet/VLAN.
- **Firewall blocks**: check `firewall-cmd --list-all`, then Add DHCP service.

## RHEL 9 / RHEL 10 Notes

Package and service names can vary by DHCP implementation. Confirm installed service with `rpm -ql dhcp-server | grep systemd`.

## Page Navigation

[Servers Index](README.md) | [Web Lab](../labs/web-server.md) | [Service Scenario](../scenarios/service-down.md)
