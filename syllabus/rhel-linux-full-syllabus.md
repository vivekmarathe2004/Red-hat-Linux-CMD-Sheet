# RHEL 9/10 Full Study Syllabus

This syllabus organizes the repository into a complete learning path for Red Hat Enterprise Linux administration. It is written for practical admin learning, interview preparation, and RHCSA/RHCE-style revision.

## How To Study

1. Read the objective.
2. Practice the commands in a VM.
3. Break the system intentionally in a lab.
4. Troubleshoot using logs, status commands, firewall checks, and SELinux checks.
5. Write your own notes after every lab.

## Job-Prep Learning Path

| Phase | Focus | Output |
| --- | --- | --- |
| Foundation | Shell, files, users, packages | You can navigate, install tools, and explain permissions |
| Admin core | systemd, networking, firewall, SELinux, storage | You can configure and verify a RHEL server |
| Services | SSH, web, database, containers, logging | You can deploy common services safely |
| Troubleshooting | Logs, ports, SELinux, DNS, storage, repos | You can diagnose issues in a repeatable order |
| Interview prep | Scenario answers and command explanation | You can explain what you check first and why |

## Practice Resources

- [Hands-on labs](../labs/README.md)
- [Troubleshooting scenarios](../scenarios/README.md)
- [Interview questions and answers](../interview/rhel-linux-interview-q-and-a.md)
- [General revision notes](../notes/general-notes.md)

## What To Prove Before Interviews

- You can explain `systemctl status`, `journalctl`, `ss`, `firewall-cmd`, `getenforce`, `ausearch`, `lsblk`, `df`, `dnf`, and `rpm`.
- You can separate service, network, firewall, SELinux, DNS, permission, package, and storage problems.
- You can describe a safe change process: check, back up, edit, validate, reload, verify.
- You can troubleshoot without immediately disabling SELinux or firewalld.
- You can explain commands in plain language, not only memorize them.

## Module 01: Linux Fundamentals

### Objectives

- Understand shell basics, command syntax, files, directories, paths, and help systems.
- Navigate the filesystem confidently.
- Use manual pages and package documentation.

### Topics

- Linux directory structure.
- Absolute and relative paths.
- File types.
- Command structure: command, option, argument.
- Shell redirection and pipes.
- Environment variables.
- Manual pages and documentation.

### Must Know Commands

```bash
pwd
ls -la
cd <directory>
cat <file>
less <file>
head <file>
tail <file>
man <command>
info <command>
command -v <command>
which <command>
echo $PATH
history
```

### Lab Tasks

- Navigate from `/` to `/etc`, `/var/log`, `/home`, and `/tmp`.
- Use `man`, `--help`, and package docs to understand a command.
- Create a small command pipeline using `grep`, `sort`, and `uniq`.

### Expected Outcome

You can move around the filesystem, read help, inspect files, and build simple shell command pipelines.

### Revision Checkpoint

- Explain absolute vs relative path.
- Explain why `/etc`, `/var`, `/home`, `/tmp`, and `/boot` matter.
- Explain how you would find help for an unknown command.

### Must Explain In Interview

“I use `man`, `--help`, and package documentation first, then test commands safely in a lab before using them on production systems.”

## Module 02: Installation, Subscription, And Repositories

### Objectives

- Register RHEL systems.
- Enable correct RHEL 9 or RHEL 10 repositories.
- Understand BaseOS, AppStream, and package lifecycle basics.

### Topics

- RHEL installation overview.
- Subscription Manager.
- Activation keys.
- Repository IDs.
- DNF cache and metadata.
- Package transactions.

### Must Know Commands

```bash
sudo subscription-manager register --org=<org> --activationkey=<key>
sudo subscription-manager status
sudo subscription-manager repos --list
sudo subscription-manager repos --list-enabled
sudo subscription-manager repos --enable=<repo-id>
dnf repolist --all
sudo dnf makecache
sudo dnf update
dnf history
```

### Lab Tasks

- Register a lab VM.
- Enable BaseOS and AppStream.
- Install and remove a test package.
- Review `dnf history`.

### Expected Outcome

You can register a RHEL system, identify enabled repositories, and understand why RHEL 9 and RHEL 10 repo IDs differ.

### Revision Checkpoint

- Explain BaseOS vs AppStream.
- Explain what `subscription-manager status` proves.
- Explain what a release lock can affect.

### Must Explain In Interview

“If a package is not found, I check registration, enabled repos, release lock, DNS/proxy, and package name.”

## Module 03: Package Management

### Objectives

- Install, update, remove, query, and troubleshoot RPM packages.
- Understand the difference between `rpm` and `dnf`.

### Topics

- RPM database.
- Package dependencies.
- Repository metadata.
- Package ownership of files.
- Transaction rollback limitations.

### Must Know Commands

```bash
sudo dnf install <package>
sudo dnf remove <package>
dnf search <term>
dnf info <package>
rpm -q <package>
rpm -qa
rpm -qf <path>
rpm -ql <package>
dnf provides "*/<file>"
sudo dnf history undo <id>
```

### Lab Tasks

- Find which package provides a command.
- List files installed by a package.
- Identify which package owns `/etc/ssh/sshd_config`.

### Expected Outcome

You can install packages with dependency resolution and use RPM queries to inspect installed files.

### Revision Checkpoint

- Explain `dnf` vs `rpm`.
- Explain `rpm -qf`.
- Explain `dnf history`.

### Must Explain In Interview

“I use `dnf` for repo-based package operations and `rpm` for direct package database queries.”

## Module 04: Files, Permissions, And ACLs

### Objectives

- Manage files, directories, ownership, permissions, umask, and ACLs.
- Diagnose permission problems across a full path.

### Topics

- File ownership.
- Permission bits.
- Special permissions: SUID, SGID, sticky bit.
- ACLs.
- `umask`.
- File attributes.

### Must Know Commands

```bash
touch <file>
mkdir -p <directory>
cp -a <src> <dst>
mv <src> <dst>
rm <path>
chmod <mode> <path>
chown <user>:<group> <path>
umask
namei -l <path>
getfacl <path>
setfacl -m u:<user>:rw <path>
lsattr <path>
chattr +i <path>
```

### Lab Tasks

- Create a shared directory using SGID.
- Add ACL access for one user.
- Troubleshoot a permission denied error with `namei -l`.

### Expected Outcome

You can design simple shared directory access and diagnose permission failures across parent directories.

### Revision Checkpoint

- Explain read/write/execute on files vs directories.
- Explain SUID, SGID, sticky bit.
- Explain ACLs and `getfacl`.

### Must Explain In Interview

“When access fails, I check every parent directory with `namei -l`, then ownership, mode, ACLs, and SELinux labels.”

## Module 05: Users, Groups, And Sudo

### Objectives

- Create and manage local users and groups.
- Configure sudo safely.
- Understand account locking, password aging, and login shells.

### Topics

- `/etc/passwd`, `/etc/shadow`, `/etc/group`.
- UID and GID.
- Supplementary groups.
- Password aging.
- Sudoers.
- Account lock and unlock.

### Must Know Commands

```bash
sudo useradd <user>
sudo passwd <user>
sudo usermod -aG <group> <user>
sudo userdel <user>
sudo groupadd <group>
id <user>
getent passwd <user>
getent group <group>
sudo chage -l <user>
sudo usermod -L <user>
sudo usermod -U <user>
sudo visudo
sudo -l -U <user>
```

### Lab Tasks

- Create an admin user with sudo access.
- Create a locked service account.
- Configure a sudo drop-in file under `/etc/sudoers.d/`.

### Expected Outcome

You can manage local identities, sudo access, and basic account security.

### Revision Checkpoint

- Explain UID, GID, primary group, supplementary groups.
- Explain why group changes require a new login session.
- Explain why `visudo` matters.

### Must Explain In Interview

“I validate sudoers syntax with `visudo -c` and confirm user access with `sudo -l -U <user>`.”

## Module 06: systemd And Boot

### Objectives

- Manage services, targets, unit files, overrides, and boot troubleshooting.

### Topics

- Units and unit types.
- Service states.
- Enable vs start.
- Targets.
- Journald.
- Boot failures.
- Unit overrides.

### Must Know Commands

```bash
systemctl status <service>
sudo systemctl start <service>
sudo systemctl stop <service>
sudo systemctl restart <service>
sudo systemctl enable --now <service>
sudo systemctl disable <service>
systemctl --failed
systemctl get-default
sudo systemctl set-default multi-user.target
systemctl cat <service>
sudo systemctl edit <service>
sudo systemctl daemon-reload
journalctl -b
journalctl -u <service> -b
```

### Lab Tasks

- Enable and disable a service.
- Create a systemd override.
- Troubleshoot a failed service from logs.

### Expected Outcome

You can control services, inspect unit files, manage boot startup, and read logs for failed services.

### Revision Checkpoint

- Explain `start` vs `enable`.
- Explain `restart` vs `reload`.
- Explain when to run `daemon-reload`.

### Must Explain In Interview

“For a failed service, I check `systemctl status`, then `journalctl -u <service> -b`, validate config, and inspect ports/SELinux if needed.”

## Module 07: Networking

### Objectives

- Configure hostnames, IP addresses, DNS, routes, and NetworkManager profiles.

### Topics

- Interface naming.
- IP addressing.
- Default gateway.
- DNS client configuration.
- NetworkManager connections.
- Listening ports.
- Name resolution troubleshooting.

### Must Know Commands

```bash
ip addr
ip route
ip link
nmcli general status
nmcli device status
nmcli connection show
sudo nmcli connection modify <connection> ipv4.method manual
sudo nmcli connection up <connection>
hostnamectl
sudo hostnamectl set-hostname <hostname>
ss -tulpn
dig <name>
getent hosts <name>
tracepath <host>
```

### Lab Tasks

- Set a static IP using `nmcli`.
- Configure DNS servers.
- Troubleshoot a service listening only on `127.0.0.1`.

### Expected Outcome

You can inspect network state and distinguish address, route, DNS, and listening-port issues.

### Revision Checkpoint

- Explain interface vs NetworkManager connection profile.
- Explain `127.0.0.1` vs `0.0.0.0` listening.
- Explain how DNS is configured through NetworkManager.

### Must Explain In Interview

“I separate connectivity from name resolution: first IP and route, then DNS, then service and firewall.”

## Module 08: Firewall And SELinux

### Objectives

- Manage network access with firewalld.
- Diagnose and fix SELinux access denials properly.

### Topics

- Firewalld zones.
- Services and ports.
- Runtime vs permanent firewall rules.
- SELinux modes.
- SELinux labels.
- Booleans.
- Port contexts.
- AVC denials.

### Must Know Commands

```bash
sudo firewall-cmd --state
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --list-all
sudo firewall-cmd --add-service=<service> --permanent
sudo firewall-cmd --add-port=<port>/<proto> --permanent
sudo firewall-cmd --reload
getenforce
sudo setenforce 0
ls -lZ <path>
ps -eZ | grep <process>
sudo restorecon -Rv <path>
getsebool -a
sudo setsebool -P <boolean> on
sudo semanage fcontext -a -t <type> "<path-regex>"
sudo semanage port -a -t <type> -p tcp <port>
sudo ausearch -m AVC -ts recent
```

### Lab Tasks

- Open HTTP permanently.
- Move web content to `/srv/www` and fix SELinux labels.
- Add an SELinux port mapping for a non-standard service port.

### Expected Outcome

You can open services safely and fix common SELinux label, boolean, and port-context issues.

### Revision Checkpoint

- Explain runtime vs permanent firewalld rules.
- Explain SELinux enforcing vs permissive.
- Explain labels, booleans, and port types.

### Must Explain In Interview

“I do not disable SELinux as a fix; I use AVC logs to identify the correct label, boolean, or port type.”

## Module 09: Storage, Filesystems, And LVM

### Objectives

- Partition disks, create filesystems, configure persistent mounts, and manage LVM.

### Topics

- Block devices.
- Filesystems.
- UUIDs.
- `/etc/fstab`.
- LVM physical volumes, volume groups, logical volumes.
- Growing filesystems.
- Swap.

### Must Know Commands

```bash
lsblk -f
sudo blkid
df -hT
findmnt
sudo parted <device>
sudo mkfs.xfs <device>
sudo mount <device> <mountpoint>
sudo mount -a
sudo pvs
sudo vgs
sudo lvs
sudo pvcreate <device>
sudo vgcreate <vg> <device>
sudo lvcreate -n <lv> -L <size> <vg>
sudo lvextend -r -L +<size> /dev/<vg>/<lv>
```

### Lab Tasks

- Create an LVM logical volume.
- Format it with XFS.
- Mount it persistently using UUID.
- Extend it online.

### Expected Outcome

You can safely create and grow lab storage using LVM and persistent mounts.

### Revision Checkpoint

- Explain PV, VG, LV.
- Explain why UUIDs are used in `/etc/fstab`.
- Explain why XFS can grow but not shrink.

### Must Explain In Interview

“Before storage changes, I confirm the target disk with `lsblk -f` and back up `/etc/fstab` before editing persistent mounts.”

## Module 10: Logs, Monitoring, And Performance

### Objectives

- Read logs, inspect processes, identify resource issues, and collect diagnostics.

### Topics

- Journald.
- Classic logs.
- Process states.
- CPU, memory, disk, and network checks.
- `sosreport`.
- Socket inspection.

### Must Know Commands

```bash
ps aux
top
free -h
uptime
journalctl -b
journalctl -p err -b
journalctl -u <service> -b
sudo tail -f /var/log/messages
sudo tail -f /var/log/secure
sudo ss -tulpn
lsof <path>
sudo dmesg -T
sudo sos report
```

### Lab Tasks

- Identify a high CPU process.
- Find which process is using a port.
- Generate a diagnostic report.

### Expected Outcome

You can collect useful system evidence before making changes.

### Revision Checkpoint

- Explain journald vs classic logs.
- Explain `ss -tulpn`.
- Explain `df` vs `du`.

### Must Explain In Interview

“I gather logs, process state, ports, disk, and memory information before changing a running production service.”

## Module 11: SSH And Remote Access

### Objectives

- Configure SSH safely for administration.
- Use keys and troubleshoot login failures.

### Topics

- SSH server and client.
- Key-based login.
- `authorized_keys`.
- SSH daemon validation.
- Root login policy.
- Remote copy tools.

### Must Know Commands

```bash
ssh <user>@<host>
ssh-keygen -t ed25519
ssh-copy-id <user>@<host>
scp <file> <user>@<host>:<path>
rsync -av <src> <user>@<host>:<dst>
sudo sshd -t
sudo systemctl restart sshd
sudo journalctl -u sshd -b
```

### Lab Tasks

- Configure key-based SSH login.
- Disable password login in a lab.
- Troubleshoot failed SSH login using logs.

### Expected Outcome

You can configure SSH safely and avoid remote lockout.

### Revision Checkpoint

- Explain `authorized_keys`.
- Explain key permissions.
- Explain why `sshd -t` is important.

### Must Explain In Interview

“Before restarting SSH remotely, I keep an existing session open and validate config with `sshd -t`.”

## Module 12: Shell Scripting And Automation

### Objectives

- Write safe shell scripts and automate tasks with cron or systemd timers.

### Topics

- Variables.
- Exit codes.
- Tests and conditionals.
- Loops.
- Functions.
- Cron.
- systemd timers.

### Must Know Commands

```bash
bash -n <script>
bash -x <script>
chmod +x <script>
crontab -e
crontab -l
sudo systemctl list-timers
logger "message"
```

### Lab Tasks

- Write a script that checks disk usage.
- Schedule it with cron.
- Log output using `logger`.

### Expected Outcome

You can write small admin scripts and schedule them safely.

### Revision Checkpoint

- Explain exit codes.
- Explain why scripts should use absolute paths in cron.
- Explain when a systemd timer is better than cron.

### Must Explain In Interview

“Cron jobs have a limited environment, so I set `PATH` or use absolute command paths.”

## Module 13: Containers

### Objectives

- Use Podman, Buildah, and Skopeo for container operations.

### Topics

- Rootless containers.
- Images and containers.
- Registries.
- Container logs.
- Port mapping.
- Volumes.
- systemd integration.

### Must Know Commands

```bash
podman pull <image>
podman run -d --name <name> -p <hostport>:<containerport> <image>
podman ps -a
podman logs <name>
podman exec -it <name> /bin/bash
podman stop <name>
podman rm <name>
podman build -t <tag> .
skopeo inspect docker://<image>
```

### Lab Tasks

- Run a rootless web container.
- Expose a port.
- Generate a systemd unit for a container.

### Expected Outcome

You can run and inspect containers using the RHEL-native Podman toolset.

### Revision Checkpoint

- Explain rootless Podman.
- Explain image vs container.
- Explain port mapping.

### Must Explain In Interview

“Podman is daemonless and supports rootless containers, which helps reduce privilege exposure.”

## Module 14: Web, Database, And Infrastructure Services

### Objectives

- Install and troubleshoot common RHEL server roles.

### Topics

- Apache.
- Nginx.
- MariaDB.
- PostgreSQL.
- BIND DNS.
- DHCP.
- NFS.
- Samba.
- Postfix.
- Chrony.
- Rsyslog.
- Cockpit.

### Must Know Checks

```bash
systemctl status <service>
journalctl -u <service> -b
sudo ss -tulpn
sudo firewall-cmd --list-all
getenforce
sudo ausearch -m AVC -ts recent
```

### Lab Tasks

- Deploy Apache and Nginx.
- Deploy MariaDB or PostgreSQL.
- Configure one file sharing service.
- Configure time sync with Chrony.

### Expected Outcome

You can deploy common service roles and verify them through service status, logs, ports, firewall, and SELinux.

### Revision Checkpoint

- Explain service-specific validation commands.
- Explain why remote access may need app config, firewall, and SELinux changes.
- Explain where service logs usually live.

### Must Explain In Interview

“I verify a service by checking the process, port, firewall, SELinux, local test, remote test, and logs.”

## Module 15: Security And Hardening

### Objectives

- Apply practical hardening without breaking manageability.

### Topics

- Updates.
- Least privilege.
- Sudo.
- SSH hardening.
- SELinux.
- Firewalld.
- Auditd.
- Crypto policies.
- File integrity.

### Must Know Commands

```bash
sudo dnf update
sudo auditctl -s
sudo systemctl enable --now auditd
update-crypto-policies --show
sudo update-crypto-policies --set DEFAULT
sudo visudo -c
sudo ss -tulpn
sudo firewall-cmd --list-all
```

### Lab Tasks

- Review exposed services.
- Enable auditd.
- Check crypto policy.
- Validate sudoers.

### Expected Outcome

You can apply basic hardening while preserving manageability.

### Revision Checkpoint

- Explain least privilege.
- Explain why direct root SSH login is risky.
- Explain crypto policies.

### Must Explain In Interview

“Hardening should be tested; I avoid breaking legacy clients without checking crypto policy, logs, and application requirements.”

## Module 16: Troubleshooting Mastery

### Objectives

- Build a repeatable troubleshooting process for real incidents.

### Topics

- Service failures.
- Boot issues.
- Network issues.
- DNS issues.
- Firewall issues.
- SELinux denials.
- Permission issues.
- Disk full.
- Package conflicts.

### Universal Triage Commands

```bash
hostnamectl
cat /etc/redhat-release
systemctl --failed
journalctl -p err -b
ip addr
ip route
sudo ss -tulpn
sudo firewall-cmd --list-all
getenforce
sudo ausearch -m AVC -ts recent
df -hT
lsblk -f
dnf repolist
```

### Lab Tasks

- Break a service config and fix it.
- Break an SELinux label and fix it.
- Break `/etc/fstab` in a VM and recover.
- Diagnose a firewall-blocked service.

### Expected Outcome

You can diagnose common RHEL failures using a consistent evidence-first process.

### Revision Checkpoint

- Explain your first five troubleshooting commands.
- Explain how to distinguish firewall vs service vs SELinux.
- Explain how to recover from a bad `/etc/fstab`.

### Must Explain In Interview

“I start broad with failed units and boot errors, then narrow into service logs, ports, firewall, SELinux, storage, and package state.”
