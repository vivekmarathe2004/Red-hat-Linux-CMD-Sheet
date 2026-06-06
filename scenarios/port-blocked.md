# Scenario: Port Blocked

> **Scenario** | [Home](../README.md) | [Section Index](README.md) | [Troubleshooting Doc](../docs/15-troubleshooting.md) | [Labs](../labs/README.md)

## What This Usually Means

This symptom tells you one layer of the system is not matching the expected state. Do not guess from the error message alone.

Collect evidence from service state, logs, ports, firewall, SELinux, DNS, storage, and package state until the failing layer is clear.

## Incident Story

A user or monitoring system reports a symptom. Your task is to confirm what is broken, identify the failing layer, fix the smallest safe thing, and prove recovery.

## Symptoms

- Service works locally but remote clients cannot connect.
- `curl localhost:<port>` works.
- Remote connection times out.

## Likely Causes

- Firewalld rule missing.
- Service listening only on `127.0.0.1`.
- Network ACL or cloud security group.
- Wrong zone or interface assignment.

## Decision Flow

Start broad, then narrow down. If the service is not running, read service logs. If it is running but unreachable, check listen address and firewall. If permissions look correct but access fails, check SELinux. If names fail but IPs work, check DNS.

## Diagnostic Flow

```bash
sudo ss -tulpn | grep <port>
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --list-all
ip addr
ip route
```

## Fix Options

Open a known service:

```bash
sudo firewall-cmd --add-service=<service> --permanent
sudo firewall-cmd --reload
```

Open a custom port:

```bash
sudo firewall-cmd --add-port=<port>/<proto> --permanent
sudo firewall-cmd --reload
```

If the service listens only on loopback, fix the service bind/listen configuration.

## Verification

```bash
sudo firewall-cmd --list-all
sudo ss -tulpn | grep <port>
curl http://<server>:<port>
```

## What To Remember

A good troubleshooting answer is not just a fix. It explains the evidence that led to the fix and the command used to verify recovery.

## Prevention

After recovery, document the root cause and add a check, note, or monitoring rule that would make the same issue easier to catch next time.

## Interview Answer

"If localhost works but remote access fails, I check listen address first, then firewall zone/rules, routing, and any external network controls."

## Page Navigation

[Previous](service-down.md) | [Scenarios Index](README.md) | [Next](ssh-lockout.md)
