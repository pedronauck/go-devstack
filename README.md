# Go Devstack

A production-ready Go project template with agent-ready tooling.

- Zero-tolerance lint pipeline (golangci-lint)
- `make verify` serial gate (fmt -> lint -> test -> build)
- TOML config + `.env` secrets pattern
- Structured logging (`log/slog` with JSON output)
- Signal handling and graceful shutdown
- 13 curated AI agent skills for Go development

## Quick Start

```bash
# 1. Clone or copy this template
# 2. Initialize your own module
go mod init github.com/yourorg/yourproject

# 3. Update import paths in all Go files
#    Replace: github.com/user/go-devstack
#    With:    github.com/yourorg/yourproject

# 4. Install dependencies
go mod tidy

# 5. Verify everything works
make verify
```

## Commands

```bash
make deps      # Tidy and verify modules
make fmt       # Format all Go files
make lint      # Strict golangci-lint (zero issues)
make test      # Run tests with -race
make build     # Compile binary
make verify    # Serial: fmt -> lint -> test -> build
```

## Project Layout

```
cmd/app/           Entry point and CLI wiring
internal/config/   TOML config + .env secrets
internal/version/  Build metadata injection
internal/logger/   Structured slog logging
```

## Configuration

Copy `.env.example` to `.env` and fill in secret values.
Copy `config.example.toml` to `config.toml` and customize settings.

```bash
# Run with default config
go run ./cmd/app run

# Run with custom config
go run ./cmd/app run -config ./config.toml

# Print version
go run ./cmd/app version
```

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
