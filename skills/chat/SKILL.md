---
description: Talk to your AID Helpdesk AI to manage Active Directory in plain English. Unlock accounts, reset passwords, look up users, manage groups — all without leaving Claude Code.
---

# AID Helpdesk — Chat

The user wants to perform an Active Directory action or ask a question about their environment.
Their request is: "$ARGUMENTS"

Send their message to the AID Helpdesk AI by running:
```bash
curl -s -X POST "$AID_URL/api/v1/chat" \
  -H "X-API-Key: $AID_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{\"message\": $(echo "$ARGUMENTS" | jq -Rs .)}"
```

If `AID_API_KEY` or `AID_URL` are not set, tell the user to run `/aid:setup` first.

Parse the JSON response:
- If `success` is true, display the `reply` field as-is. It may include action results, user details, or a conversational response.
- If `success` is false, show the `message` field and suggest the user check their connection with `/aid:setup`.
- If the response mentions a pending action that requires confirmation (destructive operations), relay that to the user clearly.

Keep your own commentary minimal — the AID Helpdesk AI's reply is the primary output.
