# systemd Services And Boot

> **Core Doc** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Control services, inspect boot targets, manage unit files, and troubleshoot startup issues.

## Why It Matters

This topic affects real server behavior. If you can explain the purpose, inspect the current state, make a safe change, and verify it, you are doing administrator work rather than memorizing syntax.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/systemd/system/` | Local unit files and overrides |
| `/usr/lib/systemd/system/` | Package-provided unit files |
| `/etc/systemd/system/*.d/` | Drop-in overrides |
| `/etc/default/grub` | GRUB defaults |
| `/boot/grub2/grub.cfg` | BIOS GRUB config |
| `/boot/efi/EFI/redhat/grub.cfg` | UEFI GRUB config |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Service status**: `systemctl status <service>` - Runtime state
- **Start service**: `sudo systemctl start <service>` - Current boot only
- **Stop service**: `sudo systemctl stop <service>` - Current boot only
- **Enable service**: `sudo systemctl enable <service>` - Start at boot
- **Enable now**: `sudo systemctl enable --now <service>` - Enable and start
- **Disable service**: `sudo systemctl disable <service>` - Do not start at boot
- **Restart service**: `sudo systemctl restart <service>` - Reload process
- **Reload unit files**: `sudo systemctl daemon-reload` - After unit changes
- **List failed**: `systemctl --failed` - Failed units
- **Default target**: `systemctl get-default` - Boot target
- **Set target**: `sudo systemctl set-default multi-user.target` - CLI boot

## Configuration Workflow

```bash
# Create a service override

sudo systemctl edit <service>

# Apply and restart

sudo systemctl daemon-reload
sudo systemctl restart <service>

# View logs for the unit

journalctl -u <service> -b
```

## Try It In A VM

Run the workflow on a disposable RHEL VM. Change one setting, verify the result, then undo or document what changed. This builds the habit you need for production systems.

## Verify

```bash
systemctl is-enabled <service>
systemctl is-active <service>
systemctl --failed
journalctl -b -p warning
```

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Unit file changed but ignored**: check `systemctl cat <service>`, then Run `systemctl daemon-reload`.
- **Service fails at boot**: check `journalctl -u <service> -b`, then Fix config, dependency, or permissions.
- **Wrong boot target**: check `systemctl get-default`, then Set correct target.

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For systemd Services And Boot, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

Both use systemd. Bootloader paths differ by BIOS versus UEFI rather than by RHEL version.

## Page Navigation

[Previous](04-users-groups-and-permissions.md) | [Docs Index](README.md) | [Next](06-networking.md)
