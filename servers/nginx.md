# Nginx

> **Server Recipe** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Install and configure Nginx as a web server or reverse proxy.

## Architecture Notes

Think of this service in layers: package, configuration, systemd unit, listening socket, firewall rule, SELinux policy, logs, and client test. A failure in any layer can look like the service is down.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/nginx/nginx.conf` | Main config |
| `/etc/nginx/conf.d/` | Server blocks |
| `/usr/share/nginx/html/` | Default document root |
| `/var/log/nginx/` | Access and error logs |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Install**: `sudo dnf install nginx` - Nginx package
- **Enable**: `sudo systemctl enable --now nginx` - Start at boot
- **Test config**: `sudo nginx -t` - Syntax check
- **Reload**: `sudo systemctl reload nginx` - Apply config
- **Logs**: `sudo journalctl -u nginx` - Service logs
- **Open HTTP**: `sudo firewall-cmd --add-service=http --permanent` - Firewall

## Safe Change Pattern

Back up config files, validate syntax when a validator exists, reload instead of restart when safe, and test from both localhost and a remote client.

## Configuration Workflow

```bash
sudo dnf install nginx
sudo systemctl enable --now nginx

sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --add-service=https --permanent
sudo firewall-cmd --reload

sudo nginx -t
sudo systemctl reload nginx
```

Reverse proxy example:

```nginx
server {
    listen 80;
    server_name <hostname>;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
    }
}
```

## Verify

```bash
systemctl status nginx
curl -I http://localhost
sudo ss -tulpn | grep ':80'
```

## Common Service Mistakes

- Opening a firewall port before confirming the service is listening.
- Restarting a service before validating the config file.
- Forgetting SELinux labels or booleans for custom paths and proxy behavior.
- Testing only from localhost when the real users connect remotely.

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Config fails**: check `sudo nginx -t`, then Fix syntax.
- **502 bad gateway**: check `ss -tulpn`, then Start backend or fix `proxy_pass`.
- **SELinux blocks proxy**: check `getsebool httpd_can_network_connect`, then `sudo setsebool -P httpd_can_network_connect on`.

## RHEL 9 / RHEL 10 Notes

Nginx availability and stream versions depend on enabled repositories.

## Page Navigation

[Servers Index](README.md) | [Web Lab](../labs/web-server.md) | [Service Scenario](../scenarios/service-down.md)
