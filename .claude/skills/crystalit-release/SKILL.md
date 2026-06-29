---
name: crystalit-release
description: "Use when releasing a new version of the CrystaLit plugin. Triggers on 'release CrystaLit', 'cut a release', 'publish an update', 'bump the version and push', 'ship a new version', or 'release the plugin'. Handles the semver bump in plugin.json, manifest validation, changelog, commit, push, optional tag/GitHub release, and the marketplace-update note for users. Maintainer-only skill — lives in .claude/skills/ and is NOT shipped with the plugin."
version: 1.0.0
---

# CrystaLit Release

Guides a clean, repeatable release of the CrystaLit Claude Code plugin.

- **Repo / remote:** `origin` -> https://github.com/Sdamirsa/crystalit-SKILL (branch `main`)
- **Plugin manifest:** `.claude-plugin/plugin.json`
- **Marketplace manifest:** `.claude-plugin/marketplace.json` (this repo is its own marketplace)
- **Marketplace name:** `crystalit` — users install with `crystalit@crystalit`

## The release model (read this first)

- `version` in **`.claude-plugin/plugin.json` is the single source of truth.** Users only receive an update when this number **increases**. If you ship changes without bumping it, installed users keep the old version silently.
- The marketplace entry in `marketplace.json` intentionally has **NO `version` field** — if set in both, `plugin.json` wins silently. Never add `version` to `marketplace.json`.
- Users pull a release with `/plugin marketplace update crystalit` (or `/plugin install crystalit@crystalit` if not yet installed).

## Release procedure (HITL — confirm the version bump with the user)

1. **Review what changed.** `git log --oneline` since the last release (use the last `v*` tag if one exists: `git log <last-tag>..HEAD --oneline`), plus a `git diff --stat`. Summarize the changes for the user.
2. **Choose the semver bump together:**
   - **patch** (`x.y.Z`): prose/doc fixes, a bug fix inside a skill, no change to the workflow contract.
   - **minor** (`x.Y.0`): a new skill, a new capability or figure type, backward-compatible additions.
   - **major** (`X.0.0`): removed/renamed a skill, changed the output file structure, or any breaking workflow change.
3. **Bump `version` in `.claude-plugin/plugin.json` only.** Do not touch `marketplace.json`'s version.
4. **Update `CHANGELOG.md`** (create it if missing) with a dated, `vX.Y.Z`-headed entry summarizing the changes.
5. **Validate both manifests parse:**
   `python -c "import json; json.load(open('.claude-plugin/plugin.json')); json.load(open('.claude-plugin/marketplace.json')); print('OK')"`
6. **Sanity sweep:** confirm the shipped `skills/` count matches the docs (README/CLAUDE.md), grep for dangling references to any removed skill, and confirm no secrets/keys slipped in.
7. **Commit on `main`:** `Release vX.Y.Z: <one-line summary>` (sign per repo norm; end with the Co-Authored-By trailer).
8. **Push:** `git push origin main`.
9. **(Recommended) Tag + GitHub release:**
   `git tag vX.Y.Z && git push origin vX.Y.Z`
   then `gh release create vX.Y.Z --title "vX.Y.Z" --notes "<changelog excerpt>"`.
10. **Tell the user the end-user update command:** `/plugin marketplace update crystalit`.

## Guardrails

- Push, tag, and GitHub-release are **outward-facing publish actions** — confirm before running, and expect the safety classifier to require explicit user intent for repo-visibility changes or force operations.
- **Never** set `version` in `marketplace.json`.
- Root `skills/` ship to end users; **this release skill must stay in `.claude/skills/`** — never move it into the root `skills/` directory.
- The `examples/` files are an intentional cardiac-CT worked demo, not domain-locked content — don't "generalize" them as part of a routine release unless the release is specifically about the examples.
- If the release removes or renames a shipped skill, also update `README.md`, `CLAUDE.md`, `docs/`, and any cross-references in other skills in the same release.
