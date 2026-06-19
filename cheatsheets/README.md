# Cheatsheets

> **Cheatsheet Index** | [Home](../README.md) | [Section Index](../README.md)

Cheatsheets are intentionally compact. Use them after you understand the concept and need a fast lookup.

## How To Use This Section

If you are new to a topic, read the matching tutorial page in [docs](../docs/README.md) first. Then come back here to memorize commands, paths, ports, and config checks.

## Good Cheatsheet Use

- Use commands here as lookup, not as a replacement for diagnosis.
- Confirm the target host, service, device, path, and release first.
- Prefer commands that verify state before commands that change state.
- When RHEL 9 and RHEL 10 differ, check the target release and package version.

## Quick Lookup

- [Command index](command-index.md): commands grouped by admin task.
- [File paths](file-paths.md): important system and service paths.
- [Ports and services](ports-and-services.md): service names, ports, packages, and firewall names.
- [Configuration files](config-files.md): config files and validation commands.
- [RHEL 9 vs RHEL 10 notes](rhel9-vs-rhel10-notes.md): version reminders.

## Best Practice

Do not copy a command only because it appears in a cheatsheet. Read the note beside it, replace placeholders, and verify the result.

## Before Copying To Production

```bash
hostnamectl
cat /etc/redhat-release
systemctl status <service>
sudo ss -tulpn
sudo firewall-cmd --list-all
getenforce
```

## Page Navigation

[Home](../README.md)
