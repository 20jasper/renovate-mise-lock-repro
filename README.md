# renovate-mise-lock-repro

> Built with Claude Code (Claude Opus 5.5).

mise config files sharing one `mise.lock` lose lock updates when a grouped branch also has a lockfile-only update.

- [`config.toml`](.config/mise/config.toml): actionlint 1.7.11
- [`conf.d/a.toml`](.config/mise/conf.d/a.toml): typos 1.52.0
- [`conf.d/b.toml`](.config/mise/conf.d/b.toml): cargo-shear `1.14`, locked at 1.14.0 (lockfile-only update)

## Result

- Bug, Renovate 44.234.1: [#1](https://github.com/20jasper/renovate-mise-lock-repro/pull/1), typos [bumped to 1.52.2](https://github.com/20jasper/renovate-mise-lock-repro/blob/338b2c80592998edd9e07bade1fdf36852380f35/.config/mise/conf.d/a.toml#L5) but [locked at 1.52.0](https://github.com/20jasper/renovate-mise-lock-repro/blob/338b2c80592998edd9e07bade1fdf36852380f35/.config/mise/mise.lock#L5-L6)
- Fixed: [#3](https://github.com/20jasper/renovate-mise-lock-repro/pull/3), typos [locked at 1.52.2](https://github.com/20jasper/renovate-mise-lock-repro/blob/f6befef0165ef15bdf22365c8241f8748ff0371e/.config/mise/mise.lock#L5-L6)
- Real case: [20jasper/JJPWRGEM#613](https://github.com/20jasper/JJPWRGEM/pull/613)
