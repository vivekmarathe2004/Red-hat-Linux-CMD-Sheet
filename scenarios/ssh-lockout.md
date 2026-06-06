# Scenario: SSH Lockout

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

## Interview Answer

“I never restart SSH blindly on a remote server. I keep a working session open, validate with `sshd -t`, check firewall and logs, then restart.”

