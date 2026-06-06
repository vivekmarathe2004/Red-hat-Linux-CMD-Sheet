# IdM / FreeIPA Overview

> **Server Recipe** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Provide a quick operational overview for Red Hat Identity Management, based on FreeIPA technology.

## Architecture Notes

Think of this service in layers: package, configuration, systemd unit, listening socket, firewall rule, SELinux policy, logs, and client test. A failure in any layer can look like the service is down.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/ipa/` | IPA client/server config |
| `/etc/sssd/sssd.conf` | SSSD configuration |
| `/var/log/ipaserver-install.log` | Server install log |
| `/var/log/ipaclient-install.log` | Client install log |
| `/var/log/sssd/` | SSSD logs |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Install client**: `sudo dnf install ipa-client` - Client package
- **Enroll client**: `sudo ipa-client-install --mkhomedir` - Interactive enrollment
- **Install server packages**: `sudo dnf install ipa-server ipa-server-dns` - Server packages
- **Server install**: `sudo ipa-server-install` - Plan DNS/realm first
- **User list**: `ipa user-find` - IPA CLI
- **Host list**: `ipa host-find` - IPA CLI
- **SSSD status**: `systemctl status sssd` - Identity client
- **ID lookup**: `id <user>` - NSS/SSSD lookup

## Safe Change Pattern

Back up config files, validate syntax when a validator exists, reload instead of restart when safe, and test from both localhost and a remote client.

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

Work from the symptom to evidence, then to the smallest safe fix.

- **Login fails**: check `/var/log/sssd/`, then Check DNS, time, enrollment, SSSD.
- **IPA CLI fails**: check `klist`, then Obtain Kerberos ticket with `kinit`.
- **Enrollment fails**: check Hostname and DNS, then Fix FQDN forward/reverse DNS.

## RHEL 9 / RHEL 10 Notes

IdM is a design-heavy service. Build a lab first and follow the official RHEL IdM documentation for production topology.

## Page Navigation

[Servers Index](README.md) | [Web Lab](../labs/web-server.md) | [Service Scenario](../scenarios/service-down.md)
