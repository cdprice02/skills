# Atelier

Local-only bucket. Nothing here comes from, or goes back to, upstream
(`mattpocock/skills`): it doesn't exist there and never will, so `git merge
upstream/main` never touches this folder. Not promoted: no top-level
`README.md` entry, no `.claude-plugin/plugin.json` entry, no `docs/atelier/`
page. That's deliberate, not an oversight; see `AGENTS.md` for why promotion
brings merge-conflict machinery this bucket exists to skip.

- **[toolchain](./toolchain/SKILL.md)**: Command lookup table for Rust,
  Python, and Node projects (test, lint, fmt, watch, bench, and friends).
  Model-invoked.
