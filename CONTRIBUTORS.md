# Contributors

> **Product:** [Product Name]
> **Organization:** HealthyX-ai

---

## Core Team

| Name | Role | GitHub |
|------|------|--------|
| [Name] | [Role] | @[username] |

---

## How to Contribute

This is a private repository. Contributions are limited to authorized members of the HealthyX-ai organization.

### Before You Start

1. **Open an issue first** — for any new feature or significant change, open an issue and describe the problem before writing code. The solution is secondary to understanding the problem.
2. **Check existing issues** — your problem may already be tracked.
3. **Read CLAUDE.md** — all code must follow the standards defined there.

### The Workflow

1. Create a feature branch from `main`
2. Write the code — following CLAUDE.md standards
3. Test in POC before opening a PR
4. Open a PR using the PR template — link the issue
5. Wait for CODEOWNERS to auto-assign a reviewer
6. Address review feedback
7. The reviewer merges — you do not merge your own PRs

### Branch Naming

| Type | Pattern | Example |
|------|---------|---------|
| Feature | `feature/description` | `feature/hcc-v28-hierarchy` |
| Bug fix | `fix/description` | `fix/raf-calculation-rounding` |
| Hotfix | `hotfix/description` | `hotfix/prod-auth-timeout` |
| Docs | `docs/description` | `docs/update-implementation-guide` |
| Infrastructure | `infra/description` | `infra/add-prod-environment` |
| Refactor | `refactor/description` | `refactor/extract-validation` |
| Chore | `chore/description` | `chore/update-dependencies` |

### Commit Format

```
type: short description (50 chars max)

Optional body — what and why, not how.
Reference issues: closes #123
```

Types: `feat` `fix` `hotfix` `docs` `infra` `refactor` `test` `chore`

---

## Acknowledgments

Built on:
- [Anthropic Claude](https://anthropic.com) — AI capabilities via the Claude API
- [Cloudflare Workers](https://workers.cloudflare.com) — serverless infrastructure
- [NOMS Healthcare](https://nomshealthcare.com) — the clinical environment where this was built and validated
