# Lab: Packages And Repositories

## Scenario Context

Practice this lab on a disposable RHEL VM. Treat it like a small work ticket: understand the goal, make the change, verify it, and clean up after yourself.

By the end, you should be able to explain what changed, where the configuration lives, and how you would troubleshoot the same task if it failed.

## Objective

Use DNF and RPM to install, inspect, remove, and troubleshoot packages.

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

## Interview Takeaway

Explain why `dnf` is preferred for installs and dependency resolution, while `rpm` is useful for local package queries.

