# Lab: SSH Remote Access

## Objective

Configure and test SSH key-based access safely.

## Requirements

- Two lab machines or one VM plus localhost testing
- OpenSSH server

## Tasks

1. Install and enable SSH.
2. Generate an Ed25519 key.
3. Copy the public key.
4. Validate SSH daemon config.
5. Review SSH logs.

## Commands

```bash
sudo dnf install openssh-server
sudo systemctl enable --now sshd
ssh-keygen -t ed25519
ssh-copy-id <user>@<host>
ssh <user>@<host>
sudo sshd -t
sudo journalctl -u sshd -b --no-pager
sudo tail -f /var/log/secure
```

## Verification

```bash
ssh -v <user>@<host>
systemctl status sshd
sudo ss -tulpn | grep ':22'
```

## Cleanup

Remove test keys from `~/.ssh/authorized_keys` if needed.

## Interview Takeaway

Before restarting SSH remotely, keep an existing session open and run `sshd -t`.

