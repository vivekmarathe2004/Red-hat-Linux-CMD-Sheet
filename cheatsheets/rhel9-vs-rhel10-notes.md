# RHEL 9 vs RHEL 10 Notes

> **Cheatsheet** | [Home](../README.md) | [Section Index](README.md) | [Docs](../docs/README.md)

## How To Use This Sheet

Use this as a quick lookup after you understand the related concept. Tables are kept here because speed matters, but production work still requires verification and careful placeholder replacement.

## Read Before Copying

Cheatsheets are fast, but they are not a substitute for understanding. Replace placeholders, check the target host, and verify every change.

## Purpose

Track practical differences to check before copying commands between RHEL 9 and RHEL 10 systems.

RHEL 10 changes continue across minor releases. Treat this sheet as a prompt for verification, not a complete migration guide. Always check the exact target minor release and the package installed on that system.

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
| Container runtime | Podman runtime and version | `podman info`, `podman version` |
| Desktop package availability | Installed package or replacement | `dnf info <package>` |

## Practical Guidance

| Topic | Guidance |
| --- | --- |
| Repository IDs | RHEL 9 and RHEL 10 repo IDs are different. Use `subscription-manager repos --list` on the target system. |
| Package streams | Database, language runtime, and web stack versions can differ. Confirm before migration. |
| Security defaults | Newer RHEL releases can have stricter crypto and security defaults. Test legacy clients. |
| Containers | Prefer matching UBI base images to the target generation where available. |
| Network config | Use NetworkManager and `nmcli`; avoid legacy network scripts. |
| SELinux | Keep enforcing and fix labels, booleans, and port contexts instead of disabling. |

## RHEL 10 Items To Watch

| Area | Practical Note | What To Do |
| --- | --- | --- |
| Release notes | RHEL 10.2 release notes document new features, removed features, deprecated features, known issues, and fixed issues. | Check the release notes for the exact minor release before a migration or interview claim. |
| Containers | RHEL 10 removed the `runc` container runtime in favor of `crun`; rootless networking guidance also changed with Podman 5 deprecations. | Check `podman info`; after upgrades, migrate existing containers if Red Hat guidance requires it. |
| Desktop packages | Some desktop applications from earlier releases are removed or replaced in RHEL 10, such as GNOME Terminal being replaced by Ptyxis and Evince by Papers. | Do not assume GUI package names from RHEL 9 still exist. Confirm with `dnf info`. |
| Audio | The PulseAudio daemon packages are removed in RHEL 10; PipeWire is the replacement and has been the default audio daemon since RHEL 9. | Troubleshoot audio with the installed PipeWire stack instead of expecting the old daemon package. |
| Security tooling | Some graphical or installer-based compliance tools were removed in RHEL 10. | Prefer command-line compliance tools and current Red Hat documentation. |
| Virtualization | `virt-manager`, monolithic `libvirtd`, and older qcow2-v2 image format are deprecated. | Prefer Cockpit/web console where appropriate and plan for modular libvirt daemons and newer image formats. |
| System roles | Some RHEL system role variables are deprecated, such as replacing `sshd` with `sshd_config`. | Check role documentation before reusing old playbooks. |

## Migration Reminder

Before moving a recipe from RHEL 9 to RHEL 10:

```bash
cat /etc/redhat-release
dnf repolist
rpm -q <package>
systemctl status <service>
update-crypto-policies --show
getenforce
podman info
```

## Source Anchors

- RHEL 10.2 release notes: <https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/10.2_release_notes/index>
- RHEL 10 removed features: <https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/10.0_release_notes/removed-features>
- RHEL 10 deprecated features: <https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/10/html/10.0_release_notes/deprecated-features>
- RHEL 9 documentation: <https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9>

## Page Navigation

[Cheatsheets Index](README.md) | [Core Docs](../docs/README.md)
