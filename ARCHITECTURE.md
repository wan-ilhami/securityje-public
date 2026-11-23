# Architecture

```
 laptop agent (Go)                      backend (NestJS)                      data
 ┌──────────────────────┐   HTTPS      ┌────────────────────────────┐
 │ runs checks, reports │─────────────►│ device door: device key    │──► Postgres
 │ shows desktop notice │              │ user door:   SSO session   │    (row-level isolation
 └──────────────────────┘              │ scoring, alerts, reports   │     per company)
                                       └─────────────▲──────────────┘──► email
 browser (Next.js dashboard) ─── HTTPS, same-origin ──┘
```

## Two doors into one backend

The backend is one application with two separate entrances. Dashboard users arrive with a single-sign-on session;
agents arrive with a per-device key. Each entrance has its own controllers, its own guard and its own rate limits. A
user session is useless on the device side and a device key is useless on the user side.

## Backend

- **Stack** — NestJS with Drizzle and Postgres. Request bodies are validated against an allow-list of fields, so an
  agent bug cannot send extra data.
- **Scan pipeline** — a scan is validated, stored, scored, compared with the device's previous scan, and turned into
  opened or resolved alerts. The scoring and comparison rules are pure functions with unit tests.
- **Tenant isolation** — every company-owned table is protected by row-level security as well as by checks in code.
  Tests confirm that one company cannot read or change another's data.
- **Roles** — owner, admin and viewer, defined in one permissions table that tests walk through route by route.
- **Scheduled jobs** — retention clean-up, offline summaries and weekly reports run from authenticated cron calls.

## Agent

A single static Go binary per operating system with no third-party dependencies. It installs as a system service,
runs its checks on a schedule and on network change, and verifies signed updates before applying them. It is
read-only by default and accepts no free-form commands from the server.

## Security of the product itself

- Device keys are stored only as hashes, shown once, and can be rotated or revoked.
- Enrolment tokens are single-use or limited-use and expire.
- Reports and exports go only to company members or confirmed addresses.
- Sensitive dashboard actions ask for a second factor again.
- Data is kept for a fixed period and then deleted.

## Frontend

A Next.js dashboard with Tailwind and shadcn/ui. It holds no auth logic of its own and talks to the backend through a
typed client generated from the API description. Pure logic lives in plain modules with unit tests; browser specs
check key flows and accessibility.
