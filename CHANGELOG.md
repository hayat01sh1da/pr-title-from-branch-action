# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [0.1.1] - 2026-08-06

### 1. Changed

- Bumped the interpreter provisioned by `actions/setup-python` 3.14.6 -> 3.14.7, picking up the CPython patch release of 2026-08-05 — notably the `tarfile` extraction-filter bypass (`gh-151558`) and the `html.parser` / `xml.etree.ElementTree` quadratic-parsing denial-of-service fixes (`gh-153030`, `gh-152674`).
- Bumped the pinned `spreen-pr` version 0.1.1 -> 0.1.2, keeping the pin on the current release per the per-release pinning policy.
  The packaged `pr-title` behaviour is unchanged, so consumer runs differ only in the interpreter.
- `README.md` pins `@v0.1.1` in the usage examples and the versioning-policy note.

## [0.1.0] - 2026-07-31

### 1. Added

- Initial composite action **PR Title from Branch**: installs the pinned [`spreen-pr`](https://pypi.org/project/spreen-pr/) PyPI package (0.1.1), derives the title and labels from the pull request's head branch (`branch-name` overridable) via `pr-title --format json`, and applies them to the pull request through the API.
- Inputs `github-token`, `apply-title` (default `true`), `apply-label` (default `false`), `create-label` (default `false`) and `overwrite` (default `false` — the title is only applied on `opened` runs so manually edited titles are never clobbered on `synchronize`).
- Outputs `title` and `labels` for compute-only (outputs-driven) usage.
- `Action - CI` workflow dry-running the derivation against the standard, hotfix and malformed branch shapes with `apply-title: 'false'`.
- `README.md` with usage, inputs/outputs, token & permissions guidance, and versioning policy; MIT `LICENSE.txt`.
