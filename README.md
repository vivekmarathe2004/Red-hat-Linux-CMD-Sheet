<p align="center">
  <img src="assets/repo-banner.png" alt="Red Hat Linux Command Sheets banner" width="100%">
</p>

<h1 align="center">Red Hat Linux Command Sheets</h1>

<p align="center">
  Tutorial-style RHEL 9/10 command notes, hands-on labs, server recipes, troubleshooting scenarios, and interview preparation.
</p>

<p align="center">
  <a href="LICENSE"><img alt="License" src="https://img.shields.io/badge/license-MIT-E00?style=for-the-badge"></a>
  <img alt="RHEL" src="https://img.shields.io/badge/RHEL-9%20%7C%2010-151515?style=for-the-badge">
  <img alt="Style" src="https://img.shields.io/badge/style-tutorial%20%2B%20runbook-E00?style=for-the-badge">
</p>

---

## Start Here

Choose the path that matches what you are doing right now.

### Learn

Start with the [full syllabus](syllabus/README.md), then read the matching [core docs](docs/README.md). Each topic explains the idea first, then gives commands, verification, mistakes, and interview points.

### Practice

Use the [hands-on labs](labs/README.md) in a disposable VM. The labs are written for repetition: do the task, verify it, clean it up, then explain what happened.

### Troubleshoot

Use [scenarios](scenarios/README.md) when you want real-world diagnostic thinking: symptoms, likely causes, decision flow, fixes, and spoken interview answers.

### Reference

Use [cheatsheets](cheatsheets/README.md) when you already understand the concept and only need a command, path, port, or config file quickly.

## Repository Sections

### Core Learning

- [Core administration docs](docs/README.md): tutorial-style notes for RHEL fundamentals.
- [Full syllabus](syllabus/README.md): course path from basics to troubleshooting.
- [General notes](notes/README.md): admin mindset, safe habits, and revision drills.

### Practice And Troubleshooting

- [Hands-on labs](labs/README.md): VM-based practice tasks.
- [Troubleshooting scenarios](scenarios/README.md): realistic failure cases and diagnostic flows.
- [Server recipes](servers/README.md): deploy and verify common services.

### Fast Review

- [Cheatsheets](cheatsheets/README.md): compact lookup references.
- [Interview prep](interview/README.md): spoken answers and scenario-based questions.

## Study Flow

```text
Syllabus -> Core Docs -> Labs -> Scenarios -> Interview Q&A -> Server Recipes
```

Use this loop for every topic:

1. Read the concept.
2. Run the commands in a VM.
3. Verify the result.
4. Break one thing intentionally.
5. Troubleshoot it from logs and evidence.
6. Explain the fix out loud.

## Safety

Many commands in this repository change system state. Practice in a VM, take backups before editing config files, and replace placeholders such as `<hostname>`, `<interface>`, `<device>`, `<mountpoint>`, `<user>`, and `<service>` before running commands.

## Official References

- RHEL 10 documentation: <https://docs.redhat.com/en/documentation/red-hat-enterprise-linux>
- RHEL 9 documentation: <https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9>
