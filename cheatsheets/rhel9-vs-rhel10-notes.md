# RHEL 9 vs RHEL 10 Notes

## How To Use This Sheet

Use this as a quick lookup after you understand the related concept. Tables are kept here because speed matters, but production work still requires verification and careful placeholder replacement.

## Purpose

Track practical differences to check before copying commands between RHEL 9 and RHEL 10 systems.

## Always Confirm

| Area | What To Check | Command |
| --- | --- | --- |
| Release | Exact RHEL version | `cat /etc/redhat-release` |
| Kernel | Running kernel | `uname -r` |
| Repositories | Enabled repo IDs | `dnf repolist` |
| Package versions | Installed package version | `rpm -q <package>` |
| Service names | Unit availability | `systemctl list-unit-files | grep <name>` |
| Crypto policy | Active policy | `update-crypto-policies --show` |
| SELinux | Mode and contexts | `getenforce`, `ls -Z <path>` |

## Practical Guidance

| Topic | Guidance |
| --- | --- |
| Repository IDs | RHEL 9 and RHEL 10 repo IDs are different. Use `subscription-manager repos --list` on the target system. |
| Package streams | Database, language runtime, and web stack versions can differ. Confirm before migration. |
| Security defaults | Newer RHEL releases can have stricter crypto and security defaults. Test legacy clients. |
| Containers | Prefer matching UBI base images to the target generation where available. |
| Network config | Use NetworkManager and `nmcli`; avoid legacy network scripts. |
| SELinux | Keep enforcing and fix labels, booleans, and port contexts instead of disabling. |

## Migration Reminder

Before moving a recipe from RHEL 9 to RHEL 10:

```bash
cat /etc/redhat-release
dnf repolist
rpm -q <package>
systemctl status <service>
update-crypto-policies --show
getenforce
```

## Source Anchors

- RHEL 10 documentation: <https://docs.redhat.com/en/documentation/red-hat-enterprise-linux>
- RHEL 9 documentation: <https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9>

