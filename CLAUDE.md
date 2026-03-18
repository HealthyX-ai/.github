# CLAUDE.md

> Loaded automatically by Claude Code. Defines project-specific standards
> so they apply every session without being restated. Update this file as
> the project evolves — it is the source of truth for how Claude Code
> operates in this repo.

---

## Project

**Product:** [Product Name]
**Pillar:** [Infrastructure / Strategy / Automation / Integrity / Discovery]
**Tagline:** *[e.g. Intelligence from visit to payment.]*
**Org:** HealthyX-ai
**Description:** [One sentence — what this does and who uses it]

---

## Stack

- **Runtime:** [e.g. Cloudflare Workers (Node 20) / ASP.NET Core 8]
- **Language:** [e.g. TypeScript / C#]
- **Database:** [e.g. Cloudflare D1 / Cloudflare KV / SQL Server]
- **Infrastructure:** Cloudflare Workers + KV
- **AI:** Anthropic Claude API (`claude-sonnet-4-6` default)
- **Deployment:** GitHub Actions — POC → TST → SUP → PROD

---

## Coding Standards

- Follow language idioms and conventions — no cleverness for its own sake
- Single-purpose functions — if it does two things, split it
- Error handling is required — never swallow exceptions silently
- No magic strings — use constants or enums
- Comments explain *why*, not *what* — the code explains what
- No hardcoded secrets, URLs, or environment-specific values
- Tests are required for all new functionality — code without tests will not be merged

---

## File Structure

```
.github/          GitHub Actions, PR template, CODEOWNERS, issue templates
docs/             All project documentation
resources/        Brand assets — logos, fonts
src/              Application source code
CLAUDE.md         This file
README.md         Project overview
LICENSE.md        Licensing
CONTRIBUTORS.md   Team and contribution guide
SECURITY.md       Vulnerability reporting and PHI incident process
wrangler.toml     Cloudflare Workers configuration
```

---

## Environment Pipeline

| Environment | Purpose | Approval | Who Has Access |
|-------------|---------|---------|---------------|
| POC | Active development — build freely | None | Internal HealthyX team |
| TST | Integration testing — tests must pass | None | Internal HealthyX team |
| SUP | Staging — 1 day behind PROD | 1 reviewer | Internal HealthyX team |
| PROD | Production | 1 reviewer, no self-review | Internal HealthyX team |
| DEMO_SUP | Demo staging | 1 reviewer | Internal (demos only) |
| DEMO_PROD | Demo production | 1 reviewer, no self-review | Internal (demos only) |
| [ORG]_SUP | Client org staging | 1 reviewer | HealthyX + that org |
| [ORG]_PROD | Client org production | 1 reviewer, no self-review | HealthyX + that org |

- POC is expendable. PROD is not. Test accordingly.
- Never write environment-specific logic in application code — use env variables
- If it works in POC but not TST, the problem is configuration, not code
- Client org environments are controlled by `[ORG]_ENABLED` repo variable

---

## Secrets & Variables

### Never hardcode — always reference via environment

**Org-level (auto-inherited — do not redeclare):**
- `ANTHROPIC_API_KEY` — Anthropic Claude API key (also set per-environment)
- `CLOUDFLARE_ACCOUNT_ID` — Cloudflare account
- `GH_ORG_NAME` — HealthyX-ai

**Repo-level variables:**
- `ACTIONS_ENABLED` — repo kill switch (`true`/`false`)
- `DEMO_ENABLED` — enables DEMO_SUP and DEMO_PROD jobs (`true`/`false`)
- `[ORG]_ENABLED` — enables client org jobs (e.g. `NOMS_ENABLED = true`)

**Environment-level (per environment in GitHub Settings → Environments):**
- `CF_KV_NAMESPACE_ID` — Cloudflare KV namespace for this environment
- `DEPLOY_URL` — deployment URL for this environment
- `ENV_NAME` — environment name string (set automatically by sync bot)
- `ANTHROPIC_API_KEY` (secret) — environment-scoped API key for billing tracking

**Anthropic API key naming convention (for Anthropic console):**
- Internal: `HEALTHYX_[ENV]_[REPO]_ANTHROPIC_API_KEY`
- Demo: `DEMO_[ENV]_[REPO]_ANTHROPIC_API_KEY`
- Client org: `[ORG]_[ENV]_[REPO]_ANTHROPIC_API_KEY`

Example: `NOMS_PROD_HEALTHYCLAIM_ANTHROPIC_API_KEY`
This name in Anthropic's console lets you track spend per org per environment.

### Kill Switch Priority
`ORG_ACTIONS_ENABLED` → `ACTIONS_ENABLED` → environment protection rules

---

## GitHub Workflow

- All work in feature branches — never commit directly to `main`
- Branch naming: `type/description` using lowercase and hyphens
  - Types: `feature`, `fix`, `hotfix`, `docs`, `infra`, `refactor`, `chore`
  - Example: `feature/hcc-v28-hierarchy`
- PR title format: `type: description` (min 5 chars after colon)
  - Types: `feat`, `fix`, `docs`, `infra`, `refactor`, `test`, `chore`, `hotfix`
- Every PR must use the PR template and link a GitHub issue (`Closes #123`)
- Squash merge to main — keep history clean
- PRs require 1 reviewer minimum — you cannot merge your own PR
- CODEOWNERS auto-assigns reviewers — do not bypass
- standards-check.yml enforces branch naming, PR title, body length, issue link, and compliance checkbox

---

## Healthcare & Compliance

This product may process Protected Health Information (PHI).

- Never log PHI — scrub before writing to any log
- Never include PHI in issues, PR descriptions, commits, or comments
- PHI handling changes require a reviewer who is not the author
- Data access pattern changes must be called out explicitly in the PR template
- When uncertain whether something touches PHI, treat it as if it does
- See SECURITY.md for the PHI incident reporting process

---

## HealthyX Brand Standards in This Codebase

**Brand colors:**
- Navy `#2d3748` — primary brand, headings, navigation
- Green `#48bb78` — accent, highlights, success states, CTAs

**Voice for user-facing strings, error messages, and UI copy:**

| Avoid | Use instead |
|-------|------------|
| "An error has occurred" | "The claim could not be submitted — check the error details below" |
| "Invalid input" | "The diagnosis code format is incorrect — ICD-10 format expected" |
| "Processing..." | "Reviewing claim codes..." |
| "Utilize" | "Use" |
| "Seamless" / "robust" / "leverage" | Plain language describing the actual thing |

**Rules:**
- Active voice always: "HealthyClaim identifies codes" not "codes are identified"
- Direct and plain — say what happened and what to do
- Never hedge with "we believe" or "we feel" — state facts

---

## What NOT to Do

- Do not push directly to `main`
- Do not merge your own PRs
- Do not hardcode secrets, API keys, or environment-specific values
- Do not include PHI in any GitHub content
- Do not set kill switches to `false` without team awareness
- Do not skip the PR template or issue link requirement
- Do not use brand-prohibited words in user-facing text
- Do not commit `.pem`, `.key`, `.env`, or `.dev.vars` files
