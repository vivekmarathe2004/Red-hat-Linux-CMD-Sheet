# MariaDB / MySQL

## Purpose

Install and manage a MariaDB-compatible SQL database server.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/my.cnf` | Main client/server config |
| `/etc/my.cnf.d/` | Drop-in configs |
| `/var/lib/mysql/` | Database datadir |
| `/var/log/mariadb/` | Logs where configured |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install | `sudo dnf install mariadb-server` | Server package |
| Enable | `sudo systemctl enable --now mariadb` | Start at boot |
| Secure setup | `sudo mariadb-secure-installation` | Interactive hardening |
| Login | `sudo mariadb` | Local admin login |
| Dump DB | `mariadb-dump <db> > <db>.sql` | Backup |
| Restore DB | `mariadb <db> < <db>.sql` | Restore |
| Logs | `sudo journalctl -u mariadb` | Service logs |

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

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Cannot start | `journalctl -u mariadb` | Fix datadir, config, or permissions |
| Remote denied | SQL host grants | Grant user from correct host |
| Port closed | `firewall-cmd --list-all` | Open `mysql` only if needed |

## RHEL 9 / RHEL 10 Notes

Database major versions can differ by RHEL release and module/application stream.

