# Security Policy

## Supported versions

| Version | Supported |
| ------- | --------- |
| Latest release | Yes |
| Older releases | No |

Security fixes land on the latest release only. Upgrade before reporting if you can.

## Reporting a vulnerability

Do not open a public issue. A public issue with a working exploit is a published zero-day.

Choose one channel:

1. Open a private advisory: https://github.com/__OWNER__/__REPO__/security/advisories/new
2. Email __SECURITY_EMAIL__ with the subject `Security report: <project>`.

Include what you can:

- Affected version or commit
- Steps to reproduce
- The impact you believe this has
- Any proof of concept

## What to expect

- Acknowledgement of the report within 3 business days
- An assessment of severity and scope within 10 business days
- A fix and a published advisory for confirmed vulnerabilities within 30 days
- Credit in the advisory, unless you ask to stay anonymous

## Scope

In scope: the code in this repository, its published artifacts, and its documented deployment instructions.

Out of scope: vulnerabilities in third-party dependencies (report those upstream), and findings that require a compromised host or physical access.
