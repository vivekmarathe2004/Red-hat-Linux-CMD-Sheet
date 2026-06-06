# Apache HTTPD

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Install and configure Apache HTTP Server for static or application-backed web service.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/httpd/conf/httpd.conf` | Main config |
| `/etc/httpd/conf.d/` | Virtual host and app configs |
| `/var/www/html/` | Default document root |
| `/var/log/httpd/` | Access and error logs |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install | `sudo dnf install httpd` | Apache package |
| Enable | `sudo systemctl enable --now httpd` | Start at boot |
| Test config | `sudo apachectl configtest` | Syntax check |
| Reload | `sudo systemctl reload httpd` | Graceful config reload |
| Logs | `sudo journalctl -u httpd` | systemd logs |
| Open firewall | `sudo firewall-cmd --add-service=http --permanent` | HTTP |
| Open HTTPS | `sudo firewall-cmd --add-service=https --permanent` | HTTPS |

## Configuration Workflow

```bash
sudo dnf install httpd
sudo systemctl enable --now httpd

echo "RHEL web server" | sudo tee /var/www/html/index.html

sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --reload

sudo apachectl configtest
sudo systemctl reload httpd
```

For custom content outside `/var/www`:

```bash
sudo semanage fcontext -a -t httpd_sys_content_t "/srv/www(/.*)?"
sudo restorecon -Rv /srv/www
```

## Verify

```bash
systemctl status httpd
curl http://localhost
sudo ss -tulpn | grep ':80'
sudo tail -f /var/log/httpd/access_log
```

## Common Service Mistakes

- Opening a firewall port before confirming the service is listening.
- Restarting a service before validating the config file.
- Forgetting SELinux labels or booleans for custom paths and proxy behavior.
- Testing only from localhost when the real users connect remotely.

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| 403 forbidden | `ls -lZ <docroot>` | Fix permissions and SELinux labels |
| Port in use | `ss -tulpn | grep ':80'` | Stop conflicting service |
| Remote access fails | `firewall-cmd --list-all` | Open `http` or `https` |

## RHEL 9 / RHEL 10 Notes

Apache package and service names remain `httpd`. Module versions can differ.

