# Troubleshooting Scenarios

These scenarios teach diagnostic thinking. They are written for real admin work and interviews: start from symptoms, form likely causes, collect evidence, fix carefully, and verify.

## How To Work A Scenario

Do not jump straight to the fix. Use this order:

1. Confirm the symptom.
2. Check service state and logs.
3. Check ports, firewall, SELinux, DNS, storage, and packages as needed.
4. Make one fix.
5. Verify locally and remotely.
6. Explain the root cause.

## Scenario List

### Service And Access

- [Service down](service-down.md)
- [Port blocked](port-blocked.md)
- [SSH lockout](ssh-lockout.md)

### Security And Policy

- [SELinux denial](selinux-denial.md)
- [Web 403 or 502](web-403-502.md)

### Platform Problems

- [Disk full](disk-full.md)
- [Broken fstab](broken-fstab.md)
- [DNS failure](dns-failure.md)
- [Package repo issue](package-repo-issue.md)

## Universal First Commands

```bash
systemctl --failed
journalctl -p err -b --no-pager
systemctl status <service>
journalctl -u <service> -b --no-pager
sudo ss -tulpn
sudo firewall-cmd --list-all
getenforce
sudo ausearch -m AVC -ts recent
df -hT
```

