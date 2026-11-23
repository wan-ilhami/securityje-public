# securityje

**Security posture for company laptops, and for one household's home network.**

securityje shows a company how safe its laptops are. A small agent on each macOS and Windows laptop checks the
machine and the Wi-Fi it is on, reports to a web dashboard, and warns the person using the laptop. Admins get alerts
and a score per device. Each company sees only its own devices. A separate Home service does the same job for one
household's router and Wi-Fi.

## Screenshots

Screenshots are to come.

## What's inside

- **Laptop checks** — firewall, disk encryption, system integrity, antivirus, updates, screen lock, remote access and
  similar settings, scored per scan. Admins can switch checks off or change thresholds through a policy.
- **Network checks** — Wi-Fi encryption, signs of a tampered gateway, unexpected DNS servers and untrusted networks.
  The agent only reads what the laptop already sees; it does not scan other devices on a company network.
- **Dashboard** — device list, score history, open alerts, a per-device report, evidence export and an executive
  summary. Roles (owner, admin, viewer) control who can change what.
- **Alerts** — email to company admins and a desktop notice for the person on the laptop. A repeated failure does not
  raise a second alert, and a fixed one resolves on its own.
- **Fleet and lifecycle** — enrolment with short-lived tokens, rollout of new agent versions, offboarding checks for
  returned or wiped devices, and an optional admin-requested rescan.
- **Vulnerability and reporting views** — known-weakness findings ranked by severity, ticketing with response
  targets, and scheduled reports.
- **Home service** — for a network the owner marks as their own, a router and Wi-Fi health view, coverage and
  dead-zone readings, and a list of connected smart devices.
- **Sign-in** — single sign-on through the author's own identity service. securityje stores no passwords.

## Architecture

See [`ARCHITECTURE.md`](./ARCHITECTURE.md).

## What this repository is

This is the **public** showcase of securityje. The application code lives in a private repository. This repo holds
only the product description, the architecture overview and the version table, with screenshots to follow. It holds
no source code, keys, tokens, internal URLs or personal data.
