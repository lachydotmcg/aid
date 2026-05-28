# AID Helpdesk — Claude Code Plugin

Manage your Windows Active Directory from Claude Code. Unlock accounts, reset passwords, look up users, and handle support tickets — all in plain English, without leaving your terminal.

Powered by [AID Helpdesk](https://web-production-01ecc.up.railway.app).

## Prerequisites

- An AID Helpdesk account — [sign up free](https://web-production-01ecc.up.railway.app/signup)
- Your tenant API key (Settings page in your dashboard)
- The AID Windows Agent running on your Windows Server

## Setup

Set two environment variables (add to your shell profile to persist):

```bash
export AID_API_KEY=your_tenant_api_key
export AID_URL=https://web-production-01ecc.up.railway.app
```

Then verify your connection:

```
/aid:setup
```

## Skills

| Skill | Description |
|---|---|
| `/aid:setup` | Verify your API key and test the connection |
| `/aid:chat <message>` | Talk to your AD assistant in plain English |
| `/aid:tickets [status]` | List your support ticket queue |
| `/aid:users [search]` | Look up Active Directory users |

## Examples

```
/aid:chat who's locked out right now?
/aid:chat unlock john.smith
/aid:chat reset sarah.jones's password
/aid:chat add mike.taylor to the IT-Admins group
/aid:tickets open
/aid:users jane
```

## How it works

The `/aid:chat` skill sends your message to the AID Helpdesk AI, which decides what AD action to take, queues it through your Windows Agent, and returns the result. Destructive actions (disable account, bulk changes) require a confirmation code — the AI will prompt you.

Every action is logged to your AID Helpdesk audit trail.

## Self-hosting

If you're running your own AID Helpdesk backend, set `AID_URL` to your instance's URL instead.

## Links

- [AID Helpdesk dashboard](https://web-production-01ecc.up.railway.app)
- [Documentation](https://github.com/lachydotmcg/ad-helpdesk)
- [Report an issue](https://github.com/lachydotmcg/ad-helpdesk/issues)
