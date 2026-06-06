# Lab: SSH Remote Access

## Scenario Context

Practice this lab on a disposable RHEL VM. Treat it like a small work ticket: understand the goal, make the change, verify it, and clean up after yourself.

By the end, you should be able to explain what changed, where the configuration lives, and how you would troubleshoot the same task if it failed.

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

## Common Lab Mistakes

- Copying placeholders such as `<user>`, `<device>`, or `<service>` without replacing them.
- Forgetting to verify the result after each task.
- Leaving test users, packages, services, or mounts behind after cleanup.
- Practicing only the success path and never checking logs when something fails.

## Interview Takeaway

Before restarting SSH remotely, keep an existing session open and run `sshd -t`.

