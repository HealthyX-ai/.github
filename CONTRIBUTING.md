# Contributing to HealthyX

> **HealthyX-ai Organization**
> This document applies to all repositories in the HealthyX-ai organization.

---

## Who Can Contribute

HealthyX repositories are private. Contributions are limited to:

- Authorized HealthyX employees and contractors
- Implementation partners operating under a current signed agreement

If you need access and do not have it, contact a HealthyX organization administrator.

---

## Before You Write a Line of Code

**1. Open an issue first.**
For any new feature, significant change, or non-trivial bug fix — open an issue and describe the problem before writing code. The team needs to understand the problem and agree on the approach. Code written before alignment is frequently rewritten.

**2. Read the repo's `CLAUDE.md`.**
Every HealthyX repository has a `CLAUDE.md` in its root. It defines the stack, coding standards, environment conventions, and project-specific rules. Read it before you start. If you are using Claude Code, it loads automatically.

**3. Check existing issues and pull requests.**
Your problem may already be tracked. Your solution may already be in progress.

---

## The Workflow

```
main branch (protected)
    ↑
feature branch → pull request → code review → squash merge
```

### Step by step

1. **Create a branch** from `main` using the naming conventions below
2. **Write the code** following the standards in `CLAUDE.md`
3. **Test in POC** before opening a pull request — do not open a PR for code you have not run
4. **Open a pull request** using the PR template — every field matters, fill it out
5. **Link the issue** — every PR must reference a GitHub issue (`Closes #123`)
6. **Wait for CODEOWNERS** to auto-assign a reviewer — do not manually assign reviewers
7. **Address review feedback** — respond to every comment, either with a change or an explanation
8. **The reviewer merges** — you do not merge your own pull requests

---

## Branch Naming

| Type | Pattern | Example |
|------|---------|---------|
| Feature | `feature/short-description` | `feature/hcc-v28-hierarchy` |
| Bug fix | `fix/short-description` | `fix/raf-rounding-error` |
| Hotfix | `hotfix/short-description` | `hotfix/prod-auth-timeout` |
| Documentation | `docs/short-description` | `docs/update-implementation-guide` |
| Infrastructure | `infra/short-description` | `infra/add-sup-environment` |
| Refactor | `refactor/short-description` | `refactor/claim-processing-logic` |

Keep descriptions short and lowercase. Use hyphens, not underscores.

---

## Commit Messages

```
type: short description (50 characters max)

Optional body — explain what and why, not how.
Keep lines under 72 characters.

Reference issues: closes #123
```

**Types:** `feat` `fix` `docs` `infra` `refactor` `test` `chore`

**Good examples:**
```
feat: add V28 hierarchy suppression logic
fix: correct RAF coefficient rounding at position 12
docs: update implementation guide with PROD rollback steps
infra: add SUP environment protection rules
```

**Not good:**
```
updates
fixed stuff
WIP
asdfgh
```

---

## Pull Request Standards

Every PR must:

- Use the repository's PR template — do not delete sections
- Link a GitHub issue with `Closes #[number]`
- Have been tested in POC before opening
- Pass all automated checks (build, tests, lint) before review is requested
- Include the healthcare/compliance section completed honestly

Every PR must not:

- Include PHI, patient data, credentials, or API keys in any form
- Push directly to `main` — PRs only
- Be merged by the author

---

## Code Standards

Each repository defines its own stack-specific standards in `CLAUDE.md`. Across all HealthyX repositories, these apply universally:

- **No hardcoded secrets.** Use GitHub Secrets and Variables. Always.
- **No PHI in code, comments, commits, or issues.** Ever.
- **Active voice in user-facing strings.** "The claim could not be submitted" not "Submission failure was encountered."
- **Error messages tell the user what happened and what to do** — not just that something failed.
- **Tests are required for new functionality.** Code without tests will not be merged.

---

## Environments

| Environment | Purpose | Who deploys |
|-------------|---------|-------------|
| POC | Development — test your work here first | Anyone with Write access |
| TST | Integration — automated after POC passes | Pipeline (automatic) |
| SUP | Staging — 1 day behind PROD | Pipeline + 1 reviewer approval |
| PROD | Production | Pipeline + 1 reviewer approval, no self-review |

Never bypass environment protection rules. If you need an emergency PROD fix, follow the hotfix branch process and still get a reviewer.

---

## Security & Compliance

Because HealthyX handles Protected Health Information:

- Never commit secrets, API keys, tokens, or credentials to any repository
- Never include patient data, client data, or PHI in any GitHub content
- If you discover a security vulnerability, report it through the process in `SECURITY.md` — do not open a public issue
- If you accidentally commit sensitive data, contact an organization administrator immediately

See [SECURITY.md](SECURITY.md) for the full vulnerability reporting process.

---

## Questions

If something in this guide is unclear, open an issue in the relevant repository or contact an organization administrator. The answer may improve this document.
