# Virtualization

> **Core Doc** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Install and manage KVM/libvirt virtualization hosts and virtual machines.

## Why It Matters

This topic affects real server behavior. If you can explain the purpose, inspect the current state, make a safe change, and verify it, you are doing administrator work rather than memorizing syntax.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/libvirt/` | libvirt configuration |
| `/var/lib/libvirt/images/` | Default VM disk storage |
| `/var/log/libvirt/` | libvirt logs |
| `/etc/qemu-kvm/` | QEMU/KVM configuration |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Install virt stack**: `sudo dnf group install "Virtualization Host"` - Package group if available
- **Install tools**: `sudo dnf install virt-install virt-viewer libvirt qemu-kvm` - Core tools
- **Enable libvirt**: `sudo systemctl enable --now libvirtd` - Start host service
- **List VMs**: `sudo virsh list --all` - VM inventory
- **Start VM**: `sudo virsh start <vm>` - Boot VM
- **Stop VM**: `sudo virsh shutdown <vm>` - Graceful shutdown
- **Force stop**: `sudo virsh destroy <vm>` - Last resort
- **Autostart**: `sudo virsh autostart <vm>` - Start at host boot
- **Console**: `sudo virsh console <vm>` - Serial console
- **Pools**: `sudo virsh pool-list --all` - Storage pools
- **Networks**: `sudo virsh net-list --all` - Virtual networks

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

## Try It In A VM

Run the workflow on a disposable RHEL VM. Change one setting, verify the result, then undo or document what changed. This builds the habit you need for production systems.

## Verify

```bash
lsmod | grep kvm
sudo systemctl status libvirtd
sudo virsh list --all
sudo virsh net-list --all
```

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **KVM unavailable**: check `lscpu, then grep Virtualization`.
- **Default network down**: check `virsh net-list --all`, then Start and autostart default network.
- **VM no console**: check VM install options, then Configure serial console in guest.

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For Virtualization, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

Package group names and OS variant names can differ. Use `osinfo-query os` when available.

## Page Navigation

[Previous](12-containers-podman-buildah-skopeo.md) | [Docs Index](README.md) | [Next](14-automation-shell-and-cron.md)
