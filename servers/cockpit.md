# Cockpit

## Purpose

Enable Cockpit web console for browser-based system administration.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/cockpit/` | Cockpit configuration |
| `/usr/share/cockpit/` | Cockpit modules |
| `/var/log/messages` | General logs |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install | `sudo dnf install cockpit` | Web console |
| Enable socket | `sudo systemctl enable --now cockpit.socket` | Socket activation |
| Open firewall | `sudo firewall-cmd --add-service=cockpit --permanent` | Port 9090 |
| Status | `systemctl status cockpit.socket` | Socket state |
| Logs | `journalctl -u cockpit` | Service logs |

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

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Browser cannot connect | Firewall and socket | Open cockpit service and start socket |
| Login denied | User and PAM | Confirm account and sudo policy |
| Certificate warning | Browser | Install trusted certificate if needed |

## RHEL 9 / RHEL 10 Notes

Cockpit modules available depend on installed packages and repositories.

