# Rsyslog

## Purpose

Collect, route, and forward system logs.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/rsyslog.conf` | Main config |
| `/etc/rsyslog.d/` | Drop-in configs |
| `/var/log/messages` | General logs |
| `/var/log/secure` | Auth logs |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install | `sudo dnf install rsyslog` | Logging daemon |
| Enable | `sudo systemctl enable --now rsyslog` | Start at boot |
| Test config | `sudo rsyslogd -N1` | Validate config |
| Reload | `sudo systemctl restart rsyslog` | Apply config |
| Send test | `logger "test message"` | Local test |
| Follow logs | `sudo tail -f /var/log/messages` | Watch logs |

## Configuration Workflow

Forward all logs to a central server:

```bash
echo "*.* @@<log-server>:514" | sudo tee /etc/rsyslog.d/90-forward.conf
sudo rsyslogd -N1
sudo systemctl restart rsyslog
```

Receive logs:

```bash
sudo vi /etc/rsyslog.d/10-listen.conf
sudo firewall-cmd --add-port=514/tcp --permanent
sudo firewall-cmd --reload
sudo systemctl restart rsyslog
```

Listener example:

```text
module(load="imtcp")
input(type="imtcp" port="514")
```

## Verify

```bash
systemctl status rsyslog
sudo rsyslogd -N1
logger "rsyslog test"
sudo tail -f /var/log/messages
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Config bad | `rsyslogd -N1` | Fix syntax |
| Forwarding fails | Network and firewall | Open TCP/UDP 514 as designed |
| No logs | `systemctl status rsyslog` | Start service and check rules |

## RHEL 9 / RHEL 10 Notes

Rsyslog and journald often work together. Some service logs may be available first in `journalctl`.

