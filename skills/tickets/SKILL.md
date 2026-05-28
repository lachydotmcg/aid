---
description: List and review support tickets from your AID Helpdesk queue. Optionally filter by status.
---

# AID Helpdesk — Tickets

Fetch the ticket queue by running:
```bash
curl -s -X GET "$AID_URL/api/v1/tickets" \
  -H "X-API-Key: $AID_API_KEY"
```

If `$ARGUMENTS` contains a status keyword (open, in_progress, resolved, closed), append `?status=<status>` to the URL.

If `AID_API_KEY` or `AID_URL` are not set, tell the user to run `/aid:setup` first.

Parse the JSON response and display the tickets in a clean summary table with these columns:
- **#** — ticket number/ID (short form)
- **Title** — ticket title
- **Requester** — requester name or email
- **Priority** — low / medium / high / urgent (use appropriate emoji: 🔴 urgent, 🟠 high, 🟡 medium, ⚪ low)
- **Status** — current status
- **Age** — how long ago it was created (e.g. "2h ago", "3 days ago")

If there are no tickets, say so clearly.

After showing the list, offer: "To dig into a specific ticket or take action, use `/aid:chat` — for example: `/aid:chat analyse ticket #42` or `/aid:chat resolve ticket #42`."
