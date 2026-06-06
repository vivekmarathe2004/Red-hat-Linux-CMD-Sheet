# Automation, Shell, And Cron

> **Core Doc** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Automate recurring tasks with shell scripts, cron, systemd timers, and basic remote loops.

## Why It Matters

This topic affects real server behavior. If you can explain the purpose, inspect the current state, make a safe change, and verify it, you are doing administrator work rather than memorizing syntax.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/crontab` | System cron file |
| `/etc/cron.d/` | Drop-in cron jobs |
| `/var/spool/cron/` | User crontabs |
| `/etc/systemd/system/` | Systemd services and timers |
| `/usr/local/bin/` | Local admin scripts |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Edit user cron**: `crontab -e` - Current user
- **List user cron**: `crontab -l` - Current user
- **Remove user cron**: `crontab -r` - Warning: deletes crontab
- **Run shell script**: `bash <script>.sh` - Execute script
- **Make executable**: `chmod +x <script>.sh` - Set execute bit
- **Check syntax**: `bash -n <script>.sh` - No execution
- **Shell debug**: `bash -x <script>.sh` - Trace execution
- **Timer list**: `systemctl list-timers` - systemd timers

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

## Try It In A VM

Run the workflow on a disposable RHEL VM. Change one setting, verify the result, then undo or document what changed. This builds the habit you need for production systems.

## Verify

```bash
bash -n /usr/local/bin/<task>.sh
sudo run-parts --test /etc/cron.daily
sudo journalctl -u crond
```

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Cron did not run**: check `journalctl -u crond`, then Fix schedule, user, path, permissions.
- **Script works manually only**: check Environment variables, then Set `PATH` and absolute paths.
- **Permission denied**: check `ls -l <script>`, then Fix mode and ownership.

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For Automation, Shell, And Cron, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

Cron remains available, but systemd timers are often better for service-oriented automation.

## Page Navigation

[Previous](13-virtualization.md) | [Docs Index](README.md) | [Next](15-troubleshooting.md)
