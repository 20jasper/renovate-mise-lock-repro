# renovate-mise-lock-repro

Renovate bug: several mise config files share one `mise.lock`, and a grouped branch containing a lockfile-only update loses the lock refreshes of all but the last config file.

## Setup

- `.config/mise/config.toml`: `actionlint` pinned exactly to `1.7.11` (`1.7.12` exists).
- `.config/mise/conf.d/a.toml`: task-scoped `typos` pinned exactly to `1.52.0` (`1.52.2` exists).
- `.config/mise/conf.d/b.toml`: task-scoped `cargo-shear` at range `1.14`, locked at `1.14.0` (`1.14.1` exists). Renovate updates this by editing the lock only.
- `renovate.json` groups all mise updates into one branch.

## Current behavior

Renovate bumps the pins in `config.toml` and `conf.d/a.toml`, and hand-edits `cargo-shear` in `mise.lock`. That hand-edited lock is passed as `newLockFileContent` to every package file's `updateArtifacts`, so each `mise lock` run starts from the stale hand-edited lock and overwrites the previous run's refresh. Only the last package file's lock changes survive: the config pins are bumped but their `mise.lock` entries stay at the old versions.

## Expected behavior

`mise.lock` contains every bumped version (`actionlint` 1.7.12, `typos` 1.52.2 or newer, `cargo-shear` 1.14.1).

## Run

```sh
RENOVATE_TOKEN="$(gh auth token)" RENOVATE_ALLOWED_UNSAFE_EXECUTIONS='["mise"]' RENOVATE_ONBOARDING=false RENOVATE_REQUIRE_CONFIG=required \
  npx --yes renovate@44.234.1 --platform=github 20jasper/renovate-mise-lock-repro
```

Real case: https://github.com/20jasper/JJPWRGEM/pull/613 (Renovate commit a5dd1440).
