# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]
### Added
- `tailscale:funnel` command to expose apps to the public internet via Tailscale Funnel
- `tailscale_funnel_start` and `tailscale_funnel_stop` functions
- `tailscale_funnel_url` function to get the public funnel URL
- Funnel URL display in `app-urls` output
- Documentation and examples for Tailscale Funnel usage

## [0.1.1] - 2024-05-01
### Added
- MIT License

### Changed
- `install` attempts to change the ownership of the tailscale dir to dokku:dokku

## [0.1.0] - 2024-04-21
### Added
- `tailscale_attach`

### Changed
- Add ts container to app container network, not the other way around
- Rename `tailscale_down` to `tailscale_detach`
- Data storage goes in a plugin data dir, not the storage dir
- Print tailscale serve url first

### Fixed
- `tailscale_serve_url` does not request TTY during exec

## [0.0.4] - 2024-04-19
### Changed
- Make `tailscale:up` idempotent

### Fixed
- Changelog tag links
- Shorter help text when not calling tailscale:help

## [0.0.3] - 2024-04-13
### Fixed
- `tailscale_serve_url` correction to the conditional

## [0.0.2] - 2024-04-13
### Added
- This changelog
- `tailscale_is_running_for_app` function
- `tailscale_serve_url` function
- `app-urls` trigger

## [0.0.1] - 2024-04-13
### Added
- `tailscale:up` command
- `tailscale:down` command
- `tailscale:serve` command
- `tailscale_up` function
- `tailscale_down` function
- `tailscale_serve_start` function
- `tailscale_serve_stop` function
- `post-destroy` trigger

[unreleased]: https://github.com/andrew-womeldorf/dokku-tailscale/compare/0.1.0...HEAD
[0.1.0]: https://github.com/andrew-womeldorf/dokku-tailscale/compare/0.0.4...0.1.0
[0.0.4]: https://github.com/andrew-womeldorf/dokku-tailscale/compare/0.0.3...0.0.4
[0.0.3]: https://github.com/andrew-womeldorf/dokku-tailscale/compare/0.0.2...0.0.3
[0.0.2]: https://github.com/andrew-womeldorf/dokku-tailscale/compare/0.0.1...0.0.2
[0.0.1]: https://github.com/andrew-womeldorf/dokku-tailscale/releases/tag/0.0.1
