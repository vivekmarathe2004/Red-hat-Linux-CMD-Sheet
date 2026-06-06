# Scenario: DNS Failure

> **Scenario** | [Home](../README.md) | [Section Index](README.md) | [Troubleshooting Doc](../docs/15-troubleshooting.md) | [Labs](../labs/README.md)

## What This Usually Means

This symptom tells you one layer of the system is not matching the expected state. Do not guess from the error message alone.

Collect evidence from service state, logs, ports, firewall, SELinux, DNS, storage, and package state until the failing layer is clear.

## Incident Story

A user or monitoring system reports a symptom. Your task is to confirm what is broken, identify the failing layer, fix the smallest safe thing, and prove recovery.

## Symptoms

- Ping by IP works, but hostname fails.
- Package installs fail due to name resolution.
- Applications cannot resolve service names.

## Likely Causes

- Wrong DNS server.
- NetworkManager profile DNS misconfigured.
- Broken `/etc/resolv.conf`.
- DNS server unreachable.
- Search domain issue.

## Decision Flow

Start broad, then narrow down. If the service is not running, read service logs. If it is running but unreachable, check listen address and firewall. If permissions look correct but access fails, check SELinux. If names fail but IPs work, check DNS.

## Diagnostic Flow

```bash
ip addr
ip route
nmcli connection show
nmcli device show <interface>
cat /etc/resolv.conf
getent hosts <hostname>
dig <hostname>
dig @<dns-server> <hostname>
```

## Fix Options

Set DNS through NetworkManager:

```bash
sudo nmcli connection modify <connection> ipv4.dns "<dns1> <dns2>"
sudo nmcli connection up <connection>
```

## Verification

```bash
getent hosts redhat.com
dig redhat.com
sudo dnf makecache
```

## What To Remember

A good troubleshooting answer is not just a fix. It explains the evidence that led to the fix and the command used to verify recovery.

## Prevention

After recovery, document the root cause and add a check, note, or monitoring rule that would make the same issue easier to catch next time.

## Interview Answer

"I separate network reachability from DNS resolution: first IP route/connectivity, then resolver config, then direct queries to the DNS server."

## Page Navigation

[Previous](broken-fstab.md) | [Scenarios Index](README.md) | [Next](package-repo-issue.md)
