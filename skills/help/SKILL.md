---
name: help
description: Show what AID Helpdesk can do — a quick overview of every command and example requests. Use when the user asks "what can you do", "how do I use this", "help", or is new to the plugin and wants to see available Active Directory actions.

---

# AID Helpdesk — Help & Command Reference

The user wants a quick overview of what this plugin can do. Present it clearly and concisely. **Do not call any API or take any action** — this is a read-only reference.

## How to respond

Show the commands as a scannable table, then a short list of example requests. Keep it tight — this is a menu, not a manual.

### Commands

| Command | What it does |
| --- | --- |
| `/aid:setup` | Verify your API key and test the connection to the backend. Run this first. |
| `/aid:chat <request>` | Do any Active Directory action in plain English (the main command). |
| `/aid:users [search]` | Look up AD users by name or username and see their status. |
| `/aid:tickets [status]` | List your support ticket queue (`open`, `closed`, or `all`). |
| `/aid:help` | Show this overview. |

### What `/aid:chat` can do

Active Directory actions available through plain language:

- **Look up** users, groups, and OUs — `who's locked out right now?`, `show users in the Sales OU`, `domain stats`
- **Unlock** a locked-out account — `unlock john.smith`
- **Reset a password** (generates a strong temporary one) — `reset sarah.jones's password`
- **Enable / disable** an account — `disable the account for mike.taylor`
- **Add / remove group membership** — `add jane.doe to IT-Admins`
- **Create or move** users and OUs — `create a user for Jane Doe`, `move john.smith to the Finance OU`

Destructive actions (disable, remove from group, reset, create, move) require a
two-step confirmation before they run.

### Example requests

```
/aid:chat who's locked out right now?
/aid:chat unlock john.smith
/aid:chat reset sarah.jones's password
/aid:chat add mike.taylor to the IT-Admins group
/aid:users jane
/aid:tickets open
```

## Notes

- If the user hasn't connected yet (missing `AID_API_KEY` or `AID_URL`), point them to `/aid:setup` first.
- Destructive actions (reset, disable, remove from a group) always ask for confirmation before running.
- Tailor the depth to the question: a broad "what can you do?" gets the full table; a specific "how do I reset a password?" gets just that flow.
