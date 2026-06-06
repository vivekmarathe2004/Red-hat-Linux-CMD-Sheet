# Red Hat Linux Command Sheets

GitHub-ready RHEL 9 and RHEL 10 command sheets, server recipes, study syllabus, notes, and interview preparation.

This `README.md` is only the repo overview. Full notes, syllabus content, command references, and interview answers are stored in separate directories.

## Where Is What

| Path | What It Contains | Use It For |
| --- | --- | --- |
| [docs/](docs/README.md) | Topic-wise RHEL administration command sheets | Learning and daily admin work |
| [servers/](servers/README.md) | Server setup recipes | Configuring services like Apache, Nginx, DNS, DHCP, NFS, Samba, databases, mail, logs, and Cockpit |
| [cheatsheets/](cheatsheets/README.md) | Fast lookup tables | Quick command, path, port, service, and config-file reference |
| [labs/](labs/README.md) | Hands-on practice labs | VM-based practice for job prep |
| [scenarios/](scenarios/README.md) | Troubleshooting scenarios | Real-world diagnosis and interview scenarios |
| [notes/](notes/) | Practical admin notes | Revision notes, safety patterns, troubleshooting habits |
| [syllabus/](syllabus/README.md) | Full syllabus-style learning path | Structured study from basics to troubleshooting |
| [interview/](interview/) | Interview questions and answers | Linux/RHEL interview preparation |

## Start Here

| Goal | Open |
| --- | --- |
| Learn RHEL step by step | [syllabus/rhel-linux-full-syllabus.md](syllabus/rhel-linux-full-syllabus.md) |
| Find a command quickly | [cheatsheets/command-index.md](cheatsheets/command-index.md) |
| Practice hands-on labs | [labs/README.md](labs/README.md) |
| Practice troubleshooting | [scenarios/README.md](scenarios/README.md) |
| Read practical admin notes | [notes/general-notes.md](notes/general-notes.md) |
| Prepare for interviews | [interview/rhel-linux-interview-q-and-a.md](interview/rhel-linux-interview-q-and-a.md) |
| Configure a server | [servers/README.md](servers/README.md) |
| Troubleshoot a system | [docs/15-troubleshooting.md](docs/15-troubleshooting.md) |

## Main Sections

| Section | Entry Page |
| --- | --- |
| Core administration | [docs/README.md](docs/README.md) |
| Server recipes | [servers/README.md](servers/README.md) |
| Cheatsheets | [cheatsheets/README.md](cheatsheets/README.md) |
| Hands-on labs | [labs/README.md](labs/README.md) |
| Troubleshooting scenarios | [scenarios/README.md](scenarios/README.md) |
| Notes | [notes/README.md](notes/README.md) |
| Syllabus | [syllabus/README.md](syllabus/README.md) |
| Interview prep | [interview/README.md](interview/README.md) |

## Safety

Many commands in this repository change system state. Test in a lab before production use, and replace placeholders such as `<hostname>`, `<interface>`, `<device>`, `<mountpoint>`, `<user>`, and `<service>` before running commands.

## Official References

- RHEL 10 documentation: <https://docs.redhat.com/en/documentation/red-hat-enterprise-linux>
- RHEL 9 documentation: <https://docs.redhat.com/en/documentation/red_hat_enterprise_linux/9>
