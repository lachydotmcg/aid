# Contributing to AID Helpdesk

Thanks for your interest in improving the AID Helpdesk plugin! Contributions that
keep it focused, safe, and easy to reason about are very welcome.

## What this is

A Claude Code plugin that lets administrators manage Windows Active Directory in
plain English. Claude Code reads the user's request, picks the right action from
a skill's reference card, calls the AID Helpdesk backend endpoint directly, and
reports the result. There is no separate AI on the backend.

## Repository layout

```
.
├── .claude-plugin/
│   └── plugin.json          # Plugin manifest (name, version, metadata)
├── skills/
│   ├── setup/SKILL.md       # /aid:setup — verify key & connection
│   ├── chat/SKILL.md        # /aid:chat  — any AD action in plain English
│   ├── users/SKILL.md       # /aid:users — user lookup
│   ├── tickets/SKILL.md     # /aid:tickets — support queue
│   └── help/SKILL.md        # /aid:help  — command reference
├── CHANGELOG.md
├── LICENSE
└── README.md
```

## Adding or changing a skill

1. Each skill is a folder under `skills/` containing a `SKILL.md` with YAML
   frontmatter (`name`, `description`) followed by the instruction body. Skills
   are auto-discovered — no manifest registration is needed.
2. Write a precise `description`: it is what Claude uses to decide when to invoke
   the skill. State the triggers explicitly.
3. **Only reference backend endpoints that actually exist.** The current API
   surface is documented in `skills/chat/SKILL.md`. Do not invent endpoints.
4. Document the new command in `README.md` and in `skills/help/SKILL.md`.
5. Add a `CHANGELOG.md` entry and bump `version` in `plugin.json`.

### Design principles

- **Stay thin.** The intelligence is Claude plus the backend. Skills are clear,
  action-first reference cards — not layers of clever prompting.
- **Confirm before destructive actions.** Resets, disables, and group removals
  must ask for confirmation first.
- **Never leak secrets.** Don't echo passwords or API keys back beyond a
  one-time setup response.

## Testing locally

Load the repository as a local plugin in Claude Code and exercise the commands:

```
/aid:setup
/aid:help
/aid:users jane
/aid:tickets open
```

## Validating the manifest

`plugin.json` must be valid JSON:

```
python -m json.tool .claude-plugin/plugin.json
```

## Submitting changes

- Keep pull requests focused and small.
- Update docs and the changelog alongside code.
- Never commit real API keys, passwords, or tenant data.

By contributing, you agree that your contributions will be licensed under the
project's [MIT License](LICENSE).
