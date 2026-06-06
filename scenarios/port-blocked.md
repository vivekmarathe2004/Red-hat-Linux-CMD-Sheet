# Scenario: Port Blocked

## Symptoms

- Service works locally but remote clients cannot connect.
- `curl localhost:<port>` works.
- Remote connection times out.

## Likely Causes

- Firewalld rule missing.
- Service listening only on `127.0.0.1`.
- Network ACL or cloud security group.
- Wrong zone or interface assignment.

## Diagnostic Flow

```bash
sudo ss -tulpn | grep <port>
sudo firewall-cmd --get-active-zones
sudo firewall-cmd --list-all
ip addr
ip route
```

## Fix Options

Open a known service:

```bash
sudo firewall-cmd --add-service=<service> --permanent
sudo firewall-cmd --reload
```

Open a custom port:

```bash
sudo firewall-cmd --add-port=<port>/<proto> --permanent
sudo firewall-cmd --reload
```

If the service listens only on loopback, fix the service bind/listen configuration.

## Verification

```bash
sudo firewall-cmd --list-all
sudo ss -tulpn | grep <port>
curl http://<server>:<port>
```

## Interview Answer

“If localhost works but remote access fails, I check listen address first, then firewall zone/rules, routing, and any external network controls.”

