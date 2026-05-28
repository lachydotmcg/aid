---
description: Search for Active Directory users from your AID Helpdesk. Look up account status, group memberships, and more.
---

# AID Helpdesk — Users

The user wants to look up one or more AD users. Their search query is: "$ARGUMENTS"

Search for users by running:
```bash
curl -s -G "$AID_URL/api/v1/users" \
  -H "X-API-Key: $AID_API_KEY" \
  --data-urlencode "q=$ARGUMENTS"
```

If no arguments were provided, fetch all users (omit the `q` parameter).

If `AID_API_KEY` or `AID_URL` are not set, tell the user to run `/aid:setup` first.

Parse the JSON response and display each user with:
- **Name** — display name
- **Username** — sAMAccountName
- **Email** — email address
- **Status** — 🔒 Locked / ✅ Active / ❌ Disabled / ⏰ Password expired (combine flags if multiple apply)
- **OU** — organisational unit path (shortened if long)
- **Last logon** — if available

If a single user matches, show their full details including group memberships.

After the results, offer relevant quick actions: "To unlock, reset a password, or take any other action, use `/aid:chat` — e.g. `/aid:chat unlock john.smith`."
