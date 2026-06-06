# Scenario: Package Repo Issue

> **Scenario** | [Home](../README.md) | [Section Index](README.md) | [Troubleshooting Doc](../docs/15-troubleshooting.md) | [Labs](../labs/README.md)

## What This Usually Means

This symptom tells you one layer of the system is not matching the expected state. Do not guess from the error message alone.

Collect evidence from service state, logs, ports, firewall, SELinux, DNS, storage, and package state until the failing layer is clear.

## Incident Story

A user or monitoring system reports a symptom. Your task is to confirm what is broken, identify the failing layer, fix the smallest safe thing, and prove recovery.

## Symptoms

- `dnf install` says no match found.
- Metadata download fails.
- System has no enabled repositories.
- Package versions are unexpected.

## Likely Causes

- System not registered.
- Required repo disabled.
- Wrong RHEL release lock.
- DNS or proxy problem.
- Third-party repo conflict.

## Decision Flow

Start broad, then narrow down. If the service is not running, read service logs. If it is running but unreachable, check listen address and firewall. If permissions look correct but access fails, check SELinux. If names fail but IPs work, check DNS.

## Diagnostic Flow

```bash
cat /etc/redhat-release
sudo subscription-manager status
sudo subscription-manager release --show
sudo subscription-manager repos --list-enabled
dnf repolist --all
sudo dnf makecache
dnf info <package>
```

## Fix Options

```bash
sudo subscription-manager refresh
sudo subscription-manager repos --enable=<repo-id>
sudo subscription-manager release --unset
sudo dnf clean all
sudo dnf makecache
```

## Verification

```bash
dnf repolist
dnf info <package>
sudo dnf install <package>
```

## What To Remember

A good troubleshooting answer is not just a fix. It explains the evidence that led to the fix and the command used to verify recovery.

## Prevention

After recovery, document the root cause and add a check, note, or monitoring rule that would make the same issue easier to catch next time.

## Interview Answer

"I check registration, enabled repos, release lock, DNS/proxy, and package name before assuming the package does not exist."

## Page Navigation

[Previous](dns-failure.md) | [Scenarios Index](README.md) | Next
