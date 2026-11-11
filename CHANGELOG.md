# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.0.0] - 2026-11-11

### Added
- `version-check.yml` reusable workflow: runs `scripts/version.sh check` in the calling repo
- `release.yml` reusable workflow: on a `vX.Y.Z` tag, checks the tag matches `VERSION` and
  makes the GitHub release from that version's `CHANGELOG.md` section; can move the major tag
- `scripts/version.sh` to bump and check the version in `VERSION` and `CHANGELOG.md`, with
  optional version markers in other files set by `scripts/version.conf`
- `examples/version.yml`, the caller workflow to copy into a repo

[Unreleased]: https://github.com/ysasiwat/github-workflows/compare/v1.0.0...HEAD
[1.0.0]: https://github.com/ysasiwat/github-workflows/releases/tag/v1.0.0
