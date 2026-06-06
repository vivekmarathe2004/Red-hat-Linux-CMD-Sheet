# Lab: Networking Basics

> **Lab** | [Home](../README.md) | [Section Index](README.md) | [Docs](../docs/README.md) | [Scenarios](../scenarios/README.md)

## Scenario Context

Practice this lab on a disposable RHEL VM. Treat it like a small work ticket: understand the goal, make the change, verify it, and clean up after yourself.

By the end, you should be able to explain what changed, where the configuration lives, and how you would troubleshoot the same task if it failed.

## Objective

Inspect addresses, routes, DNS, interfaces, and NetworkManager connection profiles.

## Why This Lab Matters

This lab turns a common admin task into muscle memory. The important part is not just reaching the final state, but proving that the system is actually configured correctly.

## Requirements

- RHEL lab VM
- Network connection

## Tasks

1. Identify interfaces and IP addresses.
2. Check routing.
3. Inspect NetworkManager profiles.
4. Test DNS resolution.
5. Find listening ports.

## Commands

```bash
ip addr
ip route
ip link
nmcli general status
nmcli device status
nmcli connection show
getent hosts redhat.com
dig redhat.com
tracepath redhat.com
sudo ss -tulpn
```

## Verification

```bash
ping -c 4 <gateway>
getent hosts <hostname>
nmcli device status
```

## Cleanup

No cleanup required if no connection profiles were changed.

## Common Lab Mistakes

- Copying placeholders such as `<user>`, `<device>`, or `<service>` without replacing them.
- Forgetting to verify the result after each task.
- Leaving test users, packages, services, or mounts behind after cleanup.
- Practicing only the success path and never checking logs when something fails.

## Review Questions

After the lab, answer these out loud: what changed, which command changed it, where is it stored, how did you verify it, and what would fail if it were misconfigured?

## Interview Takeaway

Separate IP, route, DNS, firewall, and service-listening problems instead of calling everything a network issue.

## Page Navigation

[Previous](systemd-services.md) | [Labs Index](README.md) | [Next](firewall-and-selinux.md)
