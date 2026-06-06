# Rsyslog

> **Server Recipe** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Collect, route, and forward system logs.

## Architecture Notes

Think of this service in layers: package, configuration, systemd unit, listening socket, firewall rule, SELinux policy, logs, and client test. A failure in any layer can look like the service is down.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/rsyslog.conf` | Main config |
| `/etc/rsyslog.d/` | Drop-in configs |
| `/var/log/messages` | General logs |
| `/var/log/secure` | Auth logs |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Install**: `sudo dnf install rsyslog` - Logging daemon
- **Enable**: `sudo systemctl enable --now rsyslog` - Start at boot
- **Test config**: `sudo rsyslogd -N1` - Validate config
- **Reload**: `sudo systemctl restart rsyslog` - Apply config
- **Send test**: `logger "test message"` - Local test
- **Follow logs**: `sudo tail -f /var/log/messages` - Watch logs

## Safe Change Pattern

Back up config files, validate syntax when a validator exists, reload instead of restart when safe, and test from both localhost and a remote client.

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

## Common Service Mistakes

- Opening a firewall port before confirming the service is listening.
- Restarting a service before validating the config file.
- Forgetting SELinux labels or booleans for custom paths and proxy behavior.
- Testing only from localhost when the real users connect remotely.

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Config bad**: check `rsyslogd -N1`, then Fix syntax.
- **Forwarding fails**: check Network and firewall, then Open TCP/UDP 514 as designed.
- **No logs**: check `systemctl status rsyslog`, then Start service and check rules.

## RHEL 9 / RHEL 10 Notes

Rsyslog and journald often work together. Some service logs may be available first in `journalctl`.

## Page Navigation

[Servers Index](README.md) | [Web Lab](../labs/web-server.md) | [Service Scenario](../scenarios/service-down.md)
