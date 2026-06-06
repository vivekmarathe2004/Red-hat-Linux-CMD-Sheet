# SSH And Remote Access

## Purpose

Configure SSH access, keys, daemon settings, and remote command/file workflows.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/ssh/sshd_config` | SSH server configuration |
| `/etc/ssh/sshd_config.d/` | SSH server drop-ins |
| `~/.ssh/authorized_keys` | Allowed public keys |
| `~/.ssh/config` | Client aliases |
| `/var/log/secure` | SSH authentication logs |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Connect | `ssh <user>@<host>` | Login |
| Generate key | `ssh-keygen -t ed25519` | Modern key type |
| Copy key | `ssh-copy-id <user>@<host>` | Installs public key |
| Copy file | `scp <file> <user>@<host>:<path>` | Simple copy |
| Sync files | `rsync -av <src> <user>@<host>:<dst>` | Efficient copy |
| Test config | `sudo sshd -t` | Validate daemon config |
| Restart SSH | `sudo systemctl restart sshd` | Apply changes |
| SSH logs | `sudo journalctl -u sshd` | Service logs |

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

## Verify

```bash
systemctl status sshd
sudo ss -tulpn | grep ':22'
ssh -v <user>@<host>
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Login denied | `/var/log/secure` | Fix user, key, PAM, or policy |
| Config typo | `sudo sshd -t` | Correct config before restart |
| Cannot connect | `ss -tulpn` and `firewall-cmd --list-all` | Start SSH and open firewall |

## RHEL 9 / RHEL 10 Notes

Crypto policies can affect SSH algorithms. Avoid enabling weak legacy algorithms unless there is a documented business requirement.

