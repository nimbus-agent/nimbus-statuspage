# Build prompt — `nimbus-statuspage`

Copy everything below into a fresh Claude Code session opened in a clone of `nimbus-agent/nimbus-statuspage`, with the main `nimbus-agent/Nimbus` repo available locally for reference.

---

I'm building **`nimbus-statuspage`**, a generator that renders an on-call / status page from Nimbus-indexed incidents, alerts, and DORA metrics — read-only, from the local gateway.

**Read first in the main Nimbus repo so the data access is correct:**
- `packages/gateway/src/metrics/` — the DORA calculators (deployment frequency, lead time, change failure rate, MTTR).
- The skill `.claude/commands/nimbus-cicd-data-layer.md` — the `/v1/metrics/dora` HTTP surface + schema, and the `[metrics.dora.<id>]` / `[ci.service.<id>]` config.
- The "Monitoring and Incidents" section of `docs/architecture.md` — the `incident` / `alert` item types and their indexed fields.
- The skill `.claude/commands/nimbus-http-write-surface.md` — confirms the HTTP API is read-only except a deploy carve-out (you only read).

**Goal:** a correct, static, read-only status/on-call page generated from a populated local index.

**Deliverables:**
1. A generator that reads, via the gateway's IPC or read-only HTTP API: current incidents/alerts (by service + severity), on-call status, and the DORA metrics (`/v1/metrics/dora`).
2. A templated **static page**: incident/alert board + a **DORA scorecard** (the four keys per service/team).
3. Config for which services/teams to include.
4. A refresh/publish path (e.g. a GitHub Action that regenerates and publishes to GitHub Pages on a schedule).

**Acceptance criteria:**
- Against a populated local index, the generator renders a correct page (incidents/alerts + DORA scorecard) and writes **nothing** back to the gateway.
- It degrades gracefully when the gateway is unreachable or a metric is null.

**Process:** brainstorm → spec → plan first. Confirm the DORA HTTP schema and the incident/alert read methods against the real code before building. Keep it strictly read-only. Verify against a running gateway with sample data before opening the PR. License is MIT.
