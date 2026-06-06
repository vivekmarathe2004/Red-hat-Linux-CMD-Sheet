# MariaDB / MySQL

> **Server Recipe** | [Home](../README.md) | [Section Index](README.md) | [Labs](../labs/README.md) | [Scenarios](../scenarios/README.md)

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Install and manage a MariaDB-compatible SQL database server.

## Architecture Notes

Think of this service in layers: package, configuration, systemd unit, listening socket, firewall rule, SELinux policy, logs, and client test. A failure in any layer can look like the service is down.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/my.cnf` | Main client/server config |
| `/etc/my.cnf.d/` | Drop-in configs |
| `/var/lib/mysql/` | Database datadir |
| `/var/log/mariadb/` | Logs where configured |

## Command Walkthrough

Read these as actions, not only commands. Each line says what you are trying to prove or change.

- **Install**: `sudo dnf install mariadb-server` - Server package
- **Enable**: `sudo systemctl enable --now mariadb` - Start at boot
- **Secure setup**: `sudo mariadb-secure-installation` - Interactive hardening
- **Login**: `sudo mariadb` - Local admin login
- **Dump DB**: `mariadb-dump <db> > <db>.sql` - Backup
- **Restore DB**: `mariadb <db> < <db>.sql` - Restore
- **Logs**: `sudo journalctl -u mariadb` - Service logs

## Safe Change Pattern

Back up config files, validate syntax when a validator exists, reload instead of restart when safe, and test from both localhost and a remote client.

## Configuration Workflow

```bash
sudo dnf install mariadb-server
sudo systemctl enable --now mariadb
sudo mariadb-secure-installation

sudo mariadb
```

SQL example:

```sql
CREATE DATABASE appdb;
CREATE USER 'appuser'@'localhost' IDENTIFIED BY '<password>';
GRANT ALL PRIVILEGES ON appdb.* TO 'appuser'@'localhost';
FLUSH PRIVILEGES;
```

Open remote access only when required:

```bash
sudo firewall-cmd --add-service=mysql --permanent
sudo firewall-cmd --reload
```

## Verify

```bash
systemctl status mariadb
sudo ss -tulpn | grep ':3306'
sudo mariadb -e "SHOW DATABASES;"
```

## Common Service Mistakes

- Opening a firewall port before confirming the service is listening.
- Restarting a service before validating the config file.
- Forgetting SELinux labels or booleans for custom paths and proxy behavior.
- Testing only from localhost when the real users connect remotely.

## Troubleshooting

Work from the symptom to evidence, then to the smallest safe fix.

- **Cannot start**: check `journalctl -u mariadb`, then Fix datadir, config, or permissions.
- **Remote denied**: check SQL host grants, then Grant user from correct host.
- **Port closed**: check `firewall-cmd --list-all`, then Open `mysql` only if needed.

## RHEL 9 / RHEL 10 Notes

Database major versions can differ by RHEL release and module/application stream.

## Page Navigation

[Servers Index](README.md) | [Web Lab](../labs/web-server.md) | [Service Scenario](../scenarios/service-down.md)
