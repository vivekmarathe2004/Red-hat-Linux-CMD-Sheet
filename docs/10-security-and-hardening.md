# Security And Hardening

## Purpose

Apply practical hardening for updates, authentication, auditing, crypto policy, sudo, and service exposure.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/ssh/sshd_config` | SSH daemon config |
| `/etc/sudoers` | Sudo policy |
| `/etc/sudoers.d/` | Sudo drop-ins |
| `/etc/audit/rules.d/` | Audit rules |
| `/etc/crypto-policies/config` | System crypto policy |
| `/etc/security/pwquality.conf` | Password quality policy |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Update system | `sudo dnf update` | Security baseline |
| Show crypto policy | `update-crypto-policies --show` | Current profile |
| Set crypto policy | `sudo update-crypto-policies --set DEFAULT` | Reboot recommended |
| Audit status | `sudo auditctl -s` | Audit daemon state |
| List open ports | `sudo ss -tulpn` | Exposed services |
| Firewall services | `sudo firewall-cmd --list-all` | Allowed traffic |
| Sudo validation | `sudo visudo -c` | Syntax check |
| Password aging | `sudo chage -l <user>` | Account policy |
| Lock account | `sudo usermod -L <user>` | Disable password login |

## Configuration Workflow

```bash
# Install security tooling
sudo dnf install audit aide policycoreutils-python-utils

# Enable auditing
sudo systemctl enable --now auditd

# Initialize AIDE database
sudo aide --init

# Review exposed services
sudo ss -tulpn
sudo firewall-cmd --list-all
```

## Verify

```bash
getenforce
sudo auditctl -s
update-crypto-policies --show
sudo visudo -c
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Hardening broke legacy app | `update-crypto-policies --show` | Use approved compatibility policy only when required |
| Sudo broken | `sudo visudo -c` | Fix syntax from root or rescue |
| Service unexpectedly exposed | `ss -tulpn` | Stop, disable, or firewall service |

## RHEL 9 / RHEL 10 Notes

Default crypto and security baselines can become stricter in newer RHEL versions. Test older applications before moving them to RHEL 10.

