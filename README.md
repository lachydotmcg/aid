# AID Helpdesk — Claude Code Plugin

[![License: MIT](https://img.shields.io/badge/License-MIT-blue.svg)](LICENSE)
[![Version](https://img.shields.io/badge/version-1.1.0-green.svg)](CHANGELOG.md)
[![Claude Code Plugin](https://img.shields.io/badge/Claude%20Code-plugin-8A2BE2.svg)](https://docs.claude.com/en/docs/claude-code)

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
| `/aid:dashboard` | At-a-glance command center: domain health, locked accounts, expired passwords, open tickets |
| `/aid:chat <message>` | Perform any AD action in plain English |
| `/aid:tickets [status]` | List your support ticket queue |
| `/aid:users [search]` | Look up Active Directory users |
| `/aid:help` | Show all commands and example requests |

## Examples

```
/aid:dashboard
/aid:chat who's locked out right now?
/aid:chat unlock john.smith
/aid:chat reset sarah.jones's password
/aid:chat add mike.taylor to the IT-Admins group
/aid:chat show me all disabled accounts in the Sales OU
/aid:tickets open
/aid:users jane
/aid:help
```

## Self-hosting

If you're running your own AID Helpdesk backend, set `AID_URL` to your instance's URL instead.

## Security

This plugin performs privileged AD actions and handles credentials. Keep your
`AID_API_KEY` in an environment variable (never in a commit), use an HTTPS
`AID_URL`, and review destructive actions before confirming — they require a
two-step confirmation handshake. See [SECURITY.md](SECURITY.md) for the full
policy and how to report a vulnerability.

## Contributing

Contributions are welcome — see [CONTRIBUTING.md](CONTRIBUTING.md) for the
project layout, how to add a skill, and design principles. Please also review the
[Code of Conduct](CODE_OF_CONDUCT.md) and the [changelog](CHANGELOG.md).

## License

[MIT](LICENSE) © Lachlan McG

## Links

- [AID Helpdesk dashboard](https://web-production-01ecc.up.railway.app)
- [Documentation](https://github.com/lachydotmcg/ad-helpdesk)
- [Report an issue](https://github.com/lachydotmcg/ad-helpdesk/issues)
