# PostgreSQL

## How This Service Fits

A service is not just a package. A working deployment usually needs a valid config file, a running systemd unit, a listening port, firewall access for remote clients, and SELinux policy that matches the service behavior.

Deploy in small steps: install, configure, validate, start, open access, test locally, test remotely, then review logs.

## Purpose

Install and manage a PostgreSQL database server.

## Important Files

| Path | Purpose |
| --- | --- |
| `/var/lib/pgsql/data/` | Default database cluster |
| `/var/lib/pgsql/data/postgresql.conf` | Main server config |
| `/var/lib/pgsql/data/pg_hba.conf` | Client authentication |
| `/var/log/` | Logs through journald or configured files |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install | `sudo dnf install postgresql-server postgresql` | Server and client |
| Initialize | `sudo postgresql-setup --initdb` | First-time datadir setup |
| Enable | `sudo systemctl enable --now postgresql` | Start at boot |
| Login | `sudo -iu postgres psql` | Admin shell |
| Dump DB | `pg_dump <db> > <db>.sql` | Backup |
| Restore DB | `psql <db> < <db>.sql` | Restore |
| Logs | `sudo journalctl -u postgresql` | Service logs |

## Configuration Workflow

```bash
sudo dnf install postgresql-server postgresql
sudo postgresql-setup --initdb
sudo systemctl enable --now postgresql
sudo -iu postgres psql
```

SQL example:

```sql
CREATE DATABASE appdb;
CREATE USER appuser WITH ENCRYPTED PASSWORD '<password>';
GRANT ALL PRIVILEGES ON DATABASE appdb TO appuser;
```

For remote access, edit `postgresql.conf` and `pg_hba.conf`, then:

```bash
sudo firewall-cmd --add-service=postgresql --permanent
sudo firewall-cmd --reload
sudo systemctl restart postgresql
```

## Verify

```bash
systemctl status postgresql
sudo ss -tulpn | grep ':5432'
sudo -iu postgres psql -c "\l"
```

## Common Service Mistakes

- Opening a firewall port before confirming the service is listening.
- Restarting a service before validating the config file.
- Forgetting SELinux labels or booleans for custom paths and proxy behavior.
- Testing only from localhost when the real users connect remotely.

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Auth failed | `pg_hba.conf` | Match database, user, address, method |
| Not listening remotely | `postgresql.conf` | Set `listen_addresses` |
| Cluster not initialized | Datadir contents | Run `postgresql-setup --initdb` once |

## RHEL 9 / RHEL 10 Notes

PostgreSQL major versions can differ. Check `psql --version` before migrations.

