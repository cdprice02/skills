---
name: toolchain
description: Command lookup table for Rust, Python, and Node projects. Use when running a project's build, test, lint, watch, or benchmark commands, or when another skill needs to know the right tool for this stack.
---

# Toolchain

Command lookup, keyed off the marker file in the project root. Only the
non-obvious, house-specific substitutions are recorded here; a one-file
lookup the environment already answers (a `package.json` scripts block,
`--help` output) isn't repeated.

**A repo's own `docs/agents/toolchain.md` overrides everything below.** Check
for it first; if it exists and covers the job at hand, use it instead of this
table.

**`cargo nextest` structurally cannot run doctests.** Not a missing config
option: doctests aren't exposed as test binaries on stable Rust, so cargo
treats them as a special case (nextest's own tracked limitation, upstream
issue #16). The two-command answer (`cargo nextest run`, then
`cargo test --doc`) costs nothing extra: per the nextest maintainers it
triggers no more builds than a plain `cargo test` would. Don't go looking for
a single-command fix; there isn't a stable one.

## Rust (`Cargo.toml`)

| Job                    | Command                                                         |
| ---------------------- | ---------------------------------------------------------------- |
| test                   | `cargo nextest run`, then `cargo test --doc`                    |
| test one               | `cargo nextest run -E 'test(NAME)'`                              |
| watch                  | `bacon clippy`                                                   |
| watch, full pipeline   | `watchexec -c -e rs "cargo clippy && cargo test && cargo run"`   |
| typecheck              | `cargo check --all-targets`                                     |
| lint                   | `cargo clippy --all-targets`                                    |
| fmt                    | `cargo fmt`                                                      |
| pre-push (CI parity)   | `cargo fmt --check`, `cargo clippy --all-targets -- -D warnings`, test, `RUSTDOCFLAGS='-D warnings' cargo doc --no-deps` |
| bench                  | `cargo bench` (criterion backend)                                |
| find a crate           | `cargo search NAME`, then `cargo info NAME` before adding        |
| add a crate            | `cargo add NAME --features ...`                                  |
| unused deps            | `cargo shear`                                                    |
| scaffold               | `cargo new NAME`, or `cargo generate TEMPLATE`                   |
| profile                | `samply record -- ./target/release/BIN`                          |

### Strict Clippy (new crates only)

Opt-in for a crate created from scratch. An established codebase adopts it
deliberately and separately: switching it on at once buries real work under
unrelated lint errors.

`Cargo.toml`:

```toml
[lints.clippy]
pedantic = { level = "deny", priority = -1 }
nursery = { level = "deny", priority = -1 }
unwrap_used = "deny"
expect_used = "deny"
indexing_slicing = "deny"
arithmetic_side_effects = "deny"
unreachable = "deny"
unimplemented = "deny"
unchecked_time_subtraction = "deny"
todo = "deny"
string_slice = "deny"
panic_in_result_fn = "deny"
panic = "deny"
exit = "deny"
as_conversions = "deny"
```

`clippy.toml`:

```toml
allow-unwrap-in-tests = true
allow-expect-in-tests = true
allow-panic-in-tests = true
allow-indexing-slicing-in-tests = true
```

## Python (`pyproject.toml`)

| Job       | Command               |
| --------- | ---------------------- |
| test      | `uv run pytest`        |
| test one  | `uv run pytest -k NAME`|
| typecheck | `mypy`                 |
| lint      | `ruff check`            |
| fmt       | `ruff format`           |
| pre-push  | `ruff format --check`, `ruff check`, `mypy`, test |

## Node (`package.json`)

The `scripts` block in `package.json` is the source of truth here and needs
no caching: read it and use it directly.

Package manager: the lockfile in the repo decides (`bun.lock` or `bun.lockb`
means bun, `pnpm-lock.yaml` means pnpm); never switch a repo's manager. With
no lockfile, default to bun. Use pnpm where the ecosystem standardises on it
(Cloudflare tooling, the portfolio site). Never npm, except for tools that
only document it. One-off runners follow the same split: `bunx`, `pnpm dlx`.
Global tools install with bun.
