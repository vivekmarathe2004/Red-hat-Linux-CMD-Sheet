# Installation And Subscription

## Purpose

Register RHEL systems, attach subscriptions, enable repositories, and prepare systems after installation.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/rhsm/rhsm.conf` | Subscription manager configuration |
| `/etc/yum.repos.d/redhat.repo` | Red Hat managed repo file |
| `/root/anaconda-ks.cfg` | Kickstart generated from install |
| `/var/log/anaconda/` | Installer logs |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Register | `sudo subscription-manager register` | Prompts for credentials |
| Register with org key | `sudo subscription-manager register --org=<org> --activationkey=<key>` | Common for automation |
| Show status | `sudo subscription-manager status` | Checks entitlement |
| List repos | `sudo subscription-manager repos --list-enabled` | Enabled only |
| Enable repo | `sudo subscription-manager repos --enable=<repo-id>` | Use exact repo ID |
| Release lock | `sudo subscription-manager release --set=<major.minor>` | Optional version pin |
| Remove release lock | `sudo subscription-manager release --unset` | Return to latest |
| Unregister | `sudo subscription-manager unregister` | Removes registration |

## Configuration Workflow

```bash
# Register with an activation key
sudo subscription-manager register --org=<org> --activationkey=<key>

# Check status
sudo subscription-manager status

# Enable common repositories
sudo subscription-manager repos --enable=rhel-9-for-x86_64-baseos-rpms
sudo subscription-manager repos --enable=rhel-9-for-x86_64-appstream-rpms

# For RHEL 10, use the matching RHEL 10 repo IDs shown by this command
sudo subscription-manager repos --list

# Update system
sudo dnf update
```

## Verify

```bash
sudo subscription-manager identity
sudo subscription-manager repos --list-enabled
sudo dnf repolist
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| No enabled repos | `dnf repolist` | Enable BaseOS and AppStream |
| Wrong version repos | `subscription-manager release --show` | Unset or set correct release |
| Cert errors | `subscription-manager refresh` | Refresh entitlement data |

## RHEL 9 / RHEL 10 Notes

Repository IDs are version-specific. Do not copy RHEL 9 repo IDs onto RHEL 10; list available repos and enable the matching RHEL 10 BaseOS and AppStream repositories.

