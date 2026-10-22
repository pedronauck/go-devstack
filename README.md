# Go Devstack

A production-ready Go project template with agent-ready tooling, inspired by the
internal `compozy/agh` stack.

- Pinned **Go 1.26.2** via `mise.toml` + `.go-version`
- Zero-tolerance **golangci-lint v2** with 21 linters + gofmt/goimports/golines
- `gopls` modernize analyzer integrated into `make lint`
- Race-enabled tests via `gotestsum`, coverage via `make cover`
- TOML config + `.env` secrets pattern, structured slog logging, graceful shutdown
- **Husky + lint-staged + commitlint + oxfmt + oxlint** for non-Go files and commit hygiene
- **GoReleaser** community release pipeline (multi-OS/arch archives, checksums, source, snapshot)
- **GitHub Actions** CI (detect-changes + verify) and Release (tag-driven goreleaser)
- **Distroless multi-stage Dockerfile** with version ldflags injection
- 13 curated AI agent skills for Go development

## Prerequisites

- [mise](https://mise.jdx.dev/) (recommended) or Go 1.26.2 + Bun 1.4.4
- `git`
- `make`

## Quick Start

```bash
# 1. Clone or copy this template
# 2. Rename the module to your own
go mod edit -module github.com/yourorg/yourproject
# update imports in cmd/app/main.go, magefile.go, .goreleaser.yml, Dockerfile, cliff.toml

# 3. Install pinned toolchain (Go, Bun, gotestsum, golangci-lint, goreleaser)
mise trust && mise install

# 4. Install JS deps + git hooks
bun install
make hooks-install

# 5. Verify everything is wired up
make verify
```

## Commands

### Go pipeline

```bash
make deps             # go mod tidy
make fmt              # gofmt every .go file
make lint             # golangci-lint v2 + gopls modernize (auto-fix)
make modernize        # gopls modernize idioms only
make test             # gotestsum + -race -parallel=4
make test-integration # tests with `-tags integration`
make cover            # coverage.out + coverage.html
make build            # bin/app with version ldflags
make verify           # fmt -> lint -> test -> build (BLOCKING gate)
make tools            # install gotestsum, golangci-lint, modernize, goreleaser
```

### JS/TS toolchain

```bash
make bun-lint        # oxfmt + oxlint over non-Go files
make bun-fmt         # apply oxfmt formatting
make bun-fmt-check   # check oxfmt without writing
```

### Release & containers

```bash
make release-snapshot # local goreleaser snapshot under dist/
make docker-build     # docker build -t go-devstack:dev .
```

### Hooks

```bash
make hooks-install    # husky install (requires bun install first)
```

## Project Layout

```
cmd/app/           Entry point and CLI wiring
internal/config/   TOML config + .env secrets
internal/version/  Build metadata injection (Version, Commit, BuildDate)
internal/logger/   Structured slog logging
```

## Configuration

Copy `.env.example` to `.env` and fill in secret values.
Copy `config.example.toml` to `config.toml` and customize settings.

```bash
go run ./cmd/app run                    # default config
go run ./cmd/app run -config ./config.toml
go run ./cmd/app version                # prints "<version> (commit=<sha> date=<rfc3339>)"
```

## Conventional Commits

Commit-msg hook enforces Conventional Commits. Allowed types:

```
build, chore, ci, docs, feat, fix, perf, refactor, test
```

Scope is optional. Subject case is unconstrained. Body line length is unbounded.

## Release Flow

Releases are tag-driven. Push a `v*` tag and the release workflow runs goreleaser:

```bash
git tag v0.1.0
git push origin v0.1.0
```

The pipeline produces:

- linux/darwin/windows × amd64/arm64 binaries (no windows/arm64)
- tar.gz / zip archives
- `checksums.txt`
- Source tarball
- Auto-generated changelog grouped by Conventional Commit type

Optional channels (Homebrew tap, deb/rpm, cosign signing, SBOMs) are scaffolded
as commented blocks in `.goreleaser.yml`.

## Adding Your Code

1. Add new packages under `internal/` for your domain logic
2. Update the config struct in `internal/config/config.go`
3. Wire services in `cmd/app/main.go` using the `launch()` pattern
4. Update the Package Layout table in `CLAUDE.md`
5. Run `make verify`

## Agent Tooling

This template includes comprehensive AI agent instructions:

- `CLAUDE.md` / `AGENTS.md` — agent behavior guidelines, coding style, skill dispatch protocol
- `.agents/skills/` — 13 curated skills for Go development
- `.claude/skills/` — symlinks for Claude Code compatibility
- `tasks/docs/` — PRD, techspec, and ADR templates

## License

[Choose your license]
