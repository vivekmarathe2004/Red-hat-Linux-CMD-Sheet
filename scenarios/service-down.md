# Scenario: Service Down

## What This Usually Means

This symptom tells you one layer of the system is not matching the expected state. Do not guess from the error message alone.

Collect evidence from service state, logs, ports, firewall, SELinux, DNS, storage, and package state until the failing layer is clear.

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

## Decision Flow

Start broad, then narrow down. If the service is not running, read service logs. If it is running but unreachable, check listen address and firewall. If permissions look correct but access fails, check SELinux. If names fail but IPs work, check DNS.

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

## What To Remember

A good troubleshooting answer is not just a fix. It explains the evidence that led to the fix and the command used to verify recovery.

## Interview Answer

"I first check `systemctl status` and service logs, then validate config, check dependencies, ports, firewall, SELinux, and permissions before restarting."

