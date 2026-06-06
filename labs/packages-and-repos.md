# Lab: Packages And Repositories

> **Lab** | [Home](../README.md) | [Section Index](README.md) | [Docs](../docs/README.md) | [Scenarios](../scenarios/README.md)

## Scenario Context

Practice this lab on a disposable RHEL VM. Treat it like a small work ticket: understand the goal, make the change, verify it, and clean up after yourself.

By the end, you should be able to explain what changed, where the configuration lives, and how you would troubleshoot the same task if it failed.

## Objective

Use DNF and RPM to install, inspect, remove, and troubleshoot packages.

## Why This Lab Matters

This lab turns a common admin task into muscle memory. The important part is not just reaching the final state, but proving that the system is actually configured correctly.

## Requirements

- Registered RHEL lab VM or access to enabled repositories

## Tasks

1. Check enabled repos.
2. Search for a package.
3. Install a tool.
4. Find package files.
5. Find which package owns a command.
6. Review transaction history.

## Commands

```bash
sudo subscription-manager status
dnf repolist --all
dnf search bind-utils
sudo dnf install bind-utils
rpm -q bind-utils
rpm -ql bind-utils
rpm -qf /usr/bin/dig
dnf history
```

## Verification

```bash
command -v dig
dig redhat.com
dnf info bind-utils
```

## Cleanup

```bash
sudo dnf remove bind-utils
```

## Common Lab Mistakes

- Copying placeholders such as `<user>`, `<device>`, or `<service>` without replacing them.
- Forgetting to verify the result after each task.
- Leaving test users, packages, services, or mounts behind after cleanup.
- Practicing only the success path and never checking logs when something fails.

## Review Questions

After the lab, answer these out loud: what changed, which command changed it, where is it stored, how did you verify it, and what would fail if it were misconfigured?

## Interview Takeaway

Explain why `dnf` is preferred for installs and dependency resolution, while `rpm` is useful for local package queries.

## Page Navigation

[Previous](permissions-and-acls.md) | [Labs Index](README.md) | [Next](systemd-services.md)
