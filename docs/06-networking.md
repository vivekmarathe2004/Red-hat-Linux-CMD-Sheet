# Networking

## What This Means

This topic is part of the daily RHEL administrator workflow. Learn what the feature controls, which files or services own it, and which command proves the current state.

Use the commands as tools for evidence. A strong admin does not only run a command; they explain what the output proves and what they would check next.

## Purpose

Configure hostnames, IP addresses, routes, DNS clients, and NetworkManager connections.

## Important Files

| Path | Purpose |
| --- | --- |
| `/etc/NetworkManager/system-connections/` | NetworkManager connection profiles |
| `/etc/hosts` | Static name mapping |
| `/etc/resolv.conf` | Resolver configuration, often managed |
| `/etc/hostname` | Persistent hostname |

## Common Commands

| Task | Command | Notes |
| --- | --- | --- |
| Show addresses | `ip addr` | Interface addresses |
| Show routes | `ip route` | Routing table |
| Show links | `ip link` | Interface state |
| NM status | `nmcli general status` | NetworkManager state |
| List connections | `nmcli connection show` | Saved profiles |
| Show devices | `nmcli device status` | Device mapping |
| Bring up | `sudo nmcli connection up <name>` | Activate profile |
| Bring down | `sudo nmcli connection down <name>` | Deactivate profile |
| DNS lookup | `dig <name>` | Install `bind-utils` if missing |
| Trace route | `tracepath <host>` | Path test |
| Socket list | `ss -tulpn` | Listening ports |

## Configuration Workflow

```bash
# Set static IPv4 address
sudo nmcli connection modify <connection> \
  ipv4.addresses <ip>/<prefix> \
  ipv4.gateway <gateway> \
  ipv4.dns "<dns1> <dns2>" \
  ipv4.method manual

sudo nmcli connection up <connection>

# Set hostname
sudo hostnamectl set-hostname <hostname>
```

## Verify

```bash
ip addr show <interface>
ip route
nmcli connection show <connection>
resolvectl status 2>/dev/null || cat /etc/resolv.conf
ping -c 4 <gateway>
```

## Troubleshooting

| Problem | Check | Fix |
| --- | --- | --- |
| No IP address | `nmcli device status` | Bring connection up or fix profile |
| DNS failing | `dig <name>` | Fix DNS servers in NM profile |
| Port unreachable | `ss -tulpn` and `firewall-cmd --list-all` | Start service and open firewall |

## Common Mistakes

- Running commands without confirming the target host, service, path, or device.
- Changing configuration without making a quick backup first.
- Skipping verification and assuming the command worked.
- Treating permission, firewall, SELinux, DNS, and service failures as the same problem.

## Interview Takeaway

A strong answer explains the concept, names the command, and says how you would verify the output. For Networking, practice saying what you check first and why.

## RHEL 9 / RHEL 10 Notes

NetworkManager is the standard management path. Avoid editing legacy network scripts on modern RHEL systems.

