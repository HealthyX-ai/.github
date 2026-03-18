# Changelog

> **Product:** [Product Name]
> Follows [Keep a Changelog](https://keepachangelog.com/en/1.0.0/) format.
> Versioning follows [Semantic Versioning](https://semver.org/) — MAJOR.MINOR.PATCH.

---

## [Unreleased]

Changes staged for the next release.

### Added
-

### Changed
-

### Fixed
-

### Security
-

---

## [1.0.0] - YYYY-MM-DD

### Added
- Initial release

---

## Versioning Reference

| Change type | Bump | Example |
|-------------|------|---------|
| Breaking change or major new capability | MAJOR | 1.0.0 → 2.0.0 |
| New feature, backward compatible | MINOR | 1.0.0 → 1.1.0 |
| Bug fix, patch, hotfix | PATCH | 1.0.0 → 1.0.1 |

---

## Release Process

1. Update this file — move "Unreleased" items to the new version with today's date
2. Bump the version in `package.json` / `.csproj` (whichever applies)
3. Open a PR with the changelog update
4. Merge to `main` — pipeline promotes to POC → TST → SUP → PROD with required approvals
