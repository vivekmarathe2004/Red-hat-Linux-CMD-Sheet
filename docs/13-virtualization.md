# Virtualization

## Purpose

Install and manage KVM/libvirt virtualization hosts and virtual machines.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/libvirt/` | libvirt configuration |
| `/var/lib/libvirt/images/` | Default VM disk storage |
| `/var/log/libvirt/` | libvirt logs |
| `/etc/qemu-kvm/` | QEMU/KVM configuration |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install virt stack | `sudo dnf group install "Virtualization Host"` | Package group if available |
| Install tools | `sudo dnf install virt-install virt-viewer libvirt qemu-kvm` | Core tools |
| Enable libvirt | `sudo systemctl enable --now libvirtd` | Start host service |
| List VMs | `sudo virsh list --all` | VM inventory |
| Start VM | `sudo virsh start <vm>` | Boot VM |
| Stop VM | `sudo virsh shutdown <vm>` | Graceful shutdown |
| Force stop | `sudo virsh destroy <vm>` | Last resort |
| Autostart | `sudo virsh autostart <vm>` | Start at host boot |
| Console | `sudo virsh console <vm>` | Serial console |
| Pools | `sudo virsh pool-list --all` | Storage pools |
| Networks | `sudo virsh net-list --all` | Virtual networks |

## Configuration Workflow

```bash
# Install and start virtualization services
sudo dnf install virt-install virt-viewer libvirt qemu-kvm
sudo systemctl enable --now libvirtd

# Create a VM
sudo virt-install \
  --name <vm> \
  --memory 4096 \
  --vcpus 2 \
  --disk size=40 \
  --os-variant <variant> \
  --cdrom <iso-path> \
  --network network=default
```

## Verify

```bash
lsmod | grep kvm
sudo systemctl status libvirtd
sudo virsh list --all
sudo virsh net-list --all
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| KVM unavailable | `lscpu | grep Virtualization` | Enable virtualization in firmware |
| Default network down | `virsh net-list --all` | Start and autostart default network |
| VM no console | VM install options | Configure serial console in guest |

## RHEL 9 / RHEL 10 Notes

Package group names and OS variant names can differ. Use `osinfo-query os` when available.

