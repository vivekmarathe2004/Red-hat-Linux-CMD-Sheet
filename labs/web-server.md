# Lab: Web Server

## Scenario Context

Practice this lab on a disposable RHEL VM. Treat it like a small work ticket: understand the goal, make the change, verify it, and clean up after yourself.

By the end, you should be able to explain what changed, where the configuration lives, and how you would troubleshoot the same task if it failed.

## Objective

Deploy a basic HTTP service and troubleshoot access.

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

## Interview Takeaway

For web access issues, check service, listen port, firewall, SELinux, content permissions, and logs.

