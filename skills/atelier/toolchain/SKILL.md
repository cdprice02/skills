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
| bench                  | `cargo bench` (criterion backend)                                |
| find a crate           | `cargo search NAME`, then `cargo info NAME` before adding        |
| add a crate            | `cargo add NAME --features ...`                                  |
| unused deps            | `cargo shear`                                                    |
| scaffold               | `cargo new NAME`, or `cargo generate TEMPLATE`                   |
| profile                | `samply record -- ./target/release/BIN`                          |

## Python (`pyproject.toml`)

| Job       | Command               |
| --------- | ---------------------- |
| test      | `uv run pytest`        |
| test one  | `uv run pytest -k NAME`|
| typecheck | `mypy`                 |
| lint      | `ruff check`            |
| fmt       | `ruff format`           |

## Node (`package.json`)

The `scripts` block in `package.json` is the source of truth here and needs
no caching: read it and use it directly.
