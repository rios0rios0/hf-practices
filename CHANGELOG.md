# Changelog

All notable changes to this project will be documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), and this project adheres to [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

When a new release is proposed:

1. Create a new branch `bump/x.x.x` (this isn't a long-lived branch!!!);
2. The Unreleased section on `CHANGELOG.md` gets a version number and date;
3. Open a Pull Request with the bump version changes targeting the `main` branch;
4. When the Pull Request is merged, a new Git tag must be created using <LINK TO THE PLATFORM TO OPEN THE PULL REQUEST>.

Releases to productive environments should run from a tagged version.
Exceptions are acceptable depending on the circumstances (critical bug fixes that can be cherry-picked, etc.).

## [Unreleased]

### Added

- created `CLAUDE.md` to give Claude Code sessions repo-specific guidance (no build system, manual reference counting, per-sub-project architecture)

### Changed

- corrected `.github/copilot-instructions.md` DrinkMixer constants: keys are `NAME_KEY`, `INGREDIENTS_KEY`, `DIRECTIONS_KEY` (`name`/`ingredients`/`directions`) declared as `#define` macros, not `NSString * const`

## [0.1.2] - 2026-05-19

### Changed

- refreshed `.github/copilot-instructions.md` to document the `release.yaml` CI workflow and update the repository structure tree

## [0.1.1] - 2026-04-28

### Changed

- refreshed `.github/copilot-instructions.md` to include `CHANGELOG.md` in the repository structure listing

## [0.1.0] - 2026-03-24

The changes weren't tracked until this version.
