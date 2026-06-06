# Command Index

## How To Use This Sheet

Use this as a quick lookup after you understand the related concept. Tables are kept here because speed matters, but production work still requires verification and careful placeholder replacement.

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
| Package owner | `rpm -qf <path>` |
| Package files | `rpm -ql <package>` |
| DNF history | `dnf history` |

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

## Networking

| Task | Command |
| --- | --- |
| IP addresses | `ip addr` |
| Routes | `ip route` |
| Connections | `nmcli connection show` |
| Devices | `nmcli device status` |
| DNS lookup | `dig <name>` |
| Listening ports | `ss -tulpn` |
| Trace path | `tracepath <host>` |

## Firewall And SELinux

| Task | Command |
| --- | --- |
| Firewall state | `sudo firewall-cmd --state` |
| List firewall | `sudo firewall-cmd --list-all` |
| Add service | `sudo firewall-cmd --add-service=<service> --permanent` |
| Add port | `sudo firewall-cmd --add-port=<port>/<proto> --permanent` |
| Reload firewall | `sudo firewall-cmd --reload` |
| SELinux mode | `getenforce` |
| Restore labels | `sudo restorecon -Rv <path>` |
| Add file context | `sudo semanage fcontext -a -t <type> "<path-regex>"` |
| SELinux denials | `sudo ausearch -m AVC -ts recent` |

## Storage

| Task | Command |
| --- | --- |
| Block devices | `lsblk -f` |
| Mount table | `findmnt` |
| Create filesystem | `sudo mkfs.xfs <device>` |
| Mount | `sudo mount <device> <mountpoint>` |
| LVM PVs | `sudo pvs` |
| LVM VGs | `sudo vgs` |
| LVM LVs | `sudo lvs` |
| Extend LV and FS | `sudo lvextend -r -L +<size> /dev/<vg>/<lv>` |

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

