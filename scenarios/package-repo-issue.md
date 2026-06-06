# Scenario: Package Repo Issue

## Symptoms

- `dnf install` says no match found.
- Metadata download fails.
- System has no enabled repositories.
- Package versions are unexpected.

## Likely Causes

- System not registered.
- Required repo disabled.
- Wrong RHEL release lock.
- DNS or proxy problem.
- Third-party repo conflict.

## Diagnostic Flow

```bash
cat /etc/redhat-release
sudo subscription-manager status
sudo subscription-manager release --show
sudo subscription-manager repos --list-enabled
dnf repolist --all
sudo dnf makecache
dnf info <package>
```

## Fix Options

```bash
sudo subscription-manager refresh
sudo subscription-manager repos --enable=<repo-id>
sudo subscription-manager release --unset
sudo dnf clean all
sudo dnf makecache
```

## Verification

```bash
dnf repolist
dnf info <package>
sudo dnf install <package>
```

## Interview Answer

“I check registration, enabled repos, release lock, DNS/proxy, and package name before assuming the package does not exist.”

