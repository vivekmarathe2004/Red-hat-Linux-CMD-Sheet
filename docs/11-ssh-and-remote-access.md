# SSH And Remote Access

> **Core Doc** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Configure SSH access, keys, daemon settings, and remote command/file workflows.

## Why It Matters

This topic affects real server behavior. If you can explain the purpose, inspect the current state, make a safe change, and verify it, you are doing administrator work rather than memorizing syntax.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/ssh/sshd_config` | SSH server configuration |
| `/etc/ssh/sshd_config.d/` | SSH server drop-ins |
| `~/.ssh/authorized_keys` | Allowed public keys |
| `~/.ssh/config` | Client aliases |
| `/var/log/secure` | SSH authentication logs |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Connect**: `ssh <user>@<host>` - Login
- **Generate key**: `ssh-keygen -t ed25519` - Modern key type
- **Copy key**: `ssh-copy-id <user>@<host>` - Installs public key
- **Copy file**: `scp <file> <user>@<host>:<path>` - Simple copy
- **Sync files**: `rsync -av <src> <user>@<host>:<dst>` - Efficient copy
- **Test config**: `sudo sshd -t` - Validate daemon config
- **Restart SSH**: `sudo systemctl restart sshd` - Apply changes
- **SSH logs**: `sudo journalctl -u sshd` - Service logs

## Configuration Workflow

```bash
# Install and enable SSH server

sudo dnf install openssh-server
sudo systemctl enable --now sshd

# Open firewall

sudo firewall-cmd --add-service=ssh --permanent
sudo firewall-cmd --reload

# Validate config before restart

sudo sshd -t
sudo systemctl restart sshd
```

## Try It In A VM

Run the workflow on a disposable RHEL VM. Change one setting, verify the result, then undo or document what changed. This builds the habit you need for production systems.

## Verify

```bash
systemctl status sshd
sudo ss -tulpn | grep ':22'
ssh -v <user>@<host>
```

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Login denied**: check `/var/log/secure`, then Fix user, key, PAM, or policy.
- **Config typo**: check `sudo sshd -t`, then Correct config before restart.
- **Cannot connect**: check `ss -tulpn` and `firewall-cmd --list-all`, then Start SSH and open firewall.

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For SSH And Remote Access, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

Crypto policies can affect SSH algorithms. Avoid enabling weak legacy algorithms unless there is a documented business requirement.

## Page Navigation

[Previous](10-security-and-hardening.md) | [Docs Index](README.md) | [Next](12-containers-podman-buildah-skopeo.md)
