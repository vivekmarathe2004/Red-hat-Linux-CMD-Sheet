# NFS

## Purpose

Export directories to Linux clients using Network File System.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/exports` | NFS export definitions |
| `/etc/exports.d/` | Export drop-ins |
| `/var/lib/nfs/` | NFS state |
| `/etc/fstab` | Client persistent mounts |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install server | `sudo dnf install nfs-utils` | NFS tools |
| Enable server | `sudo systemctl enable --now nfs-server` | Start at boot |
| Export reload | `sudo exportfs -rav` | Apply exports |
| Show exports | `sudo exportfs -v` | Active exports |
| Client mount | `sudo mount -t nfs <server>:/<export> <mountpoint>` | Temporary |
| Open firewall | `sudo firewall-cmd --add-service=nfs --permanent` | NFS |

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

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Mount denied | `/etc/exports` | Fix client CIDR/options |
| Permission denied | UID/GID and SELinux | Align ownership and labels |
| Export not visible | `exportfs -rav` | Reload exports |

## RHEL 9 / RHEL 10 Notes

NFSv4 is preferred for modern deployments.

