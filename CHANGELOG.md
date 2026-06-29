# Changelog

All notable changes to the CrystaLit plugin are documented here. This project
follows [Semantic Versioning](https://semver.org/). The authoritative version is
the `version` field in `.claude-plugin/plugin.json`; bump it on every release so
installed users receive the update via `/plugin marketplace update crystalit`.

## v1.0.0 — 2026-06-24

Initial public release.

- Five-phase literature crystallization pipeline (`crystalit` orchestrator + `crystalit-noter`, `crystalit-ontologist`, `crystalit-labeler`, `crystalit-vizmaker`, `crystalit-writer`).
- Companion skills: `foundation-scout` (Phase 0 library building) and `panel-discussion` (strategic deliberation).
- Removed the 4 paper-writing companion skills (`scope-strategist`, `paper-architect`, `figure-strategist`, `paper-polisher`); they belong in a separate plugin.
- Repo is its own Claude Code marketplace via `.claude-plugin/marketplace.json` (install with `crystalit@crystalit`).
- Domain-agnostic skills; `examples/` ship a cardiac-CT worked demo.
