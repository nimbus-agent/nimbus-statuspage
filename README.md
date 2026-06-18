# Nimbus Statuspage

Generate an on-call / status page from [**Nimbus**](https://github.com/nimbus-agent/Nimbus)-indexed incidents, alerts, and DORA metrics.

> **Status: SCAFFOLD — not yet built.** This repo holds its vision and a [build prompt](./NEW-SESSION-PROMPT.md).

## What it will be

Nimbus is "on-call intelligence for DevOps/platform teams." Its local index already holds `incident`, `alert`, and infra/deployment items (from Datadog, Grafana, PagerDuty, Sentry, New Relic, …), and the Phase 5 CI/CD data layer computes **DORA** metrics (deployment frequency, lead time, change failure rate, MTTR) exposed via IPC and a read-only HTTP API (e.g. `GET /v1/metrics/dora`).

This tool reads that data from the local gateway (**read-only**) and renders a status/on-call page:

- **Current incidents & alerts** by service and severity.
- **On-call status** — who/what is active.
- **DORA scorecard** — the four keys, per service/team.

Output is a static page that can be auto-refreshed and published (e.g. GitHub Pages or a cron job).

## Data sources & boundary

It consumes the gateway's read surfaces only — indexed incidents/alerts plus `/v1/metrics/dora`. It never writes (the HTTP API is read-only except a sanctioned deploy route it does not use; invariant I13). The metrics layer lives in the main repo under [`packages/gateway/src/metrics`](https://github.com/nimbus-agent/Nimbus/tree/main/packages/gateway/src/metrics).

## License

[MIT](./LICENSE).
