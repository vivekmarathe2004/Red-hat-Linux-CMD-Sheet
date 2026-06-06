# Lab: Networking Basics

## Objective

Inspect addresses, routes, DNS, interfaces, and NetworkManager connection profiles.

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

## Interview Takeaway

Separate IP, route, DNS, firewall, and service-listening problems instead of calling everything a network issue.

