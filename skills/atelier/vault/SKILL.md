---
name: vault
description: Search, read, and capture notes in the Obsidian vault at $OBSIDIAN_VAULT. Use when the task needs prior context (a project, acronym, past decision, meeting, or person not in the repo), or when something worth keeping was just found: a tool evaluation, troubleshooting pattern, design decision, or project insight.
---

# Obsidian vault

A second brain spanning 2022 to present: project history, meetings, people,
and reusable technical knowledge that is not in training data.

**Read `$OBSIDIAN_VAULT/docs/conventions.md` before writing anything.** It is
the single source of truth for layout, frontmatter, tags, templates, and the
capture workflow, shared with every other surface that writes to the vault.

## Reaching the vault

Obsidian Sync puts the vault on every machine as a plain directory.

| Job | Tool |
|---|---|
| Read, keyword search, write, edit | Filesystem: `rg`, `Read`, `Edit`, `Write` |
| Move or rename a note | hive `forager_move` (repoints wikilinks) |
| Search by meaning, or Google Drive content | hive `forager_semantic_search` |
| Suggest links for a note | hive `forager_related_notes` |
| View a Drive page or picture | hive `forager_get_page` |

hive paths are relative to `$OBSIDIAN_VAULT`. hive needs the home PC awake;
when it reports otherwise, fall back to `rg`. Work profiles have no hive at
all, so the filesystem is the only path there.

If `$OBSIDIAN_VAULT` is empty or unexpanded, use `$HOME/repos/obsidian`.

## Search before creating

Duplicates are the main failure mode.

```bash
rg -il "{topic}" "$OBSIDIAN_VAULT" --glob '*.md'             # filename + content
rg -l "^type: knowledge" "$OBSIDIAN_VAULT/knowledge"        # by frontmatter
rg -o 'tags:.*topic/[a-z-]+' "$OBSIDIAN_VAULT" -r '$0'      # by tag
```

When the words are unknown (a topic, not a term), use
`forager_semantic_search`. Prefer appending to an existing note over creating
a near-duplicate.
