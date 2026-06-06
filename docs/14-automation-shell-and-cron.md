# Automation, Shell, And Cron

## Purpose

Automate recurring tasks with shell scripts, cron, systemd timers, and basic remote loops.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/crontab` | System cron file |
| `/etc/cron.d/` | Drop-in cron jobs |
| `/var/spool/cron/` | User crontabs |
| `/etc/systemd/system/` | Systemd services and timers |
| `/usr/local/bin/` | Local admin scripts |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Edit user cron | `crontab -e` | Current user |
| List user cron | `crontab -l` | Current user |
| Remove user cron | `crontab -r` | Warning: deletes crontab |
| Run shell script | `bash <script>.sh` | Execute script |
| Make executable | `chmod +x <script>.sh` | Set execute bit |
| Check syntax | `bash -n <script>.sh` | No execution |
| Shell debug | `bash -x <script>.sh` | Trace execution |
| Timer list | `systemctl list-timers` | systemd timers |

## Configuration Workflow

```bash
# Create a script
sudo vi /usr/local/bin/<task>.sh
sudo chmod 0750 /usr/local/bin/<task>.sh

# Add a cron job
sudo vi /etc/cron.d/<task>
```

Example cron file:

```text
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin
0 2 * * * root /usr/local/bin/<task>.sh
```

## Verify

```bash
bash -n /usr/local/bin/<task>.sh
sudo run-parts --test /etc/cron.daily
sudo journalctl -u crond
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Cron did not run | `journalctl -u crond` | Fix schedule, user, path, permissions |
| Script works manually only | Environment variables | Set `PATH` and absolute paths |
| Permission denied | `ls -l <script>` | Fix mode and ownership |

## RHEL 9 / RHEL 10 Notes

Cron remains available, but systemd timers are often better for service-oriented automation.

