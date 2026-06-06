# Troubleshooting Scenarios

Real-world RHEL troubleshooting scenarios for students, interviews, and junior admin practice.

## Scenarios

| Scenario | Main Skill |
| --- | --- |
| [Service down](service-down.md) | systemd and logs |
| [Port blocked](port-blocked.md) | sockets and firewall |
| [SELinux denial](selinux-denial.md) | AVC diagnosis |
| [Disk full](disk-full.md) | storage triage |
| [Broken fstab](broken-fstab.md) | boot and mounts |
| [DNS failure](dns-failure.md) | resolver troubleshooting |
| [SSH lockout](ssh-lockout.md) | safe remote access |
| [Package repo issue](package-repo-issue.md) | DNF and subscription |
| [Web 403 or 502](web-403-502.md) | web service triage |

## Universal Flow

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

## Related

- [Troubleshooting docs](../docs/15-troubleshooting.md)
- [Hands-on labs](../labs/README.md)
- [Interview Q&A](../interview/rhel-linux-interview-q-and-a.md)

