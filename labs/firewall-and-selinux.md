# Lab: Firewall And SELinux

## Objective

Open a service with firewalld and fix SELinux labeling for custom web content.

## Requirements

- RHEL lab VM
- `httpd`
- `policycoreutils-python-utils`

## Tasks

1. Install Apache.
2. Open HTTP in firewalld.
3. Move web content to `/srv/www`.
4. Apply correct SELinux file context.
5. Verify access.

## Commands

```bash
sudo dnf install httpd policycoreutils-python-utils
sudo systemctl enable --now httpd
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --reload

sudo mkdir -p /srv/www
echo "SELinux lab" | sudo tee /srv/www/index.html
sudo semanage fcontext -a -t httpd_sys_content_t "/srv/www(/.*)?"
sudo restorecon -Rv /srv/www
ls -lZ /srv/www
getenforce
sudo ausearch -m AVC -ts recent
```

## Verification

```bash
sudo firewall-cmd --list-all
curl http://localhost
systemctl status httpd
```

## Cleanup

```bash
sudo rm -rf /srv/www
sudo semanage fcontext -d -t httpd_sys_content_t "/srv/www(/.*)?" 2>/dev/null
sudo systemctl disable --now httpd
```

## Interview Takeaway

Firewall controls network entry; SELinux controls what a process can access after it runs.

