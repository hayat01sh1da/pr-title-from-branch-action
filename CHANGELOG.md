# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [0.1.0] - 2026-07-31

### 1. Added

- Initial composite action **PR Title from Branch**: installs the pinned [`spreen-pr`](https://pypi.org/project/spreen-pr/) PyPI package (0.1.1), derives the title and labels from the pull request's head branch (`branch-name` overridable) via `pr-title --format json`, and applies them to the pull request through the API.
- Inputs `github-token`, `apply-title` (default `true`), `apply-label` (default `false`), `create-label` (default `false`) and `overwrite` (default `false` — the title is only applied on `opened` runs so manually edited titles are never clobbered on `synchronize`).
- Outputs `title` and `labels` for compute-only (outputs-driven) usage.
- `Action - CI` workflow dry-running the derivation against the standard, hotfix and malformed branch shapes with `apply-title: 'false'`.
- `README.md` with usage, inputs/outputs, token & permissions guidance, and versioning policy; MIT `LICENSE.txt`.
