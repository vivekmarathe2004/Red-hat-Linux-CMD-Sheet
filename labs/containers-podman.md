# Lab: Containers With Podman

## Objective

Run and troubleshoot a rootless Podman container.

## Requirements

- RHEL lab VM
- Podman installed

## Tasks

1. Install Podman.
2. Pull a UBI image.
3. Run a container.
4. Inspect logs.
5. Stop and remove the container.

## Commands

```bash
sudo dnf install podman
podman pull registry.access.redhat.com/ubi9/ubi
podman run --name ubi-test registry.access.redhat.com/ubi9/ubi cat /etc/os-release
podman ps -a
podman logs ubi-test
podman rm ubi-test
```

Web-style test:

```bash
podman run -d --name web-test -p 8080:8080 registry.access.redhat.com/ubi9/httpd-24
podman ps
curl http://localhost:8080
podman logs web-test
```

## Verification

```bash
podman ps -a
podman images
```

## Cleanup

```bash
podman stop web-test
podman rm web-test
```

## Interview Takeaway

Podman is daemonless and supports rootless containers, which is a key difference from older Docker-centered workflows.

