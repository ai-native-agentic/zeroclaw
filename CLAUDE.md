# ZEROCLAW — RUST AUTONOMOUS AGENT RUNTIME

**Branch:** master

## OVERVIEW

ZeroClaw is a Rust-first autonomous agent runtime optimized for performance, efficiency, stability, extensibility, sustainability, and security. Core architecture is trait-driven and modular — extend by implementing traits and registering in factory modules.

Key extension points: `Provider` (`src/providers/traits.rs`), `Channel` (`src/channels/traits.rs`), `Tool` (`src/tools/traits.rs`), `Memory` (`src/memory/traits.rs`), `Observer` (`src/observability/traits.rs`), `RuntimeAdapter` (`src/runtime/traits.rs`), `Peripheral` (`src/peripherals/traits.rs`).

## STRUCTURE

```
zeroclaw/
├── src/main.rs            # CLI entrypoint and command routing
├── src/lib.rs             # Module exports and shared command enums
├── src/config/            # Schema + config loading/merging
├── src/agent/             # Orchestration loop
├── src/gateway/           # Webhook/gateway server
├── src/security/          # Policy, pairing, secret store
├── src/memory/            # Markdown/SQLite backends + embeddings/vector merge
├── src/providers/         # Model providers and resilient wrapper
├── src/channels/          # Telegram/Discord/Slack/etc channels
├── src/tools/             # Tool execution surface (shell, file, memory, browser)
├── src/peripherals/       # Hardware peripherals (STM32, RPi GPIO)
├── src/runtime/           # Runtime adapters (currently native)
├── docs/                  # Topic-based documentation
└── .github/               # CI, templates, automation workflows
```

## WHERE TO LOOK

| Task | Path | Notes |
|------|------|-------|
| Add provider | `src/providers/traits.rs` | Implement `Provider` trait |
| Add channel | `src/channels/traits.rs` | Implement `Channel` trait |
| Add tool | `src/tools/traits.rs` | Implement `Tool` trait |
| Agent loop | `src/agent/` | Core orchestration |
| Security policy | `src/security/` | High risk — careful review |
| Gateway/webhooks | `src/gateway/` | High risk |
| Hardware | `src/peripherals/` | STM32, RPi GPIO |
| Config | `src/config/` | Schema + loading/merging |
| Change playbooks | `docs/contributing/change-playbooks.md` | Provider/channel/tool recipes |
| PR discipline | `docs/contributing/pr-discipline.md` | Privacy, attribution |

## COMMANDS

```bash
cargo fmt --all -- --check
cargo clippy --all-targets -- -D warnings
cargo test
./dev/ci.sh all                    # Full pre-PR validation
```

Docs-only changes: run markdown lint and link-integrity checks. Bootstrap scripts: `bash -n install.sh`.

## CONVENTIONS

**Rust**: Edition 2021. `cargo fmt --check`, `cargo clippy -- -D warnings`, `cargo test`. Feature-gated deps via Cargo features.

**Workflow**: Read before write. One concern per PR. Minimal patch — no speculative abstractions. Validate by risk tier. Conventional commit titles. Small PRs preferred (`size: XS/S/M`). Work from non-`master` branch; open PR to `master`.

**Risk tiers**: Low (docs/chore/tests-only), Medium (most `src/**` behavior changes), High (`security/`, `runtime/`, `gateway/`, `tools/`, `.github/workflows/`). When uncertain, classify higher.

## ANTI-PATTERNS

| Forbidden | Why |
|-----------|-----|
| Heavy deps for minor convenience | Binary bloat, audit surface |
| Silently weakening security policy | Safety-critical code |
| Speculative config/feature flags | No use case = no code |
| Massive formatting-only mixed with functional changes | Review noise |
| Modifying unrelated modules "while here" | Scope creep |
| Bypassing failing checks without explanation | CI integrity |
| Hiding behavior changes in refactor commits | Reviewability |
| Personal identity/sensitive info in test data | Privacy |
