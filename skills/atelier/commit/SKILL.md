---
name: commit
description: Commit message format, branch choice, and staging rules. Use before every git commit, when naming or pushing a branch, or when closing an issue by a merged change.
---

# Commit

## When to commit

Commit when the user asks, or when a task they told you to finish reaches a
natural commit point (a logical change done, checks green). Otherwise name the
commit point and the message you would use, and keep working.

## Branch

- A repo whose changes land through PRs: never commit on its default branch.
  Branch first as `type/short-slug` (`fix/dangling-skill-links`).
- Personal config repos (`~/.nix-atelier/config/claude`, `config/skills`) and
  a repo with no remote: commit on the current branch.
- A worktree branch with a generated name (`worktree-agent-ad38e685`): rename
  it to `type/short-slug` before pushing.
- Hold every push until open design questions are answered; commit mechanical
  fixes locally meanwhile, then push them together.

## Staging

Stage by path (`git add <paths>`), then read `git diff --cached --stat`. One
logical change per commit; split unrelated edits into separate commits.

## Message

```text
type(scope): imperative summary, lowercase, no period, under 72 chars

Optional body: why, or the non-obvious constraint. At most 3 lines,
wrapped at 72.
```

- Types: `feat`, `fix`, `refactor`, `docs`, `test`, `chore`, `ci`, `perf`.
  Scope is the module or area as the repo already names it; check
  `git log --oneline -20` for the house scopes.
- The subject carries the change; most commits need no body. The diff already
  shows what changed, so the body only explains why.
- Write the body as plain prose: headings, bullet lists, and tables belong in
  the PR body (the `pr` skill).
- Name things by what they are: module, behaviour, constraint. Internal plan
  codes, audit waves, and session artifacts stay out of history.
- End with the attribution trailer the harness supplies.

Examples:

```text
fix(link-skills): prune dangling skill links
```

```text
chore: drop inline issue refs from comments and docs

Refs like (#122) surface in generated option docs, where consumers
have no context for them. CHANGELOG.md keeps its refs.
```

## Closing issues

Reference the merged PR (`Closed by #135.`), found with
`gh pr list --state merged`, rather than a commit SHA.
