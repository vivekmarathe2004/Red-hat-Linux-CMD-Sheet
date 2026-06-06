# Lab: Users And Groups

> **Lab** | [Home](../README.md) | [Section Index](README.md) | [Docs](../docs/README.md) | [Scenarios](../scenarios/README.md)

## Scenario Context

Practice this lab on a disposable RHEL VM. Treat it like a small work ticket: understand the goal, make the change, verify it, and clean up after yourself.

By the end, you should be able to explain what changed, where the configuration lives, and how you would troubleshoot the same task if it failed.

## Objective

Create local users, groups, sudo access, and account policies.

## Why This Lab Matters

This lab turns a common admin task into muscle memory. The important part is not just reaching the final state, but proving that the system is actually configured correctly.

## Requirements

- RHEL 9 or RHEL 10 lab VM
- Root or sudo access

## Tasks

1. Create a user named `<student>`.
2. Create a group named `<admins>`.
3. Add the user to the group.
4. Grant sudo through the `wheel` group.
5. Lock and unlock the account.
6. Check password aging.

## Commands

```bash
sudo useradd <student>
sudo passwd <student>
sudo groupadd <admins>
sudo usermod -aG <admins> <student>
sudo usermod -aG wheel <student>
id <student>
sudo chage -l <student>
sudo usermod -L <student>
sudo passwd -S <student>
sudo usermod -U <student>
sudo -l -U <student>
```

## Verification

```bash
getent passwd <student>
getent group <admins>
id <student>
sudo -l -U <student>
```

## Cleanup

```bash
sudo userdel -r <student>
sudo groupdel <admins>
```

## Common Lab Mistakes

- Copying placeholders such as `<user>`, `<device>`, or `<service>` without replacing them.
- Forgetting to verify the result after each task.
- Leaving test users, packages, services, or mounts behind after cleanup.
- Practicing only the success path and never checking logs when something fails.

## Review Questions

After the lab, answer these out loud: what changed, which command changed it, where is it stored, how did you verify it, and what would fail if it were misconfigured?

## Interview Takeaway

Explain the difference between primary group, supplementary groups, password lock, and sudo authorization.

## Page Navigation

Previous | [Labs Index](README.md) | [Next](permissions-and-acls.md)
