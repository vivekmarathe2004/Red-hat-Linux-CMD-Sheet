# Scenario: Service Down

## Symptoms

- Application is unavailable.
- `systemctl --failed` shows a failed unit.
- Users report connection errors.

## Likely Causes

- Bad configuration syntax.
- Missing dependency.
- Permission or SELinux issue.
- Port conflict.
- Required mount or network unavailable.

## Diagnostic Flow

```bash
systemctl status <service>
journalctl -u <service> -b --no-pager
systemctl cat <service>
systemctl list-dependencies <service>
sudo ss -tulpn
getenforce
sudo ausearch -m AVC -ts recent
```

## Fix Options

- Run the service-specific config validation command.
- Restore the last known good config backup.
- Fix permissions or SELinux labels.
- Stop the process using the required port.
- Restart after validation.

```bash
sudo systemctl restart <service>
systemctl status <service>
```

## Verification

```bash
systemctl is-active <service>
journalctl -u <service> -b --no-pager
```

## Interview Answer

“I first check `systemctl status` and service logs, then validate config, check dependencies, ports, firewall, SELinux, and permissions before restarting.”

