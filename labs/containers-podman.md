# Lab: Containers With Podman

> **Lab** | [Home](../README.md) | [Section Index](README.md) | [Docs](../docs/README.md) | [Scenarios](../scenarios/README.md)

## Scenario Context

Practice this lab on a disposable RHEL VM. Treat it like a small work ticket: understand the goal, make the change, verify it, and clean up after yourself.

By the end, you should be able to explain what changed, where the configuration lives, and how you would troubleshoot the same task if it failed.

## Objective

Run and troubleshoot a rootless Podman container.

## Why This Lab Matters

This lab turns a common admin task into muscle memory. The important part is not just reaching the final state, but proving that the system is actually configured correctly.

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

## Common Lab Mistakes

- Copying placeholders such as `<user>`, `<device>`, or `<service>` without replacing them.
- Forgetting to verify the result after each task.
- Leaving test users, packages, services, or mounts behind after cleanup.
- Practicing only the success path and never checking logs when something fails.

## Review Questions

After the lab, answer these out loud: what changed, which command changed it, where is it stored, how did you verify it, and what would fail if it were misconfigured?

## Interview Takeaway

Podman is daemonless and supports rootless containers, which is a key difference from older Docker-centered workflows.

## Page Navigation

[Previous](ssh-remote-access.md) | [Labs Index](README.md) | [Next](web-server.md)
