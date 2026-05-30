---
name: dashboard
description: At-a-glance command center for your Active Directory. Shows domain health in one view — total users, locked accounts, expired passwords, and open tickets — with suggested next actions. Use when the user asks "how's everything looking", "give me an overview", "what needs attention", "status dashboard", or "AD health check".
---

# AID Helpdesk — Command Center Dashboard

The user wants a single at-a-glance overview of their Active Directory health and
support queue. Gather the read-only data below, then present it as a clean status
board. **This is read-only — do not take any action.** Only suggest next steps.

If `AID_API_KEY` or `AID_URL` are not set, tell the user to run `/aid:setup` first
and stop.

## Gather the data

Run these calls (they are all read-only). If any single call fails, show what you
*did* get and note which section was unavailable — don't abandon the whole board.

Domain summary (totals, locked count, expired count):
```bash
curl -s -X POST "$AID_URL/api/v1/actions/get_stats" \
  -H "X-API-Key: $AID_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{}'
```

Currently locked accounts:
```bash
curl -s -X POST "$AID_URL/api/v1/actions/list_locked_accounts" \
  -H "X-API-Key: $AID_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{}'
```

Accounts with expired passwords:
```bash
curl -s -X POST "$AID_URL/api/v1/actions/list_expired_passwords" \
  -H "X-API-Key: $AID_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{}'
```

Open support tickets:
```bash
curl -s -X GET "$AID_URL/api/v1/tickets?status=open" \
  -H "X-API-Key: $AID_API_KEY"
```

## Present the board

Lead with a one-line health verdict, then a compact summary, then the detail
sections. Keep it scannable — this is a dashboard, not a report.

### 1. Health line

Pick a single headline based on what you found:
- 🟢 **All clear** — nothing locked, nothing expired, no open tickets
- 🟡 **A few things to look at** — some locked/expired accounts or open tickets,
  but nothing urgent
- 🔴 **Needs attention** — many locked accounts, urgent tickets, or expired
  passwords piling up

### 2. Summary table

| Metric | Count |
| --- | --- |
| 👥 Total users | from `get_stats` |
| 🔒 Locked accounts | count from `list_locked_accounts` |
| ⏰ Expired passwords | count from `list_expired_passwords` |
| 🎫 Open tickets | count from the tickets call |

Use the `get_stats` totals where available; fall back to the length of the
returned lists if a count field is absent.

### 3. Detail — only show sections that have entries

**🔒 Locked accounts** — list each as `name (username)`. If more than ~10, show
the first 10 and note how many more there are.

**⏰ Expired passwords** — same format.

**🎫 Open tickets** — show the top few by priority (🔴 urgent, 🟠 high, 🟡 medium,
⚪ low) with title and age. If more than ~5, summarise the rest by count.

If a section is empty, collapse it to a single line (e.g. "🔒 Locked accounts —
none ✅") rather than an empty heading.

### 4. Suggested next actions

End with 1–3 concrete, copy-pasteable suggestions drawn from what you found —
never run them, just offer:

- If accounts are locked: ``Unlock them with `/aid:chat unlock <username>`.``
- If passwords are expired: ``Force a reset with `/aid:chat reset <username>'s password`.``
- If tickets are open: ``Dig into one with `/aid:chat analyse ticket #<id>`.``
- If everything is clear: say so plainly — no action needed.

## Notes

- This board only reads documented endpoints; it never makes changes.
- If a call returns `success: false` with `limit_reached`, relay the plan-quota
  upgrade message and show whatever other sections succeeded.
- If a call returns HTTP 504, the Windows agent is likely offline — note it and
  suggest checking the agent service, then show any sections that did return.
