# Changelog

All notable changes to the AID Helpdesk plugin are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [1.2.0] - 2026-05-31

### Added
- New `/aid:dashboard` skill: a read-only "command center" status board that
  aggregates domain health into one at-a-glance view — total users, locked
  accounts, expired passwords, and open tickets — with a health verdict and
  suggested next actions. Uses only existing documented read endpoints.

### Changed
- README and `/aid:help`: added `/aid:dashboard` to the command table and
  examples.
- Bumped version to 1.2.0 in `plugin.json`.

## [1.1.0] - 2026-05-31

### Added
- New `/aid:help` skill: a read-only command reference and capability overview
  for users who want to see everything the plugin can do at a glance.
- Open-source scaffolding: `LICENSE` file, `CONTRIBUTING.md`, `SECURITY.md`,
  `CODE_OF_CONDUCT.md`, `.gitignore`, `CHANGELOG.md`, and GitHub issue/PR
  templates.

### Changed
- README: added badges, a `/aid:help` entry, and Security, Contributing, and
  License sections.

## [1.0.0]

### Added
- Initial release with the `setup`, `chat`, `users`, and `tickets` skills.
