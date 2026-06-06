# Lab: Firewall And SELinux

## Scenario Context

Practice this lab on a disposable RHEL VM. Treat it like a small work ticket: understand the goal, make the change, verify it, and clean up after yourself.

By the end, you should be able to explain what changed, where the configuration lives, and how you would troubleshoot the same task if it failed.

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

## Common Lab Mistakes

- Copying placeholders such as `<user>`, `<device>`, or `<service>` without replacing them.
- Forgetting to verify the result after each task.
- Leaving test users, packages, services, or mounts behind after cleanup.
- Practicing only the success path and never checking logs when something fails.

## Interview Takeaway

Firewall controls network entry; SELinux controls what a process can access after it runs.

