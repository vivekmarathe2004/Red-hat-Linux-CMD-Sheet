# Lab: Users And Groups

## Objective

Create local users, groups, sudo access, and account policies.

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

## Interview Takeaway

Explain the difference between primary group, supplementary groups, password lock, and sudo authorization.

