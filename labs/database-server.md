# Lab: Database Server

## Objective

Install and verify a basic database service.

## Requirements

- RHEL lab VM
- Choose MariaDB or PostgreSQL

## Tasks

1. Install one database server.
2. Start and enable it.
3. Create a test database.
4. Verify local access.
5. Review service logs.

## MariaDB Commands

```bash
sudo dnf install mariadb-server
sudo systemctl enable --now mariadb
sudo mariadb-secure-installation
sudo mariadb -e "CREATE DATABASE labdb;"
sudo mariadb -e "SHOW DATABASES;"
sudo journalctl -u mariadb -b --no-pager
```

## PostgreSQL Commands

```bash
sudo dnf install postgresql-server postgresql
sudo postgresql-setup --initdb
sudo systemctl enable --now postgresql
sudo -iu postgres psql -c "CREATE DATABASE labdb;"
sudo -iu postgres psql -c "\\l"
sudo journalctl -u postgresql -b --no-pager
```

## Verification

```bash
systemctl status mariadb || systemctl status postgresql
sudo ss -tulpn | grep -E '3306|5432'
```

## Cleanup

```bash
sudo systemctl disable --now mariadb 2>/dev/null
sudo systemctl disable --now postgresql 2>/dev/null
```

## Interview Takeaway

Database remote access usually requires service listen config, database auth rules, firewall rules, and SELinux awareness.

