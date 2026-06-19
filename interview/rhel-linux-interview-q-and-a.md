# RHEL Linux Interview Questions And Answers

> **Interview** | [Home](../README.md) | [Section Index](README.md) | [Scenarios](../scenarios/README.md) | [Labs](../labs/README.md)

This guide is written for spoken interview practice. Do not memorize only the command. Learn to explain what you check first, why you check it, and how you verify the fix.

Related practice:

- [Hands-on labs](../labs/README.md)
- [Troubleshooting scenarios](../scenarios/README.md)
- [Command index](../cheatsheets/command-index.md)

## Answer Format

Use this pattern for scenario answers:

```text
First I check the current state.
The command I use is ...
If the output shows ...
Then I check ...
After fixing, I verify with ...
```

## Core Linux Basics

### How do you check the RHEL version?

Use:

```bash
cat /etc/redhat-release
cat /etc/os-release
```

Short answer: "`/etc/redhat-release` gives the simple RHEL release, and `/etc/os-release` gives structured OS information useful for scripts."

### How do you check the running kernel?

```bash
uname -r
```

Short answer: "`uname -r` shows the kernel currently running. It may differ from the newest installed kernel if the system has not rebooted."

### What is the difference between `/tmp` and `/var/tmp`?

`/tmp` is for short-lived temporary files. `/var/tmp` is usually preserved longer across reboots. Applications should not store important permanent data in either location.

### What is the root user?

The root user is the superuser. It can change almost anything on the system, so daily administration should normally use `sudo` for accountability and safer control.

## Files And Permissions

### How do you troubleshoot a permission denied error?

Start with the full path, not only the final file:

```bash
namei -l <path>
ls -l <path>
getfacl <path>
ls -lZ <path>
```

Spoken answer: "I check every parent directory with `namei -l`, then normal permissions, ACLs, and SELinux context."

### What does execute permission mean on a directory?

Execute on a directory means the user can enter or traverse it. A file may look readable, but access can still fail if a parent directory lacks execute permission.

### What are SUID, SGID, and sticky bit?

- SUID runs an executable with the file owner's privileges.
- SGID runs with group privileges on files and makes new files inherit the directory group on directories.
- Sticky bit on shared directories lets users delete only their own files.

### What are ACLs?

ACLs add permissions beyond owner, group, and others.

```bash
getfacl <path>
setfacl -m u:<user>:rw <path>
```

## Users, Groups, And Sudo

### How do you add a user and give sudo access?

```bash
sudo useradd <user>
sudo passwd <user>
sudo usermod -aG wheel <user>
sudo -l -U <user>
```

Spoken answer: "I add the user, set a password, add them to `wheel`, then verify sudo privileges with `sudo -l -U`."

### Why might new group membership not work immediately?

The user usually needs a new login session. Existing shells keep the old group list.

### Why use `visudo`?

`visudo` validates sudoers syntax before saving. A syntax error in sudoers can lock admins out of privilege escalation.

## Package Management

### What is the difference between `dnf` and `rpm`?

`dnf` works with repositories and resolves dependencies. `rpm` queries or installs individual RPM packages directly.

Use `dnf` for normal installs:

```bash
sudo dnf install <package>
```

Use `rpm` to inspect installed package ownership:

```bash
rpm -qf <path>
rpm -ql <package>
```

### A package is not found. What do you check first?

Check registration, enabled repositories, release lock, DNS/proxy, and package name:

```bash
sudo subscription-manager status
sudo subscription-manager repos --list-enabled
sudo subscription-manager release --show
dnf repolist --all
sudo dnf makecache
dnf info <package>
```

## systemd

### What is the difference between `start` and `enable`?

`start` runs a service now. `enable` configures it to start at boot.

```bash
sudo systemctl start <service>
sudo systemctl enable <service>
sudo systemctl enable --now <service>
```

### A service is down. What do you check first?

```bash
systemctl status <service>
journalctl -u <service> -b --no-pager
systemctl cat <service>
```

Spoken answer: "I check service state first, then current boot logs. If config changed, I validate the config and reload or restart only after fixing errors."

### What does `systemctl daemon-reload` do?

It tells systemd to reload unit definitions after unit files or overrides change. It does not restart the service by itself.

## Networking

### How do you separate network problems?

Check each layer separately:

```bash
ip addr
ip route
nmcli device status
getent hosts <hostname>
dig <hostname>
sudo ss -tulpn
```

Spoken answer: "I separate IP address, routing, DNS, listening port, firewall, and SELinux instead of calling everything a network problem."

### What does `127.0.0.1:<port>` mean in `ss` output?

The service is listening only on loopback. Remote clients cannot connect directly to that address.

### What does `0.0.0.0:<port>` mean?

The service is listening on all IPv4 interfaces.

## Firewall And SELinux

### How do you open HTTP permanently?

```bash
sudo firewall-cmd --add-service=http --permanent
sudo firewall-cmd --reload
sudo firewall-cmd --list-all
```

### What is the difference between firewalld and SELinux?

Firewalld controls network traffic entering the host. SELinux controls what processes can do once they are running.

### How do you troubleshoot SELinux?

```bash
getenforce
ls -lZ <path>
ps -eZ | grep <process>
sudo ausearch -m AVC -ts recent
getsebool -a | grep <service>
```

Spoken answer: "I do not disable SELinux as the fix. I use AVC denials to decide whether I need a label, boolean, port type, or policy change."

## Storage And LVM

### What are PV, VG, and LV?

Physical volumes are disks or partitions used by LVM. Volume groups pool that storage. Logical volumes are the usable block devices created from the pool.

### How do you test `/etc/fstab` safely?

```bash
sudo cp -a /etc/fstab /etc/fstab.bak.$(date +%F-%H%M)
sudo mount -a
findmnt <mountpoint>
```

Spoken answer: "I back up `fstab`, compare UUIDs with `blkid`, then run `mount -a` before rebooting."

### Can XFS shrink?

No. XFS can grow, often online, but it cannot shrink.

## SSH

### How do you avoid SSH lockout?

Keep an existing session open and validate the daemon config before restart:

```bash
sudo sshd -t
sudo systemctl restart sshd
sudo journalctl -u sshd -b --no-pager
```

Also check firewall and port state:

```bash
sudo ss -tulpn | grep ':22'
sudo firewall-cmd --list-all
```

## Containers

### What is Podman?

Podman is a daemonless container engine commonly used on RHEL. It supports rootless containers, which allows users to run containers without full root privileges.

Useful commands:

```bash
podman pull <image>
podman run -d --name <name> <image>
podman ps -a
podman logs <name>
podman exec -it <name> /bin/bash
```

## Web And Database Scenarios

### Apache returns 403. What do you check?

Check Unix permissions, parent directories, SELinux labels, and web logs:

```bash
namei -l <docroot>
ls -lZ <docroot>
sudo ausearch -m AVC -ts recent
sudo tail -f /var/log/httpd/error_log
```

### Nginx returns 502. What do you check?

Check whether the backend is running and reachable:

```bash
systemctl status <backend-service>
sudo ss -tulpn
curl http://127.0.0.1:<backend-port>
sudo nginx -t
```

Also check SELinux if Nginx or Apache proxies to a network backend.

## Quick Scenario Practice

### Service works locally but not remotely

Practice with: [Service down](../scenarios/service-down.md), [Port blocked](../scenarios/port-blocked.md), [Web server lab](../labs/web-server.md)

Answer: "I first prove the service works locally with `curl` or the service client. Then I check the listen address with `ss -tulpn`. If it listens only on `127.0.0.1`, remote clients cannot reach it. If it listens on the right address, I check firewalld with `firewall-cmd --list-all`, routing, DNS, and any external firewall or cloud rule. I verify from localhost and from a second host."

Useful commands:

```bash
systemctl status <service>
curl http://127.0.0.1:<port>
sudo ss -tulpn | grep <port>
sudo firewall-cmd --list-all
ip route
getent hosts <hostname>
```

### Disk is full

Practice with: [Disk full](../scenarios/disk-full.md), [Storage and LVM lab](../labs/storage-and-lvm.md)

Answer: "I check whether the filesystem is full or the inode table is full. `df -hT` shows space and filesystem type; `df -ih` shows inode usage. Then I identify growth with `du`, avoid deleting active logs blindly, clean package caches or old logs safely, and extend storage if the data is legitimate. I verify the filesystem has free space and the affected service recovered."

Useful commands:

```bash
df -hT
df -ih
sudo du -xhd1 /var | sort -h
journalctl --disk-usage
sudo lvs
```

### DNS fails but IP works

Practice with: [DNS failure](../scenarios/dns-failure.md), [Networking basics lab](../labs/networking-basics.md)

Answer: "If IP works but names fail, I separate routing from name resolution. I confirm the route, check NetworkManager DNS settings, test normal name resolution with `getent hosts`, and query a specific DNS server with `dig @<dns-server>`. If direct DNS works but normal lookup fails, I inspect resolver configuration and NetworkManager profile settings."

Useful commands:

```bash
ip route
nmcli device show <interface> | grep DNS
getent hosts <hostname>
dig <hostname>
dig @<dns-server> <hostname>
```

### Broken boot after storage change

Practice with: [Broken fstab](../scenarios/broken-fstab.md), [Storage and LVM lab](../labs/storage-and-lvm.md)

Answer: "After a storage change, I suspect `/etc/fstab`, missing devices, bad UUIDs, or unsupported mount options. From rescue or emergency mode, I compare `fstab` with `lsblk -f` and `blkid`, comment or fix the bad entry, then test with `mount -a` before rebooting. I verify the system reaches the normal target and the mount appears in `findmnt`."

Useful commands:

```bash
lsblk -f
sudo blkid
sudo vi /etc/fstab
sudo mount -a
findmnt <mountpoint>
systemctl --failed
```

### SELinux blocks a service

Practice with: [SELinux denial](../scenarios/selinux-denial.md), [Firewall and SELinux lab](../labs/firewall-and-selinux.md)

Answer: "I do not start by disabling SELinux permanently. I confirm the mode with `getenforce`, check labels with `ls -lZ`, then read recent AVC denials. The fix depends on the evidence: restore a default label, add a file context, set a boolean, or add an SELinux port type. I verify with the service test and confirm new AVC denials are not appearing."

Useful commands:

```bash
getenforce
ls -lZ <path>
sudo ausearch -m AVC -ts recent
sudo restorecon -Rv <path>
getsebool -a | grep <service>
```

### Package install fails

Practice with: [Package repo issue](../scenarios/package-repo-issue.md), [Packages and repositories lab](../labs/packages-and-repos.md)

Answer: "I separate package name problems from repository and subscription problems. I check registration, enabled repositories, release lock, metadata cache, DNS/proxy access, and package availability. I avoid adding random third-party repositories until I understand why the expected Red Hat repository is not providing the package."

Useful commands:

```bash
sudo subscription-manager status
sudo subscription-manager repos --list-enabled
sudo subscription-manager release --show
dnf repolist --all
sudo dnf makecache
dnf info <package>
```

## Final Interview Advice

The strongest answer is calm and ordered. Say what you check first, show the command, explain the expected output, and finish with how you verify recovery.

## Page Navigation

[Interview Index](README.md) | [Scenarios](../scenarios/README.md)
