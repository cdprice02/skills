# Atelier

Skills I wrote. Upstream (`mattpocock/skills`) has no `atelier/` bucket, so
`git merge upstream/main` never touches this folder. Not promoted: no
top-level `README.md` entry, no `.claude-plugin/plugin.json` entry, no docs
page, which keeps it clear of the merge-conflict machinery `AGENTS.md`
describes for promoted buckets.

- **[commit](./commit/SKILL.md)**: Commit message format, branch choice, and
  staging rules. Model-invoked.
- **[toolchain](./toolchain/SKILL.md)**: Command lookup table for Rust,
  Python, and Node projects. Model-invoked.
- **[vault](./vault/SKILL.md)**: Reaching and searching the Obsidian vault;
  conventions live in the vault's own `docs/conventions.md`. Model-invoked.
