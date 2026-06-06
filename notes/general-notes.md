# General Notes

> **Notes** | [Home](../README.md) | [Section Index](README.md) | [Syllabus](../syllabus/README.md) | [Interview](../interview/README.md)

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

General notes for using these RHEL 9 and RHEL 10 command sheets safely.

## Safety Notes

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

- Do not run disk, firewall, SELinux, user deletion, or repository removal commands blindly.
- Replace placeholders such as `<device>`, `<interface>`, `<hostname>`, `<user>`, `<service>`, and `<mountpoint>` before running commands.
- Test destructive commands in a VM or lab first.
- Take backups before changing production configuration files.
- Keep a second root or sudo session open when changing SSH, firewall, SELinux, or network settings remotely.

## Safe Change Pattern

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

```bash
# 1. Confirm current state

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.
systemctl status <service>
sudo firewall-cmd --list-all
getenforce

# 2. Back up config

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.
sudo cp -a <config-file> <config-file>.bak.$(date +%F-%H%M)

# 3. Edit and validate

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.
sudo vi <config-file>
sudo <validation-command>

# 4. Reload or restart

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.
sudo systemctl reload <service> || sudo systemctl restart <service>

# 5. Verify

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.
systemctl status <service>
journalctl -u <service> -b --no-pager
```

## RHEL Version Notes

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

- RHEL 9 and RHEL 10 use different repository IDs. Always confirm with `subscription-manager repos --list`.
- Package versions and application streams can differ between major releases.
- Security defaults can become stricter in newer releases, especially crypto policies.
- Prefer `dnf`, `nmcli`, `systemctl`, `firewall-cmd`, and `podman` over older legacy workflows.

## Production Checklist

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

Before exposing a service:

```bash
systemctl status <service>
sudo ss -tulpn
sudo firewall-cmd --list-all
getenforce
sudo ausearch -m AVC -ts recent
journalctl -u <service> -b --no-pager
```

Before changing storage:

```bash
lsblk -f
findmnt
df -hT
sudo blkid
sudo cp -a /etc/fstab /etc/fstab.bak.$(date +%F-%H%M)
```

Before changing networking remotely:

```bash
ip addr
ip route
nmcli connection show
nmcli device status
sudo firewall-cmd --list-all
```

## Troubleshooting Habit

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

Use this order for most issues:

1. Confirm the service exists and is running.
2. Read current boot logs.
3. Confirm the port is listening.
4. Confirm firewall access.
5. Check SELinux denials.
6. Confirm file permissions and ownership.
7. Confirm the correct RHEL version, package version, and repository state.

```bash
systemctl status <service>
journalctl -u <service> -b
sudo ss -tulpn
sudo firewall-cmd --list-all
sudo ausearch -m AVC -ts recent
ls -lZ <path>
cat /etc/redhat-release
rpm -q <package>
dnf repolist
```

## Admin Mindset

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

- First identify whether the issue is service, network, firewall, SELinux, permission, storage, package, or DNS related.
- Prefer reading logs before changing configuration.
- Make one change at a time and verify after each change.
- Keep commands repeatable by saving exact steps in a change note or ticket.
- Use package-managed config locations instead of random custom paths unless there is a clear reason.
- Do not disable security controls to make a problem disappear. Use the denial or error message to find the correct fix.

## Student And Job-Prep Strategy

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

- Learn every topic in this order: concept, command, config file, verification, troubleshooting.
- For each command, memorize what it proves, not only what it prints.
- Practice saying answers aloud. Interviews reward clear diagnostic thinking.
- Build a small VM lab and repeat labs until commands feel familiar.
- Keep one page of personal mistakes; reviewing mistakes is faster than rereading everything.
- Do not skip SELinux, firewalld, storage, and systemd logs. These topics appear constantly in practical interviews.

## Answer Pattern For Interviews

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

Use this structure when answering troubleshooting questions:

1. State what you would check first.
2. Name the command.
3. Explain what result you expect.
4. Explain the next branch if the result is good or bad.

Example:

```text
I would first check whether the service is running with systemctl status.
If it is failed, I read journalctl -u service -b.
If it is running, I check whether it is listening with ss -tulpn.
Then I check firewalld, SELinux, and application logs.
```

## Common Beginner Mistakes

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

| Mistake | Better Habit |
| --- | --- |
| Running commands without checking the target | Confirm host, device, service, and path first |
| Disabling SELinux permanently | Read AVC denials and fix labels, booleans, or port types |
| Editing `/etc/resolv.conf` directly | Use NetworkManager connection settings |
| Restarting SSH remotely without validation | Keep a session open and run `sshd -t` |
| Editing `/etc/fstab` without testing | Back it up and run `mount -a` |
| Opening firewall ports without checking listen state | Confirm service is listening with `ss -tulpn` |
| Deleting logs blindly during disk-full issues | Identify growth source and clean safely |
| Using `kill -9` first | Try graceful stop or normal kill before force |

## What Every Job-Ready Beginner Should Know

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

| Area | You Should Be Able To Explain |
| --- | --- |
| systemd | service state, enable vs start, logs, failed units |
| Networking | IP, route, DNS, listen address, ports |
| Firewall | zones, services, runtime vs permanent rules |
| SELinux | enforcing/permissive, labels, booleans, AVC denials |
| Storage | block devices, filesystems, fstab, LVM basics |
| Packages | DNF vs RPM, repo enablement, package ownership |
| Users | UID/GID, groups, sudo, password aging |
| Web | 403 vs 502 troubleshooting |
| SSH | keys, config validation, remote lockout prevention |

## Common Command Patterns

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

| Goal | Pattern |
| --- | --- |
| Check service health | `systemctl status <service>` |
| Read service logs | `journalctl -u <service> -b --no-pager` |
| Find listening process | `sudo ss -tulpn | grep <port>` |
| Check package owner | `rpm -qf <path>` |
| Check package files | `rpm -ql <package>` |
| Check command path | `command -v <command>` |
| Check file access path | `namei -l <path>` |
| Check SELinux label | `ls -lZ <path>` |
| Check firewall | `sudo firewall-cmd --list-all` |
| Check repo state | `dnf repolist --all` |

## Service Deployment Notes

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

Use this flow for most services:

```bash
sudo dnf install <package>
sudo systemctl enable --now <service>
systemctl status <service>
sudo ss -tulpn | grep <port>
sudo firewall-cmd --add-service=<firewall-service> --permanent
sudo firewall-cmd --reload
journalctl -u <service> -b --no-pager
```

If a service uses a custom directory:

```bash
sudo mkdir -p <directory>
sudo chown <user>:<group> <directory>
sudo chmod <mode> <directory>
ls -ldZ <directory>
```

If SELinux blocks the service:

```bash
sudo ausearch -m AVC -ts recent
sudo sealert -a /var/log/audit/audit.log
ls -lZ <path>
getsebool -a | grep <service>
```

Install SELinux helper tools when needed:

```bash
sudo dnf install policycoreutils-python-utils setroubleshoot-server
```

## Repository And Package Notes

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

- Use Red Hat repositories through `subscription-manager` whenever possible.
- Avoid mixing random third-party repositories on production systems.
- Always check what a package removal will remove before confirming.
- Use `dnf history` after major package changes so rollback options are visible.
- Keep repository files readable and documented when adding custom repos.

Useful commands:

```bash
sudo subscription-manager status
sudo subscription-manager repos --list-enabled
dnf repolist --all
dnf info <package>
dnf repoquery --requires <package>
dnf history
sudo dnf history info <id>
```

## Network Notes

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

- `ip addr` shows interface addresses.
- `ip route` shows routing.
- `nmcli connection show` shows saved NetworkManager profiles.
- `nmcli device status` shows actual device state.
- A system can have a connection profile that is not active.
- DNS failure and routing failure are different problems; test them separately.

Basic network triage:

```bash
ip addr
ip route
nmcli device status
nmcli connection show
ping -c 4 <gateway>
getent hosts <hostname>
dig <hostname>
tracepath <remote-host>
```

## Firewall And SELinux Notes

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

Firewalld controls network access. SELinux controls what processes can do after access reaches the system.

Good firewall habits:

```bash
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --list-all
sudo firewall-cmd --add-service=<service> --permanent
sudo firewall-cmd --reload
```

Good SELinux habits:

```bash
getenforce
ls -lZ <path>
ps -eZ | grep <process>
sudo ausearch -m AVC -ts recent
sudo restorecon -Rv <path>
```

Use `setenforce 0` only as a temporary test. If permissive mode fixes the issue, the real fix is usually a context, boolean, port type, or policy change.

## Storage Notes

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

Before touching disks:

```bash
lsblk -f
sudo blkid
df -hT
findmnt
sudo pvs
sudo vgs
sudo lvs
```

High-risk commands:

```bash
sudo mkfs.xfs <device>
sudo pvcreate <device>
sudo parted <device>
sudo wipefs <device>
```

These can destroy data. Confirm device names carefully because names such as `/dev/sdb` can change between boots or hardware changes.

Prefer UUIDs in `/etc/fstab`:

```bash
sudo blkid <device>
sudo vi /etc/fstab
sudo mount -a
findmnt <mountpoint>
```

## User And Permission Notes

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

- Unix permissions are checked across the whole path, not only the final file.
- Group membership usually requires logout/login before it appears in a session.
- ACLs are useful, but too many ACLs can make troubleshooting harder.
- Use `visudo` for sudo rules because it validates syntax.

Useful commands:

```bash
id <user>
getent passwd <user>
getent group <group>
namei -l <path>
getfacl <path>
sudo visudo -c
sudo -l -U <user>
```

## Boot And Rescue Notes

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

If a system fails after config changes:

```bash
systemctl --failed
journalctl -xb
journalctl -p err -b
```

Common boot blockers:

| Problem | Likely Area |
| --- | --- |
| Emergency mode after storage change | `/etc/fstab`, missing disk, bad UUID |
| Network does not start | NetworkManager profile, interface name, IP conflict |
| Service fails during boot | Unit dependency, config syntax, permission, SELinux |
| Login failure | PAM, SSSD, expired account, filesystem full |

## Documentation Habit

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

For every server you configure, record:

- RHEL version and kernel.
- Package names and versions.
- Enabled repositories.
- Service names and enabled state.
- Config files changed.
- Firewall services or ports opened.
- SELinux booleans, contexts, or ports changed.
- Verification commands and results.

## Daily Revision Drill

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

Run these in a lab VM and explain each output:

```bash
cat /etc/redhat-release
hostnamectl
systemctl --failed
journalctl -p err -b --no-pager
ip addr
ip route
sudo ss -tulpn
sudo firewall-cmd --list-all
getenforce
df -hT
lsblk -f
dnf repolist
```

## Before You Say "It Is A Network Issue"

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

Check these separately:

- Does the server have the right IP?
- Does it have a route?
- Does DNS resolve?
- Is the service listening?
- Is it listening on the right address?
- Is firewalld allowing traffic?
- Is SELinux blocking the process?
- Is an external firewall or cloud rule blocking it?

## Strong Interview Closing Line

## How To Read This Guide

Read this as a practical field guide, not a dictionary. Each section should help you answer what the concept means, when to use it, how to verify it, and how to explain it in an interview.

```text
After fixing the issue, I verify the service from localhost and from a remote client, check logs again, and document what changed.
```

## Page Navigation

[Notes Index](README.md) | [Syllabus](../syllabus/README.md)
