# AID Helpdesk — Claude Code Plugin

Manage your Windows Active Directory from Claude Code. Unlock accounts, reset passwords, look up users, and handle support tickets — all in plain English, without leaving your terminal.

Powered by [AID Helpdesk](https://web-production-01ecc.up.railway.app).

## How it works

You are the AI. The plugin gives Claude Code a reference card of available AD actions and their endpoints. Claude Code reads your plain-English request, picks the right action, calls the endpoint directly, and shows you the result. No separate AI runs on the backend — just a direct queue to your Windows agent.

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
| `/aid:chat <message>` | Perform any AD action in plain English |
| `/aid:tickets [status]` | List your support ticket queue |
| `/aid:users [search]` | Look up Active Directory users |

## Examples

```
/aid:chat who's locked out right now?
/aid:chat unlock john.smith
/aid:chat reset sarah.jones's password
/aid:chat add mike.taylor to the IT-Admins group
/aid:chat show me all disabled accounts in the Sales OU
/aid:tickets open
/aid:users jane
```

## Self-hosting

If you're running your own AID Helpdesk backend, set `AID_URL` to your instance's URL instead.

## Links

- [AID Helpdesk dashboard](https://web-production-01ecc.up.railway.app)
- [Documentation](https://github.com/lachydotmcg/ad-helpdesk)
- [Report an issue](https://github.com/lachydotmcg/ad-helpdesk/issues)
