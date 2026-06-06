# Mail With Postfix

> **Server Recipe** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Configure Postfix for local delivery, relay, or simple SMTP sending.

## Architecture Notes

Think of this service in layers: package, configuration, systemd unit, listening socket, firewall rule, SELinux policy, logs, and client test. A failure in any layer can look like the service is down.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/postfix/main.cf` | Main Postfix config |
| `/etc/postfix/master.cf` | Service definitions |
| `/var/log/maillog` | Mail logs |
| `/etc/aliases` | Local aliases |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Install**: `sudo dnf install postfix mailx` - MTA and test client
- **Enable**: `sudo systemctl enable --now postfix` - Start at boot
- **Check config**: `sudo postfix check` - Basic validation
- **Reload**: `sudo systemctl reload postfix` - Apply config
- **Queue**: `mailq` - Mail queue
- **Flush queue**: `sudo postfix flush` - Retry delivery
- **Aliases**: `sudo newaliases` - Rebuild alias db

## Safe Change Pattern

Back up config files, validate syntax when a validator exists, reload instead of restart when safe, and test from both localhost and a remote client.

## Configuration Workflow

```bash
sudo dnf install postfix mailx
sudo postconf -e "myhostname = <hostname>"
sudo postconf -e "myorigin = $myhostname"
sudo postconf -e "inet_interfaces = localhost"
sudo postfix check
sudo systemctl enable --now postfix
```

Relay example:

```bash
sudo postconf -e "relayhost = [<smtp-relay-host>]:587"
sudo systemctl reload postfix
```

## Verify

```bash
systemctl status postfix
echo "test" | mail -s "test mail" <user>@<domain>
mailq
sudo tail -f /var/log/maillog
```

## Common Service Mistakes

- Opening a firewall port before confirming the service is listening.
- Restarting a service before validating the config file.
- Forgetting SELinux labels or booleans for custom paths and proxy behavior.
- Testing only from localhost when the real users connect remotely.

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Mail stuck**: check `mailq`, then Check DNS, relay, auth, firewall.
- **Config error**: check `postfix check`, then Fix `main.cf`.
- **Remote SMTP blocked**: check `ss -tulpn` and network policy, then Use approved relay.

## RHEL 9 / RHEL 10 Notes

Many environments block direct outbound SMTP. Use an approved relay where required.

## Page Navigation

[Servers Index](README.md) | [Web Lab](../labs/web-server.md) | [Service Scenario](../scenarios/service-down.md)
