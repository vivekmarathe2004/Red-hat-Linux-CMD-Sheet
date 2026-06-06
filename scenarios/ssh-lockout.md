# Scenario: SSH Lockout

## What This Usually Means

This symptom tells you one layer of the system is not matching the expected state. Do not guess from the error message alone.

Collect evidence from service state, logs, ports, firewall, SELinux, DNS, storage, and package state until the failing layer is clear.

## Symptoms

- SSH login fails after config or firewall changes.
- Existing sessions still work.
- New sessions are refused or denied.

## Likely Causes

- Bad `sshd_config`.
- SSH service stopped.
- Firewall rule removed.
- User account locked.
- Key permissions wrong.
- PAM or SELinux issue.

## Decision Flow

Start broad, then narrow down. If the service is not running, read service logs. If it is running but unreachable, check listen address and firewall. If permissions look correct but access fails, check SELinux. If names fail but IPs work, check DNS.

## Diagnostic Flow

```bash
sudo sshd -t
systemctl status sshd
sudo ss -tulpn | grep ':22'
sudo firewall-cmd --list-all
sudo tail -f /var/log/secure
sudo journalctl -u sshd -b --no-pager
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
```

## Fix Options

- Keep the existing session open.
- Fix config syntax before restart.
- Reopen firewall.
- Fix key file permissions.

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
sudo sshd -t
sudo systemctl restart sshd
```

## Verification

```bash
ssh -v <user>@<host>
systemctl status sshd
```

## What To Remember

A good troubleshooting answer is not just a fix. It explains the evidence that led to the fix and the command used to verify recovery.

## Interview Answer

"I never restart SSH blindly on a remote server. I keep a working session open, validate with `sshd -t`, check firewall and logs, then restart."

