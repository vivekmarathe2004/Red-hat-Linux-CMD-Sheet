# IdM / FreeIPA Overview

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Provide a quick operational overview for Red Hat Identity Management, based on FreeIPA technology.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/ipa/` | IPA client/server config |
| `/etc/sssd/sssd.conf` | SSSD configuration |
| `/var/log/ipaserver-install.log` | Server install log |
| `/var/log/ipaclient-install.log` | Client install log |
| `/var/log/sssd/` | SSSD logs |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install client | `sudo dnf install ipa-client` | Client package |
| Enroll client | `sudo ipa-client-install --mkhomedir` | Interactive enrollment |
| Install server packages | `sudo dnf install ipa-server ipa-server-dns` | Server packages |
| Server install | `sudo ipa-server-install` | Plan DNS/realm first |
| User list | `ipa user-find` | IPA CLI |
| Host list | `ipa host-find` | IPA CLI |
| SSSD status | `systemctl status sssd` | Identity client |
| ID lookup | `id <user>` | NSS/SSSD lookup |

## Configuration Workflow

Client enrollment:

```bash
sudo dnf install ipa-client
sudo ipa-client-install --mkhomedir
sudo systemctl status sssd
id <idm-user>
```

Server installation requires DNS, realm, hostname, time sync, and certificate planning before running:

```bash
sudo dnf install ipa-server ipa-server-dns
sudo ipa-server-install
```

## Verify

```bash
ipa ping
ipa user-find
id <idm-user>
getent passwd <idm-user>
systemctl status sssd
```

## Common Service Mistakes

- Opening a firewall port before confirming the service is listening.
- Restarting a service before validating the config file.
- Forgetting SELinux labels or booleans for custom paths and proxy behavior.
- Testing only from localhost when the real users connect remotely.

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Login fails | `/var/log/sssd/` | Check DNS, time, enrollment, SSSD |
| IPA CLI fails | `klist` | Obtain Kerberos ticket with `kinit` |
| Enrollment fails | Hostname and DNS | Fix FQDN forward/reverse DNS |

## RHEL 9 / RHEL 10 Notes

IdM is a design-heavy service. Build a lab first and follow the official RHEL IdM documentation for production topology.

