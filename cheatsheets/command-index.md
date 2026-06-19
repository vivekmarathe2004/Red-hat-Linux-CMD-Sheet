# Command Index

> **Cheatsheet** | [Home](../README.md) | [Section Index](README.md) | [Docs](../docs/README.md)

## How To Use This Sheet

Use this as a quick lookup after you understand the related concept. Tables are kept here because speed matters, but production work still requires verification and careful placeholder replacement.

## Read Before Copying

Cheatsheets are fast, but they are not a substitute for understanding. Replace placeholders, check the target host, and verify every change.

## Purpose

Fast lookup for common RHEL administration commands.

## System

| Task | Command |
| --- | --- |
| Show RHEL version | `cat /etc/redhat-release` |
| Show OS details | `cat /etc/os-release` |
| Show hostname info | `hostnamectl` |
| Set hostname | `sudo hostnamectl set-hostname <hostname>` |
| Show uptime | `uptime` |
| Show kernel | `uname -r` |
| Show boot ID | `cat /proc/sys/kernel/random/boot_id` |
| Show failed units | `systemctl --failed` |
| System health | `systemctl is-system-running` |
| Reboot | `sudo systemctl reboot` |
| Power off | `sudo systemctl poweroff` |

## Packages And Repos

| Task | Command |
| --- | --- |
| Register system | `sudo subscription-manager register --org=<org> --activationkey=<key>` |
| Subscription status | `sudo subscription-manager status` |
| List enabled repos | `sudo subscription-manager repos --list-enabled` |
| DNF repos | `dnf repolist --all` |
| Install package | `sudo dnf install <package>` |
| Remove package | `sudo dnf remove <package>` |
| Update system | `sudo dnf update` |
| Search package | `dnf search <term>` |
| Package info | `dnf info <package>` |
| Package owner | `rpm -qf <path>` |
| Package files | `rpm -ql <package>` |
| Installed package version | `rpm -q <package>` |
| Check dependencies | `dnf repoquery --requires <package>` |
| Make repo cache | `sudo dnf makecache` |
| DNF history | `dnf history` |
| DNF transaction details | `sudo dnf history info <id>` |

## Files

| Task | Command |
| --- | --- |
| List files | `ls -la` |
| Copy preserving metadata | `cp -a <src> <dst>` |
| Find file | `find <dir> -name "<pattern>"` |
| Search text | `grep -R "<text>" <dir>` |
| Disk free | `df -hT` |
| Directory size | `du -sh <path>` |
| Archive | `tar -czf <archive>.tar.gz <dir>` |
| Extract | `tar -xzf <archive>.tar.gz` |
| Show file type | `file <path>` |
| Follow file updates | `tail -f <file>` |
| Show path permissions | `namei -l <path>` |

## Processes And Logs

| Task | Command |
| --- | --- |
| Process tree | `ps -ef --forest` |
| Find process | `pgrep -a <name>` |
| Live processes | `top` |
| Kill by PID | `sudo kill <pid>` |
| Force kill by PID | `sudo kill -9 <pid>` |
| Current boot logs | `journalctl -b --no-pager` |
| Error logs this boot | `journalctl -p err -b --no-pager` |
| Unit logs | `journalctl -u <service> -b --no-pager` |
| Follow unit logs | `journalctl -u <service> -f` |
| Kernel messages | `dmesg -T` |
| Auth logs | `sudo tail -f /var/log/secure` |

## Users And Permissions

| Task | Command |
| --- | --- |
| Add user | `sudo useradd <user>` |
| Set password | `sudo passwd <user>` |
| Add group | `sudo groupadd <group>` |
| Add user to group | `sudo usermod -aG <group> <user>` |
| User identity | `id <user>` |
| Change owner | `sudo chown <user>:<group> <path>` |
| Change mode | `sudo chmod 0640 <file>` |
| ACL view | `getfacl <path>` |
| ACL set | `sudo setfacl -m u:<user>:rw <path>` |

## Services

| Task | Command |
| --- | --- |
| Status | `systemctl status <service>` |
| Start | `sudo systemctl start <service>` |
| Stop | `sudo systemctl stop <service>` |
| Restart | `sudo systemctl restart <service>` |
| Reload | `sudo systemctl reload <service>` |
| Enable now | `sudo systemctl enable --now <service>` |
| Failed units | `systemctl --failed` |
| Unit logs | `journalctl -u <service> -b` |
| Show unit file | `systemctl cat <service>` |
| List unit files | `systemctl list-unit-files` |
| Reload unit definitions | `sudo systemctl daemon-reload` |
| Validate boot target | `systemctl get-default` |

## Networking

| Task | Command |
| --- | --- |
| IP addresses | `ip addr` |
| Routes | `ip route` |
| Connections | `nmcli connection show` |
| Devices | `nmcli device status` |
| DNS lookup | `dig <name>` |
| NSS host lookup | `getent hosts <name>` |
| Listening ports | `ss -tulpn` |
| Listening TCP ports | `sudo ss -ltnp` |
| Show active zone | `sudo firewall-cmd --get-active-zones` |
| Trace path | `tracepath <host>` |
| NetworkManager profile details | `nmcli connection show <connection>` |
| Bring connection up | `sudo nmcli connection up <connection>` |
| DNS servers from NM | `nmcli device show <interface> | grep DNS` |

## Firewall And SELinux

| Task | Command |
| --- | --- |
| Firewall state | `sudo firewall-cmd --state` |
| List firewall | `sudo firewall-cmd --list-all` |
| Add service | `sudo firewall-cmd --add-service=<service> --permanent` |
| Add port | `sudo firewall-cmd --add-port=<port>/<proto> --permanent` |
| Reload firewall | `sudo firewall-cmd --reload` |
| SELinux mode | `getenforce` |
| Set permissive temporarily | `sudo setenforce 0` |
| Restore labels | `sudo restorecon -Rv <path>` |
| Add file context | `sudo semanage fcontext -a -t <type> "<path-regex>"` |
| List SELinux booleans | `getsebool -a` |
| Set SELinux boolean | `sudo setsebool -P <boolean> on` |
| Add SELinux port | `sudo semanage port -a -t <type> -p <proto> <port>` |
| SELinux denials | `sudo ausearch -m AVC -ts recent` |

## Storage

| Task | Command |
| --- | --- |
| Block devices | `lsblk -f` |
| Mount table | `findmnt` |
| Filesystem usage | `df -hT` |
| Inode usage | `df -ih` |
| UUIDs | `sudo blkid` |
| Create filesystem | `sudo mkfs.xfs <device>` |
| Mount | `sudo mount <device> <mountpoint>` |
| Test fstab | `sudo mount -a` |
| LVM PVs | `sudo pvs` |
| LVM VGs | `sudo vgs` |
| LVM LVs | `sudo lvs` |
| Extend LV and FS | `sudo lvextend -r -L +<size> /dev/<vg>/<lv>` |

## SSH And Remote Access

| Task | Command |
| --- | --- |
| SSH to host | `ssh <user>@<host>` |
| Copy file to host | `scp <file> <user>@<host>:<path>` |
| Validate sshd config | `sudo sshd -t` |
| SSH service status | `systemctl status sshd` |
| SSH logs | `sudo journalctl -u sshd -b --no-pager` |
| Check SSH listener | `sudo ss -tulpn | grep ':22'` |
| User SSH directory | `ls -ldZ ~/.ssh` |
| Authorized keys | `ls -lZ ~/.ssh/authorized_keys` |

## Containers

| Task | Command |
| --- | --- |
| Pull | `podman pull <image>` |
| Run | `podman run -d --name <name> <image>` |
| List | `podman ps -a` |
| Logs | `podman logs <name>` |
| Exec | `podman exec -it <name> /bin/bash` |
| Build | `podman build -t <tag> .` |
| Inspect remote | `skopeo inspect docker://<image>` |
| Stop container | `podman stop <name>` |
| Remove container | `podman rm <name>` |
| Images | `podman images` |
| Generate systemd unit | `podman generate systemd --new --name <name>` |
| Rootless socket status | `systemctl --user status podman.socket` |

## Safe Change Pattern

| Step | Command |
| --- | --- |
| Confirm host | `hostnamectl` |
| Back up config | `sudo cp -a <config-file> <config-file>.bak.$(date +%F-%H%M)` |
| Validate config | `sudo <validation-command>` |
| Reload service | `sudo systemctl reload <service>` |
| Restart service | `sudo systemctl restart <service>` |
| Verify service | `systemctl status <service>` |
| Verify logs | `journalctl -u <service> -b --no-pager` |

## Page Navigation

[Cheatsheets Index](README.md) | [Core Docs](../docs/README.md)
