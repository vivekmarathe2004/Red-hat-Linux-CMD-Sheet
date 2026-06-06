# Mail With Postfix

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Configure Postfix for local delivery, relay, or simple SMTP sending.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/postfix/main.cf` | Main Postfix config |
| `/etc/postfix/master.cf` | Service definitions |
| `/var/log/maillog` | Mail logs |
| `/etc/aliases` | Local aliases |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install | `sudo dnf install postfix mailx` | MTA and test client |
| Enable | `sudo systemctl enable --now postfix` | Start at boot |
| Check config | `sudo postfix check` | Basic validation |
| Reload | `sudo systemctl reload postfix` | Apply config |
| Queue | `mailq` | Mail queue |
| Flush queue | `sudo postfix flush` | Retry delivery |
| Aliases | `sudo newaliases` | Rebuild alias db |

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

| Problem | Check | Fix |
| --- | --- | --- |
| Mail stuck | `mailq` | Check DNS, relay, auth, firewall |
| Config error | `postfix check` | Fix `main.cf` |
| Remote SMTP blocked | `ss -tulpn` and network policy | Use approved relay |

## RHEL 9 / RHEL 10 Notes

Many environments block direct outbound SMTP. Use an approved relay where required.

