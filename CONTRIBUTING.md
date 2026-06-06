# Contributing

Contributions should keep this repository practical, accurate, and easy to scan.

## Style

- Use Markdown.
- Prefer tested RHEL 9 and RHEL 10 commands.
- Mark destructive operations clearly.
- Use placeholders for environment-specific values: `<hostname>`, `<interface>`, `<device>`, `<mountpoint>`, `<user>`.
- Prefer current tools: `dnf`, `systemctl`, `firewall-cmd`, `semanage`, `nmcli`, `podman`, `journalctl`.
- Include verification commands after configuration steps.

## Command Sheet Format

```md
# Topic Name

## Purpose

## Important Files

## Common Commands

## Configuration Workflow

## Verify

## Troubleshooting

## RHEL 9 / RHEL 10 Notes
```

## Review Checklist

- Links work from `README.md`.
- Commands use fenced `bash` blocks.
- Dangerous commands include a warning.
- RHEL version differences are called out only when useful.

