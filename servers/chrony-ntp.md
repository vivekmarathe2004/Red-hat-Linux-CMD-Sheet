# Chrony / NTP

## Purpose

Synchronize system time with Chrony and optionally serve time to clients.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/chrony.conf` | Main Chrony config |
| `/var/lib/chrony/` | Chrony state |
| `/var/log/chrony/` | Chrony logs if enabled |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install | `sudo dnf install chrony` | Time sync |
| Enable | `sudo systemctl enable --now chronyd` | Start at boot |
| Sources | `chronyc sources -v` | Time sources |
| Tracking | `chronyc tracking` | Sync status |
| Step clock | `sudo chronyc makestep` | Correct large offset |
| Open NTP | `sudo firewall-cmd --add-service=ntp --permanent` | If serving clients |

## Configuration Workflow

```bash
sudo dnf install chrony
sudo vi /etc/chrony.conf
sudo systemctl enable --now chronyd
sudo systemctl restart chronyd
```

Server pool example:

```text
pool <ntp-pool-or-server> iburst
```

Allow clients example:

```text
allow <client-cidr>
```

## Verify

```bash
timedatectl
chronyc tracking
chronyc sources -v
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Unsynchronized | `chronyc sources -v` | Fix NTP source/firewall |
| Large offset | `chronyc tracking` | Use `chronyc makestep` in maintenance |
| Clients cannot sync | Firewall | Open NTP and add `allow` CIDR |

## RHEL 9 / RHEL 10 Notes

Chrony is the preferred NTP implementation on RHEL.

