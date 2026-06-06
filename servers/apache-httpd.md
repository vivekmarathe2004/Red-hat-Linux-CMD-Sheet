# Apache HTTPD

> **Server Recipe** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Install and configure Apache HTTP Server for static or application-backed web service.

## Architecture Notes

Think of this service in layers: package, configuration, systemd unit, listening socket, firewall rule, SELinux policy, logs, and client test. A failure in any layer can look like the service is down.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/httpd/conf/httpd.conf` | Main config |
| `/etc/httpd/conf.d/` | Virtual host and app configs |
| `/var/www/html/` | Default document root |
| `/var/log/httpd/` | Access and error logs |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Install**: `sudo dnf install httpd` - Apache package
- **Enable**: `sudo systemctl enable --now httpd` - Start at boot
- **Test config**: `sudo apachectl configtest` - Syntax check
- **Reload**: `sudo systemctl reload httpd` - Graceful config reload
- **Logs**: `sudo journalctl -u httpd` - systemd logs
- **Open firewall**: `sudo firewall-cmd --add-service=http --permanent` - HTTP
- **Open HTTPS**: `sudo firewall-cmd --add-service=https --permanent` - HTTPS

## Safe Change Pattern

Back up config files, validate syntax when a validator exists, reload instead of restart when safe, and test from both localhost and a remote client.

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

Work from the symptom to evidence, then to the smallest safe fix.

- **403 forbidden**: check `ls -lZ <docroot>`, then Fix permissions and SELinux labels.
- **Port in use**: check `ss -tulpn, then grep ':80'`.
- **Remote access fails**: check `firewall-cmd --list-all`, then Open `http` or `https`.

## RHEL 9 / RHEL 10 Notes

Apache package and service names remain `httpd`. Module versions can differ.

## Page Navigation

[Servers Index](README.md) | [Web Lab](../labs/web-server.md) | [Service Scenario](../scenarios/service-down.md)
