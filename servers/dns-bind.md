# DNS With BIND

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Configure BIND as an authoritative or caching DNS server.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/named.conf` | Main BIND config |
| `/var/named/` | Zone files |
| `/etc/rndc.key` | RNDC control key |
| `/var/log/messages` | Common named logs |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install | `sudo dnf install bind bind-utils` | Server and tools |
| Enable | `sudo systemctl enable --now named` | Start at boot |
| Check config | `sudo named-checkconf` | Syntax |
| Check zone | `sudo named-checkzone <zone> <file>` | Zone validation |
| Query | `dig @localhost <name>` | DNS test |
| Reload | `sudo rndc reload` | Reload zones |
| Open firewall | `sudo firewall-cmd --add-service=dns --permanent` | TCP/UDP 53 |

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

| Problem | Check | Fix |
| --- | --- | --- |
| Zone not loading | `named-checkzone` | Fix serial, SOA, or syntax |
| Query refused | `named.conf` ACLs | Fix `allow-query` |
| SELinux file issue | `ls -Z /var/named` | Restore labels |

## RHEL 9 / RHEL 10 Notes

BIND config concepts are stable. Validate all zone files before reload.

