# Scenario: DNS Failure

## What This Usually Means

This symptom tells you one layer of the system is not matching the expected state. Do not guess from the error message alone.

Collect evidence from service state, logs, ports, firewall, SELinux, DNS, storage, and package state until the failing layer is clear.

## Symptoms

- Ping by IP works, but hostname fails.
- Package installs fail due to name resolution.
- Applications cannot resolve service names.

## Likely Causes

- Wrong DNS server.
- NetworkManager profile DNS misconfigured.
- Broken `/etc/resolv.conf`.
- DNS server unreachable.
- Search domain issue.

## Decision Flow

Start broad, then narrow down. If the service is not running, read service logs. If it is running but unreachable, check listen address and firewall. If permissions look correct but access fails, check SELinux. If names fail but IPs work, check DNS.

## Diagnostic Flow

```bash
ip addr
ip route
nmcli connection show
nmcli device show <interface>
cat /etc/resolv.conf
getent hosts <hostname>
dig <hostname>
dig @<dns-server> <hostname>
```

## Fix Options

Set DNS through NetworkManager:

```bash
sudo nmcli connection modify <connection> ipv4.dns "<dns1> <dns2>"
sudo nmcli connection up <connection>
```

## Verification

```bash
getent hosts redhat.com
dig redhat.com
sudo dnf makecache
```

## What To Remember

A good troubleshooting answer is not just a fix. It explains the evidence that led to the fix and the command used to verify recovery.

## Interview Answer

"I separate network reachability from DNS resolution: first IP route/connectivity, then resolver config, then direct queries to the DNS server."

