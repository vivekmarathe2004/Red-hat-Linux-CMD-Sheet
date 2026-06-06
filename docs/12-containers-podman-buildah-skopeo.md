# Containers With Podman, Buildah, And Skopeo

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Build, run, inspect, and manage containers using RHEL-native container tools.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/containers/registries.conf` | Registry search and policy |
| `/etc/containers/storage.conf` | Container storage config |
| `~/.config/containers/` | User container config |
| `/var/lib/containers/` | Root container storage |
| `~/.local/share/containers/` | Rootless container storage |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Install tools | `sudo dnf install podman buildah skopeo` | Container stack |
| Pull image | `podman pull <image>` | Download image |
| Run container | `podman run --name <name> -p <hostport>:<containerport> <image>` | Foreground |
| Run detached | `podman run -d --name <name> <image>` | Background |
| List containers | `podman ps -a` | All containers |
| Logs | `podman logs <name>` | Container logs |
| Exec shell | `podman exec -it <name> /bin/bash` | Debug |
| Stop | `podman stop <name>` | Graceful stop |
| Remove | `podman rm <name>` | Remove container |
| Images | `podman images` | Local images |
| Build image | `podman build -t <tag> .` | Uses Containerfile |
| Inspect remote | `skopeo inspect docker://<image>` | No pull needed |

## Configuration Workflow

```bash
# Run a rootless web container
podman pull registry.access.redhat.com/ubi9/httpd-24
podman run -d --name <name> -p 8080:8080 registry.access.redhat.com/ubi9/httpd-24

# Generate a systemd user unit
mkdir -p ~/.config/systemd/user
podman generate systemd --new --files --name <name>
mv container-<name>.service ~/.config/systemd/user/
systemctl --user daemon-reload
systemctl --user enable --now container-<name>.service
```

## Verify

```bash
podman ps
podman logs <name>
curl http://localhost:8080
systemctl --user status container-<name>.service
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| Port denied rootless | `podman logs <name>` | Use high host port or rootful service |
| Image pull fails | `podman login <registry>` | Authenticate or fix registry |
| Container exits | `podman inspect <name>` | Check command, env, logs |

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For Containers With Podman, Buildah, And Skopeo, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

Image tags should match the intended base where possible, such as UBI 9 for RHEL 9 and UBI 10 for RHEL 10 when available.

