# Lab: Web Server

> **Lab** | [Home](../README.md) | [Section Index](README.md) | [Docs](../docs/README.md) | [Scenarios](../scenarios/README.md)

## Scenario Context

Practice this lab on a disposable RHEL VM. Treat it like a small work ticket: understand the goal, make the change, verify it, and clean up after yourself.

By the end, you should be able to explain what changed, where the configuration lives, and how you would troubleshoot the same task if it failed.

## Objective

Deploy a basic HTTP service and troubleshoot access.

## Why This Lab Matters

This lab turns a common admin task into muscle memory. The important part is not just reaching the final state, but proving that the system is actually configured correctly.

## Requirements

- RHEL lab VM
- Apache or Nginx

## Tasks

1. Install Apache.
2. Add a test page.
3. Start and enable the service.
4. Open firewall access.
5. Verify local and remote access.

## Commands

```bash
sudo dnf install httpd
echo "RHEL web lab" | sudo tee /var/www/html/index.html
sudo systemctl enable --now httpd
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --reload
sudo apachectl configtest
curl http://localhost
```

## Verification

```bash
systemctl status httpd
sudo ss -tulpn | grep ':80'
sudo firewall-cmd --list-all
sudo tail -f /var/log/httpd/access_log
```

## Cleanup

```bash
sudo systemctl disable --now httpd
sudo dnf remove httpd
```

## Common Lab Mistakes

- Copying placeholders such as `<user>`, `<device>`, or `<service>` without replacing them.
- Forgetting to verify the result after each task.
- Leaving test users, packages, services, or mounts behind after cleanup.
- Practicing only the success path and never checking logs when something fails.

## Review Questions

After the lab, answer these out loud: what changed, which command changed it, where is it stored, how did you verify it, and what would fail if it were misconfigured?

## Interview Takeaway

For web access issues, check service, listen port, firewall, SELinux, content permissions, and logs.

## Page Navigation

[Previous](containers-podman.md) | [Labs Index](README.md) | [Next](database-server.md)
