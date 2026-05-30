# Change Log — AID Helpdesk Plugin

A running log of changes made to this repository by the automated agent. Newest entries first.

---

## 2026-05-31 — `/aid:help` skill + open-source prep (v1.1.0)

**Focus:** add one high-value, read-only UX skill and make the repo open-source
ready. No changes to the existing skill behaviour or backend contract.

> ⚠️ Process note (honest record): early in this run my file reads were
> unreliable and twice returned content that did not match the real repository.
> Acting on the first bad read, I briefly committed a fictional "computer-use
> command center" — that commit was reverted with `git reset --hard` before any
> push. Acting on a second bad read, I overwrote `skills/tickets/SKILL.md` and
> `skills/users/SKILL.md` with incorrect simplified versions (wrong endpoints and
> auth header); those were restored from `HEAD` with `git checkout` and are
> **unchanged** from the original. Everything below reflects the real, verified
> state of the project.

### New feature
- **Added `/aid:help` skill** (`skills/help/SKILL.md`) — a read-only command
  reference and capability overview. Lists every command in a table plus example
  requests, and answers "what can you do?" for new users. Takes no action and
  calls no backend endpoint; the capability list is drawn from the documented
  actions in `skills/chat/SKILL.md`. Skills are auto-discovered, so no manifest
  change was needed to register it.

### Manifest
- **`.claude-plugin/plugin.json`** — bumped `version` `1.0.0` → `1.1.0`.
  (`license: MIT` and `repository` were already present; left unchanged.)

### Open-source scaffolding (new files)
- `LICENSE` — MIT, © Lachlan McG (no license *file* existed previously, though
  the manifest already declared MIT).
- `CONTRIBUTING.md` — layout, how to add a skill, "only call endpoints that
  exist", design principles, local testing.
- `SECURITY.md` — credential handling (this plugin manages AD passwords + a
  tenant API key sent via the `X-API-Key` header), safe-use guidance, the
  two-step confirmation model, and private vulnerability reporting.
- `CODE_OF_CONDUCT.md` — Contributor Covenant 2.1.
- `.gitignore` — secrets first (`.env`, `*.key`, `*.pem`, `secrets.json`), plus
  OS/editor cruft, logs, node, build output.
- `CHANGELOG.md` — Keep a Changelog format; documents 1.0.0 and 1.1.0.
- `.github/ISSUE_TEMPLATE/bug_report.md` (with a "don't paste secrets" warning)
- `.github/ISSUE_TEMPLATE/feature_request.md`
- `.github/ISSUE_TEMPLATE/config.yml` — disables blank issues, routes security
  reports to a private advisory, links the dashboard.
- `.github/PULL_REQUEST_TEMPLATE.md`

### Docs
- **`README.md`** — added license/version/plugin badges, a `/aid:help` row and
  example, and new **Security**, **Contributing**, and **License** sections.

### Verification
- `plugin.json` validated as parseable JSON.
- `skills/tickets/SKILL.md` and `skills/users/SKILL.md` confirmed byte-identical
  to `HEAD` (no unintended changes).

### Notes / decisions
- Did **not** invent any backend endpoints — every skill still maps only to the
  documented AID Helpdesk API (`/api/v1/...`, `X-API-Key` auth).
- A background "spawn task" chip about a stale plugin `.zip` was created earlier
  under the mistaken-project assumption; it is **not applicable** (no such file
  exists here) and can be dismissed.
