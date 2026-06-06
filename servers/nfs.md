# NFS

> **Server Recipe** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Export directories to Linux clients using Network File System.

## Architecture Notes

Think of this service in layers: package, configuration, systemd unit, listening socket, firewall rule, SELinux policy, logs, and client test. A failure in any layer can look like the service is down.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/exports` | NFS export definitions |
| `/etc/exports.d/` | Export drop-ins |
| `/var/lib/nfs/` | NFS state |
| `/etc/fstab` | Client persistent mounts |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Install server**: `sudo dnf install nfs-utils` - NFS tools
- **Enable server**: `sudo systemctl enable --now nfs-server` - Start at boot
- **Export reload**: `sudo exportfs -rav` - Apply exports
- **Show exports**: `sudo exportfs -v` - Active exports
- **Client mount**: `sudo mount -t nfs <server>:/<export> <mountpoint>` - Temporary
- **Open firewall**: `sudo firewall-cmd --add-service=nfs --permanent` - NFS

## Safe Change Pattern

Back up config files, validate syntax when a validator exists, reload instead of restart when safe, and test from both localhost and a remote client.

## Configuration Workflow

```bash
sudo dnf install nfs-utils
sudo mkdir -p /srv/nfs/<share>
sudo chown nobody:nobody /srv/nfs/<share>
sudo chmod 0775 /srv/nfs/<share>

echo "/srv/nfs/<share> <client-cidr>(rw,sync,no_subtree_check)" | sudo tee -a /etc/exports
sudo exportfs -rav
sudo systemctl enable --now nfs-server

sudo firewall-cmd --add-service=nfs --permanent
sudo firewall-cmd --reload
```

## Verify

```bash
sudo exportfs -v
showmount -e <server>
systemctl status nfs-server
```

## Common Service Mistakes

- Opening a firewall port before confirming the service is listening.
- Restarting a service before validating the config file.
- Forgetting SELinux labels or booleans for custom paths and proxy behavior.
- Testing only from localhost when the real users connect remotely.

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Mount denied**: check `/etc/exports`, then Fix client CIDR/options.
- **Permission denied**: check UID/GID and SELinux, then Align ownership and labels.
- **Export not visible**: check `exportfs -rav`, then Reload exports.

## RHEL 9 / RHEL 10 Notes

NFSv4 is preferred for modern deployments.

## Page Navigation

[Servers Index](README.md) | [Web Lab](../labs/web-server.md) | [Service Scenario](../scenarios/service-down.md)
