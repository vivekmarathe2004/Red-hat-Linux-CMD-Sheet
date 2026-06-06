# Lab: Web Server

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

## Interview Takeaway

For web access issues, check service, listen port, firewall, SELinux, content permissions, and logs.

