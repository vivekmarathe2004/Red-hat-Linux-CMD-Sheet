# RHEL Linux Interview Questions And Answers

## Basic Linux

| Question | Answer |
| --- | --- |
| What is Linux? | Linux is an open-source Unix-like operating system kernel. A Linux distribution combines the kernel with tools, libraries, package management, and services. |
| What is RHEL? | Red Hat Enterprise Linux is Red Hat's enterprise Linux distribution with support, certification, lifecycle management, and enterprise tooling. |
| How do you check the RHEL version? | Use `cat /etc/redhat-release` or `cat /etc/os-release`. |
| How do you check the kernel version? | Use `uname -r`. |
| What is the root user? | The administrative superuser with full control of the system. Use it carefully, usually through `sudo`. |
| What is the difference between absolute and relative paths? | Absolute paths start from `/`; relative paths start from the current directory. |
| What is `/etc` used for? | System and service configuration files. |
| What is `/var` used for? | Variable data such as logs, spool files, caches, and application state. |
| What is `/home` used for? | Regular users' home directories. |
| What is `/boot` used for? | Kernel, initramfs, and bootloader files. |

## Files And Permissions

| Question | Answer |
| --- | --- |
| How do you view permissions? | Use `ls -l` or `stat <path>`. |
| What do `r`, `w`, and `x` mean on files? | Read, write, and execute. |
| What does execute mean on a directory? | It allows entering or traversing the directory. |
| How do you change file permissions? | Use `chmod <mode> <path>`. |
| How do you change ownership? | Use `chown <user>:<group> <path>`. |
| What is umask? | A default permission mask applied when new files and directories are created. |
| What is SUID? | A special bit that runs an executable with the file owner's privileges. |
| What is SGID? | On files, it runs with group privileges; on directories, new files inherit the directory group. |
| What is sticky bit? | On shared directories, users can delete only their own files. Common on `/tmp`. |
| What are ACLs? | Access Control Lists provide extra permissions beyond owner, group, and others. |
| How do you view ACLs? | Use `getfacl <path>`. |
| How do you set an ACL? | Use `setfacl -m u:<user>:rw <path>`. |
| How do you troubleshoot path permissions? | Use `namei -l <path>` to inspect every parent directory. |

## Users, Groups, And Sudo

| Question | Answer |
| --- | --- |
| How do you add a user? | `sudo useradd <user>` then `sudo passwd <user>`. |
| How do you delete a user with home directory? | `sudo userdel -r <user>`. This is destructive. |
| How do you add a user to a group? | `sudo usermod -aG <group> <user>`. |
| Why might a new group not appear immediately? | The user needs a new login session. |
| What file stores user accounts? | `/etc/passwd`. |
| What file stores password hashes? | `/etc/shadow`. |
| What file stores groups? | `/etc/group`. |
| What is sudo? | A tool that allows permitted users to run commands as another user, usually root. |
| How do you safely edit sudoers? | Use `sudo visudo`. |
| How do you check sudo syntax? | `sudo visudo -c`. |
| How do you check a user's sudo privileges? | `sudo -l -U <user>`. |
| How do you lock an account? | `sudo usermod -L <user>`. |
| How do you check password aging? | `sudo chage -l <user>`. |

## Package Management

| Question | Answer |
| --- | --- |
| What is RPM? | The package format and low-level package tool used on RHEL. |
| What is DNF? | The high-level package manager that resolves dependencies and works with repositories. |
| Difference between `rpm` and `dnf`? | `rpm` manages local packages directly; `dnf` handles repos, dependencies, and transactions. |
| How do you install a package? | `sudo dnf install <package>`. |
| How do you remove a package? | `sudo dnf remove <package>`. |
| How do you search for a package? | `dnf search <term>`. |
| How do you find which package owns a file? | `rpm -qf <path>`. |
| How do you list files in a package? | `rpm -ql <package>`. |
| How do you list enabled repos? | `dnf repolist` or `subscription-manager repos --list-enabled`. |
| How do you see DNF history? | `dnf history`. |
| Can every DNF transaction be undone? | No. Undo depends on package availability, dependencies, and transaction type. |

## Subscription And Repositories

| Question | Answer |
| --- | --- |
| What does Subscription Manager do? | Registers RHEL systems and manages entitlement and Red Hat repositories. |
| How do you register with activation key? | `sudo subscription-manager register --org=<org> --activationkey=<key>`. |
| How do you check subscription status? | `sudo subscription-manager status`. |
| What are BaseOS and AppStream? | BaseOS provides core OS packages; AppStream provides applications and runtimes. |
| Why should repo IDs not be copied between RHEL 9 and RHEL 10? | Repo IDs are version-specific. List repos on the target system. |
| What is a release lock? | A setting that pins the system to a specific RHEL minor release. |

## systemd

| Question | Answer |
| --- | --- |
| What is systemd? | The init and service manager used by modern RHEL systems. |
| How do you start a service? | `sudo systemctl start <service>`. |
| How do you enable a service at boot? | `sudo systemctl enable <service>`. |
| How do you start and enable together? | `sudo systemctl enable --now <service>`. |
| Difference between start and enable? | `start` runs now; `enable` starts at boot. |
| How do you check service logs? | `journalctl -u <service> -b`. |
| How do you list failed units? | `systemctl --failed`. |
| What is a target? | A group of units representing a system state. |
| How do you check default target? | `systemctl get-default`. |
| How do you change default target? | `sudo systemctl set-default <target>`. |
| What does `daemon-reload` do? | Reloads systemd unit definitions after changes. |
| What is masking a service? | Preventing it from starting manually or as a dependency. |

## Networking

| Question | Answer |
| --- | --- |
| How do you show IP addresses? | `ip addr`. |
| How do you show routes? | `ip route`. |
| What manages networking on RHEL? | NetworkManager. |
| What is `nmcli`? | Command-line tool for NetworkManager. |
| Difference between interface and connection profile? | Interface is hardware/device; connection profile is saved network configuration. |
| How do you show NetworkManager connections? | `nmcli connection show`. |
| How do you show network devices? | `nmcli device status`. |
| How do you check DNS resolution? | `dig <name>` or `getent hosts <name>`. |
| How do you check listening ports? | `sudo ss -tulpn`. |
| What does `127.0.0.1:8080` in `ss` mean? | Service listens only on loopback, not remote interfaces. |
| What does `0.0.0.0:80` mean? | Service listens on all IPv4 addresses. |

## Firewall

| Question | Answer |
| --- | --- |
| What is firewalld? | Dynamic firewall manager used on RHEL. |
| What is a firewalld zone? | A trust level applied to network interfaces or sources. |
| How do you list current firewall rules? | `sudo firewall-cmd --list-all`. |
| How do you list active zones? | `sudo firewall-cmd --get-active-zones`. |
| Difference between runtime and permanent rules? | Runtime rules apply now; permanent rules survive reload/reboot. |
| How do you permanently open HTTP? | `sudo firewall-cmd --add-service=http --permanent && sudo firewall-cmd --reload`. |
| How do you open a custom port? | `sudo firewall-cmd --add-port=<port>/<proto> --permanent && sudo firewall-cmd --reload`. |

## SELinux

| Question | Answer |
| --- | --- |
| What is SELinux? | Mandatory Access Control system that confines processes using policy and labels. |
| What are SELinux modes? | Enforcing, Permissive, and Disabled. |
| How do you check SELinux mode? | `getenforce`. |
| What is permissive mode? | SELinux logs denials but allows access. |
| How do you temporarily set permissive mode? | `sudo setenforce 0`. |
| What is an SELinux context? | A security label assigned to files, processes, ports, and other objects. |
| How do you view file contexts? | `ls -lZ <path>`. |
| How do you restore default contexts? | `sudo restorecon -Rv <path>`. |
| How do you create persistent file context rules? | `sudo semanage fcontext -a -t <type> "<path-regex>"`. |
| How do you view SELinux denials? | `sudo ausearch -m AVC -ts recent`. |
| Should SELinux be disabled? | Usually no. Fix labels, booleans, port contexts, or policies. |

## Storage And LVM

| Question | Answer |
| --- | --- |
| How do you list block devices? | `lsblk -f`. |
| How do you show mounted filesystems? | `findmnt` or `df -hT`. |
| What is `/etc/fstab`? | File that defines persistent mounts. |
| How do you test `/etc/fstab`? | `sudo mount -a`. |
| Why use UUIDs in `/etc/fstab`? | UUIDs are more stable than device names. |
| What is LVM? | Logical Volume Manager for flexible storage management. |
| What is a PV? | Physical Volume, usually a disk or partition used by LVM. |
| What is a VG? | Volume Group, a pool of storage made from PVs. |
| What is an LV? | Logical Volume, usable block device created from a VG. |
| How do you extend an LV and filesystem? | `sudo lvextend -r -L +<size> /dev/<vg>/<lv>`. |
| Can XFS shrink? | No. XFS can grow but cannot shrink. |

## Logs And Troubleshooting

| Question | Answer |
| --- | --- |
| How do you view current boot logs? | `journalctl -b`. |
| How do you view previous boot logs? | `journalctl -b -1`. |
| How do you view errors from current boot? | `journalctl -p err -b`. |
| Where are authentication logs? | `/var/log/secure` and journald. |
| Where are SELinux audit logs? | `/var/log/audit/audit.log`. |
| How do you find a high CPU process? | `top` or `ps -eo pid,cmd,%cpu --sort=-%cpu`. |
| How do you check memory? | `free -h`. |
| How do you check disk usage? | `df -hT` and `du -sh <path>`. |
| How do you find which process uses a port? | `sudo ss -tulpn | grep <port>`. |

## SSH

| Question | Answer |
| --- | --- |
| How do you connect to a remote server? | `ssh <user>@<host>`. |
| How do you create an SSH key? | `ssh-keygen -t ed25519`. |
| How do you copy a public key? | `ssh-copy-id <user>@<host>`. |
| Where are authorized keys stored? | `~/.ssh/authorized_keys`. |
| How do you validate SSH server config? | `sudo sshd -t`. |
| Why disable root SSH login? | Reduces attack risk and improves accountability. |
| What should you do before restarting SSH remotely? | Keep an existing session open and run `sshd -t`. |

## Containers

| Question | Answer |
| --- | --- |
| What is Podman? | A daemonless container engine used on RHEL. |
| What is rootless Podman? | Running containers without root privileges. |
| What is Buildah? | Tool for building container images. |
| What is Skopeo? | Tool for inspecting and copying container images without running them. |
| How do you list containers? | `podman ps -a`. |
| How do you view container logs? | `podman logs <container>`. |
| How do you run a container in background? | `podman run -d --name <name> <image>`. |
| How do you map ports? | `podman run -p <hostport>:<containerport> <image>`. |

## Web And Database Services

| Question | Answer |
| --- | --- |
| How do you validate Apache config? | `sudo apachectl configtest`. |
| Apache service name on RHEL? | `httpd`. |
| How do you validate Nginx config? | `sudo nginx -t`. |
| What causes Nginx 502 errors? | Backend service down, wrong proxy address, SELinux, firewall, or timeout. |
| How do you initialize PostgreSQL? | `sudo postgresql-setup --initdb`. |
| Which PostgreSQL file controls client authentication? | `pg_hba.conf`. |
| How do you secure MariaDB initially? | `sudo mariadb-secure-installation`. |
| What should be checked before exposing a database remotely? | Bind address, auth rules, firewall, SELinux, strong passwords, and network restrictions. |

## Scenario-Based Interview Questions

| Scenario | Strong Answer |
| --- | --- |
| Website works locally but not remotely. | Check service status, listen address, firewall, SELinux, routing, DNS, and external network ACLs. Commands: `systemctl status`, `ss -tulpn`, `firewall-cmd --list-all`, `getenforce`, `ausearch`. |
| User cannot sudo after being added to wheel. | Confirm group membership with `id`, ask user to re-login, check sudoers with `visudo -c`, and verify with `sudo -l -U <user>`. |
| Server boots into emergency mode after storage change. | Suspect `/etc/fstab`. Check UUIDs with `blkid`, fix bad entries, test with `mount -a`. |
| Service fails after config edit. | Run service-specific validation, check `journalctl -u <service> -b`, restore backup if needed, then reload/restart. |
| SELinux blocks a web directory. | Check AVC denials and labels, add proper file context with `semanage fcontext`, then `restorecon`. |
| Package install says no match found. | Check enabled repos, subscription status, package name, architecture, release lock, and AppStream availability. |
| Disk is full. | Use `df -hT`, `du -xhd1 /`, check logs/cache, identify growth source, and extend storage if needed. |
| Port is already in use. | Use `ss -tulpn | grep <port>` to find the process, then stop, reconfigure, or change the conflicting service. |

