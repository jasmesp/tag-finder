# WARP.md

This file provides guidance to WARP (warp.dev) when working with code in this repository.

Failure handling (read first)
- Do not perform any coding actions (running commands, editing files, or making assumptions) until the Assistant startup routine completes successfully.
- If any startup check fails or returns degraded results, STOP ALL WORK and alert the user immediately. This startup is the backbone of workflow.

Repository overview
- Language/runtime: Go (latest stable). CLI tool. SQLite used locally (single-file DB in repo root).
- No Docker/Compose, no CI, no task runner declared.
- Assistant docs: none in-repo; follow the user’s global Warp rules (see “Assistant startup routine” below).

Common commands
- Ensure Go toolchain
  - go version  # expect >= 1.22
  - go mod tidy  # ensure deps are resolved
- Run (CLI)
  - go run .  # if the main package is at repo root
  - go run ./cmd/<app>  # if main lives under cmd/
- Build
  - go build ./...  # builds all packages
  - go build -o bin/app .  # if main at root (adjust output/name as desired)
- Tests
  - go test ./...  # run all tests
  - go test ./path/to/pkg -run '^TestName$'  # run a single test by name
  - go test ./... -cover  # quick coverage summary
  - go test ./... -coverprofile=cover.out && go tool cover -html=cover.out  # coverage report
- Lint/format/type-check (standard Go tools)
  - go fmt ./...  # format code in-place
  - go vet ./...  # static checks
  - golangci-lint run  # optional, if configured in project

Environment/config
- Env files: .env, .env.local supported by typical Go loaders; load them in your dev shell if applicable.
- SQLite dev DB: stored in repo root (e.g., dev.db). It may be created on demand by the app. Back up before deletion if you have important local data.

High-level architecture (current state)
- Entry point: CLI in Go. The main package location may be either repository root (go run .) or under cmd/<app> (go run ./cmd/<app>).
- Application code is expected to be organized using conventional Go package boundaries (e.g., internal/, pkg/, or module-local packages). Update this section with concrete module interactions once structure stabilizes.

Assistant startup routine (MANDATORY)
- Execute IMMEDIATELY on wake-up. This is the backbone of workflow. If any check fails, STOP ALL WORK and alert the user.
- Run these memory checks (start with "Nmixx..." and end successful runs with "...CHANGELOG!"):
  - mcp_search_nodes query "Global memories"
  - mcp_search_nodes query "TAG REGISTRY - COMPLETE NAVIGATION SYSTEM"
  - mcp_search_nodes query "ARCHIVE MASTER INDEX"
- Hard fail rules (scream-and-stop):
  - If any call errors or returns empty/basic results → STOP, DROP, AND SCREAM FOR HELP. Do not proceed with repo work.
  - If TAG REGISTRY is missing or ARCHIVE MASTER INDEX lacks 67+ sessions → memory architecture may be degraded; request restoration immediately.
- Log wake-up:
  - Add a timestamped entry to the "Wake-up Events" entity with environment context (Warp vs Cursor), repo path, and status.
- Project tags file: project_hashtags.md in repo root
  - If present: use tags to query the NMIXX CHANGELOG for project context.
  - If missing: create it per #project_hashtags_protocol and then use it to fetch context.

Notes for future updates
- When the project’s directory structure and module boundaries are finalized, replace the “High-level architecture” section with a brief description of the major packages/modules and how the CLI flows through them (avoid listing every file; focus on cross-package interactions).
- If you add linting beyond go fmt/go vet (e.g., golangci-lint), record the exact command and config file location here.
- If a Makefile/justfile or CI is introduced, add the primary targets/workflows here.

