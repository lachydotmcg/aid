---
description: Manage Active Directory in plain English. Unlock accounts, reset passwords, look up users, manage groups — all without leaving Claude Code. You are the AI — just pick the right endpoint and call it.
---

# AID Helpdesk — Chat

The user wants to perform an Active Directory action or ask about their environment.  
Their request is: "$ARGUMENTS"

You are the intelligence layer. Based on the request, pick the right action from the table below, build the `curl` call, run it, and display the result. No AI runs on the backend — it's a direct queue to the Windows agent.

If `AID_API_KEY` or `AID_URL` are not set, tell the user to run `/aid:setup` first.

---

## Endpoint

```
POST $AID_URL/api/v1/actions/<action>
X-API-Key: $AID_API_KEY
Content-Type: application/json
Body: { "param": "value", ... }
```

---

## Action reference

### Read — user queries
| Action | Required params | Optional params | Description |
|--------|----------------|-----------------|-------------|
| `get_user_info` | `username` | | Full details: status, groups, OU, last logon |
| `search_users` | `query` | | Search by name or username (partial match) |
| `list_users` | | | All domain users |
| `list_locked_accounts` | | | All currently locked accounts |
| `list_expired_passwords` | | | Accounts with expired passwords |
| `list_group_memberships` | `username` | | All groups a user belongs to |
| `list_users_in_ou` | `ou` | | All users in a specific OU |
| `get_stats` | | | Domain summary (totals, locked count, expired count) |

### Read — group & OU queries
| Action | Required params | Optional params | Description |
|--------|----------------|-----------------|-------------|
| `list_groups` | | | All AD groups |
| `search_groups` | `query` | | Search groups by name |
| `get_group_members` | `group` | | Members of a group |
| `list_ous` | | | All Organisational Units |

### Write — account actions (reversible)
| Action | Required params | Optional params | Description |
|--------|----------------|-----------------|-------------|
| `unlock_account` | `username` | | Unlock a locked-out account |
| `enable_account` | `username` | | Re-enable a disabled account |
| `reset_password` | `username`, `password` | | Reset password; user must change at logon |
| `force_password_change` | `username` | | Force password change at next logon |
| `set_password_never_expires` | `username` | `enabled` (true/false) | Toggle password expiry |
| `add_to_group` | `username`, `group` | | Add user to a group |

### Destructive — confirm before calling
| Action | Required params | Optional params | Description |
|--------|----------------|-----------------|-------------|
| `disable_account` | `username` | | Disable an account |
| `remove_from_group` | `username`, `group` | | Remove user from a group |
| `create_user` | `first_name`, `last_name`, `username` | `ou` | Create a new AD account |
| `move_user` | `username`, `ou` | | Move user to a different OU |
| `create_ou` | `name` | `parent_ou` | Create a new Organisational Unit |

---

## Examples

Unlock an account:
```bash
curl -s -X POST "$AID_URL/api/v1/actions/unlock_account" \
  -H "X-API-Key: $AID_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"username": "john.smith"}'
```

Reset a password (generate a secure one if the user didn't provide one):
```bash
curl -s -X POST "$AID_URL/api/v1/actions/reset_password" \
  -H "X-API-Key: $AID_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"username": "john.smith", "password": "Temp@38271!"}'
```

Look up a user:
```bash
curl -s -X POST "$AID_URL/api/v1/actions/get_user_info" \
  -H "X-API-Key: $AID_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"username": "john.smith"}'
```

Who's locked out right now:
```bash
curl -s -X POST "$AID_URL/api/v1/actions/list_locked_accounts" \
  -H "X-API-Key: $AID_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{}'
```

---

## Destructive actions — two-step confirmation

Destructive actions (`disable_account`, `remove_from_group`, `move_user`, `create_user`, `reset_password`, `set_password_never_expires`) require a confirmation handshake:

1. Send the action normally. The backend replies with HTTP 409 and `confirmation_required: true` plus a `confirm_token`.
2. **Show the user exactly what will happen and ask them to confirm.** Only if they say yes, re-send the *same* request with the `confirm_token` field added.

Example — first call returns:
```json
{ "success": false, "confirmation_required": true, "confirm_token": "482016",
  "message": "'disable_account' is a destructive action. Re-send ... with confirm_token ..." }
```
Then confirm with the user, and re-send:
```bash
curl -s -X POST "$AID_URL/api/v1/actions/disable_account" \
  -H "X-API-Key: $AID_API_KEY" -H "Content-Type: application/json" \
  -d '{"username": "bob.smith", "confirm_token": "482016"}'
```
The token is single-use and expires in 5 minutes. Never echo the token back without the user actually confirming.

## Displaying results

Parse the JSON response:
- `success: true` — show `message` and format `data` clearly
- `success: false` with `confirmation_required` — this is a destructive action; confirm with the user (see above)
- `success: false` with `limit_reached` — the tenant hit their monthly plan quota; relay the upgrade message
- `success: false` (other) — show `message`; if it mentions the agent, suggest checking the Windows agent is running
- HTTP 504 — agent offline; tell the user to check the Windows agent service

For user data, display:
- **Name** — display name
- **Username** — sAMAccountName  
- **Status** — 🔒 Locked / ✅ Active / ❌ Disabled / ⏰ Password expired (combine flags if multiple)
- **Groups** — comma-separated or bullet list
- **OU** — organisational unit path
- **Last logon** — if available

For destructive actions, confirm with the user before calling. State clearly what will happen.

If the request is ambiguous (e.g. "unlock john" matches multiple users), use `search_users` first, then confirm which account to act on.
