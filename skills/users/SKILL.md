---
description: Search for Active Directory users from your AID Helpdesk. Look up account status, group memberships, and more.
---

# AID Helpdesk — Users

The user wants to look up one or more AD users. Their search query is: "$ARGUMENTS"

If `AID_API_KEY` or `AID_URL` are not set, tell the user to run `/aid:setup` first.

## Decision

- **No arguments** → list all users (`list_users`)
- **Exact username** (no spaces, looks like a sAMAccountName) → full details (`get_user_info`)
- **Partial name or search term** → search (`search_users`)

## Calls

List all users:
```bash
curl -s -X POST "$AID_URL/api/v1/actions/list_users" \
  -H "X-API-Key: $AID_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{}'
```

Get full details for a specific user:
```bash
curl -s -X POST "$AID_URL/api/v1/actions/get_user_info" \
  -H "X-API-Key: $AID_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{\"username\": \"$ARGUMENTS\"}"
```

Search by name or partial username:
```bash
curl -s -X POST "$AID_URL/api/v1/actions/search_users" \
  -H "X-API-Key: $AID_API_KEY" \
  -H "Content-Type: application/json" \
  -d "{\"query\": \"$ARGUMENTS\"}"
```

## Display

For each user show:
- **Name** — display name
- **Username** — sAMAccountName
- **Email** — email address
- **Status** — 🔒 Locked / ✅ Active / ❌ Disabled / ⏰ Password expired (combine flags if multiple apply)
- **OU** — organisational unit path (shorten if long)
- **Last logon** — if available

If a single user was returned, also show their full group memberships.

After the results, offer: "To take action on a user, use `/aid:chat` — e.g. `/aid:chat unlock john.smith` or `/aid:chat reset john.smith's password`."
