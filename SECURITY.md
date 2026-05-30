# Security Policy

AID Helpdesk performs privileged Active Directory operations — unlocking
accounts, resetting passwords, enabling/disabling accounts, and changing group
membership. Security is a first-class concern for this plugin.

## How the plugin protects you

- **Confirmation before destructive actions.** Skills instruct Claude to confirm
  the target before any reset, disable, or group removal.
- **No password echo.** Temporary passwords are surfaced once during a reset and
  are never logged or repeated afterward.
- **API-key auth.** All backend calls require your tenant API key in an
  `X-API-Key` header; the key lives in the `AID_API_KEY` environment variable,
  not in the repository.
- **Thin and auditable.** The plugin adds no hidden prompting — every action maps
  to a documented backend endpoint you can review.

## Using the plugin safely

- **Keep your API key out of version control.** Set `AID_API_KEY` and `AID_URL`
  as environment variables. Never paste your key into a file, issue, or commit.
  The repository `.gitignore` excludes common secret files as a backstop.
- **Use HTTPS for `AID_URL`.** Credentials and account data should only travel
  over TLS.
- **Scope your tenant key** to the minimum privileges the helpdesk needs.
- **Rotate the key** if you suspect it was exposed.
- **Review before confirming** any destructive action Claude proposes.

## Reporting a vulnerability

Please **do not** open a public issue for security problems. Instead, report
privately:

- Open a [GitHub Security Advisory](https://github.com/lachydotmcg/ad-helpdesk/security/advisories/new)
  (preferred), or
- Contact the maintainer via the email on their GitHub profile.

Include steps to reproduce, the impact, and any suggested remediation. You can
expect an initial acknowledgement within a few days.

## Scope

This policy covers the plugin in this repository. Issues in the AID Helpdesk
backend, the Windows agent, Claude Code itself, or the underlying models should
be reported to their respective maintainers.
