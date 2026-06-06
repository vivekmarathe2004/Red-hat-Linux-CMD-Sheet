# Nginx

## Purpose

Install and configure Nginx as a web server or reverse proxy.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/nginx/nginx.conf` | Main config |
| `/etc/nginx/conf.d/` | Server blocks |
| `/usr/share/nginx/html/` | Default document root |
| `/var/log/nginx/` | Access and error logs |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install | `sudo dnf install nginx` | Nginx package |
| Enable | `sudo systemctl enable --now nginx` | Start at boot |
| Test config | `sudo nginx -t` | Syntax check |
| Reload | `sudo systemctl reload nginx` | Apply config |
| Logs | `sudo journalctl -u nginx` | Service logs |
| Open HTTP | `sudo firewall-cmd --add-service=http --permanent` | Firewall |

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

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Config fails | `sudo nginx -t` | Fix syntax |
| 502 bad gateway | `ss -tulpn` | Start backend or fix `proxy_pass` |
| SELinux blocks proxy | `getsebool httpd_can_network_connect` | `sudo setsebool -P httpd_can_network_connect on` |

## RHEL 9 / RHEL 10 Notes

Nginx availability and stream versions depend on enabled repositories.

