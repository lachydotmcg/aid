# Change Log — AID Helpdesk Plugin

A running log of changes made to this repository by the automated agent. Newest entries first.

---

## 2026-05-31 — `/aid:dashboard` command-center skill (v1.2.0)

**Focus:** add one genuinely high-value, read-only feature that realises the
"command center" idea — a single at-a-glance AD health board. No changes to any
existing skill's behaviour or backend contract; no new endpoints invented.

### New feature
- **Added `/aid:dashboard` skill** (`skills/dashboard/SKILL.md`) — a read-only
  status board that aggregates four already-documented read endpoints into one
  view:
  - `get_stats` (domain totals)
  - `list_locked_accounts`
  - `list_expired_passwords`
  - `GET /api/v1/tickets?status=open`

  It opens with a 🟢/🟡/🔴 health verdict, shows a compact summary table, then
  expands only the non-empty detail sections (locked accounts, expired
  passwords, open tickets by priority), and ends with copy-pasteable suggested
  next actions that point back to `/aid:chat`. It is strictly read-only and
  degrades gracefully if one call fails (shows what it got, notes what was
  unavailable; handles `limit_reached` and HTTP 504 / agent-offline).

### Wiring
- **`skills/help/SKILL.md`** — added `/aid:dashboard` to the command table and
  the example list.
- **`README.md`** — added a `/aid:dashboard` row to the Skills table and an
  example line.
- **`.claude-plugin/plugin.json`** — bumped `version` `1.1.0` → `1.2.0`.
- **`CHANGELOG.md`** — new `[1.2.0]` entry.

### Verification
- `plugin.json` validated as parseable JSON.
- `skills/` now lists: chat, dashboard, help, setup, tickets, users.
- Confirmed the new skill references **only** endpoints already documented in
  `skills/chat/SKILL.md` and `skills/tickets/SKILL.md` — nothing invented.

### Notes / decisions
- The empty, untracked `plugins/ai-command-center/` scaffold (a leftover from an
  earlier project mix-up — git can't track empty dirs, so it was never committed)
  was *not* removed: the attempted cleanup was declined by the permission
  classifier, and it's harmless. Left in place; safe for a human to delete.
- Skills are auto-discovered, so no manifest registration was needed for the new
  skill.

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
