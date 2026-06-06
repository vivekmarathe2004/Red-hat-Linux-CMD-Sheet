# Scenario: DNS Failure

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

## Interview Answer

“I separate network reachability from DNS resolution: first IP route/connectivity, then resolver config, then direct queries to the DNS server.”

