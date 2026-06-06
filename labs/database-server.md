# Lab: Database Server

> **Lab** | [Home](../README.md) | [Section Index](README.md) | [Docs](../docs/README.md) | [Scenarios](../scenarios/README.md)

## Scenario Context

Practice this lab on a disposable RHEL VM. Treat it like a small work ticket: understand the goal, make the change, verify it, and clean up after yourself.

By the end, you should be able to explain what changed, where the configuration lives, and how you would troubleshoot the same task if it failed.

## Objective

Install and verify a basic database service.

## Why This Lab Matters

This lab turns a common admin task into muscle memory. The important part is not just reaching the final state, but proving that the system is actually configured correctly.

## Requirements

- RHEL lab VM
- Choose MariaDB or PostgreSQL

## Tasks

1. Install one database server.
2. Start and enable it.
3. Create a test database.
4. Verify local access.
5. Review service logs.

## MariaDB Commands

```bash
sudo dnf install mariadb-server
sudo systemctl enable --now mariadb
sudo mariadb-secure-installation
sudo mariadb -e "CREATE DATABASE labdb;"
sudo mariadb -e "SHOW DATABASES;"
sudo journalctl -u mariadb -b --no-pager
```

## PostgreSQL Commands

```bash
sudo dnf install postgresql-server postgresql
sudo postgresql-setup --initdb
sudo systemctl enable --now postgresql
sudo -iu postgres psql -c "CREATE DATABASE labdb;"
sudo -iu postgres psql -c "\\l"
sudo journalctl -u postgresql -b --no-pager
```

## Verification

```bash
systemctl status mariadb || systemctl status postgresql
sudo ss -tulpn | grep -E '3306|5432'
```

## Cleanup

```bash
sudo systemctl disable --now mariadb 2>/dev/null
sudo systemctl disable --now postgresql 2>/dev/null
```

## Common Lab Mistakes

- Copying placeholders such as `<user>`, `<device>`, or `<service>` without replacing them.
- Forgetting to verify the result after each task.
- Leaving test users, packages, services, or mounts behind after cleanup.
- Practicing only the success path and never checking logs when something fails.

## Review Questions

After the lab, answer these out loud: what changed, which command changed it, where is it stored, how did you verify it, and what would fail if it were misconfigured?

## Interview Takeaway

Database remote access usually requires service listen config, database auth rules, firewall rules, and SELinux awareness.

## Page Navigation

[Previous](web-server.md) | [Labs Index](README.md) | [Next](troubleshooting-drill.md)
