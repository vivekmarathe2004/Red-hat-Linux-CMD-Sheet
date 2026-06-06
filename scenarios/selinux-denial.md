# Scenario: SELinux Denial

## What This Usually Means

This symptom tells you one layer of the system is not matching the expected state. Do not guess from the error message alone.

Collect evidence from service state, logs, ports, firewall, SELinux, DNS, storage, and package state until the failing layer is clear.

## Symptoms

- Permissions look correct but service still cannot access a file or port.
- Service works when SELinux is permissive.
- Audit logs show AVC messages.

## Likely Causes

- Wrong file context.
- Missing SELinux boolean.
- Non-standard port lacks correct SELinux port type.
- Custom path not labeled for the service.

## Decision Flow

Start broad, then narrow down. If the service is not running, read service logs. If it is running but unreachable, check listen address and firewall. If permissions look correct but access fails, check SELinux. If names fail but IPs work, check DNS.

## Diagnostic Flow

```bash
getenforce
ls -lZ <path>
ps -eZ | grep <process>
sudo ausearch -m AVC -ts recent
getsebool -a | grep <service>
```

## Fix Options

Restore default labels:

```bash
sudo restorecon -Rv <path>
```

Add a persistent file context:

```bash
sudo semanage fcontext -a -t <type> "<path-regex>"
sudo restorecon -Rv <path>
```

Set a boolean:

```bash
sudo setsebool -P <boolean> on
```

Add a port mapping:

```bash
sudo semanage port -a -t <type> -p tcp <port>
```

## Verification

```bash
sudo ausearch -m AVC -ts recent
systemctl restart <service>
systemctl status <service>
```

## What To Remember

A good troubleshooting answer is not just a fix. It explains the evidence that led to the fix and the command used to verify recovery.

## Interview Answer

"I do not disable SELinux as a fix. I use AVC logs to identify whether the problem is a label, boolean, port type, or policy issue."

