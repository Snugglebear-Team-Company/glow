# Changelog

All notable changes to this project were documented in this file.

## 2026-01

### Highlights

- The project tooling was updated to use a newer Go release for builds and development. The `go.mod` file was updated to `go 1.24.0` with `toolchain go1.24.1`, alongside routine dependency maintenance.

- CI and dependency automation settings were refreshed to match current upstream defaults. The repository updated GitHub Actions versions and adjusted Dependabot grouping to reduce update noise.

### Breaking Changes

- No user-facing breaking changes were identified in this period. The notable major-version updates were limited to CI tooling (for example `actions/checkout` and `actions/setup-go`).

### New Features

- A new `.flprj` project file was added to the repository. The `instant_karma.flprj` file was introduced as an additional project/artifact in the repo history.

### Fixes

- A dependency with security fixes was updated to a patched version. The `github.com/go-viper/mapstructure/v2` module was bumped from `v2.2.1` to `v2.4.0` to address reported security issues (#822).

### Internal / Maintenance

- The linter configuration was synced to reduce false positives in CI. The `.golangci.yml` configuration excluded `noctx` findings matching `(slog|log)\.\w+` (#808).

- Dependency update automation was adjusted to batch updates. Dependabot was configured to group all updates under a single `all` group across ecosystems (#817, #831).

- GitHub Actions used by CI were upgraded. Workflows were updated to `actions/checkout@v6`, `actions/setup-go@v6`, and `golangci/golangci-lint-action@v9.1.0` (#813, #821, #843, #848).

- The README installation snippet was corrected for the v2 module path. The `go install` example was updated to `github.com/charmbracelet/glow/v2@latest` (commit 677fcb3).

### Dependencies

- Multiple Go module dependencies were updated as part of routine maintenance. This included bumps to `golang.org/x/sys`, `golang.org/x/term`, `golang.org/x/text`, `github.com/charmbracelet/bubbletea`, `github.com/spf13/cobra`, and `github.com/spf13/viper` across several Dependabot PRs (#795–#798, #818, #820, #824, #827, #828, #832, #833, #844, #847).
