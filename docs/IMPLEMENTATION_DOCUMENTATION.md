# Implementation Guide

> **Product:** [Product Name]
> **Version:** See [Changelog](VERSION_DOCUMENTATION.md)
> **Last Updated:** YYYY-MM-DD
> **Audience:** HealthyX implementation team. This document is not for end users.

---

## Prerequisites

Everything that must be in place before implementation begins. Do not start without confirming all of these.

### Infrastructure
- [ ] Cloudflare account access confirmed
- [ ] HealthyX-ai GitHub org membership active
- [ ] Org secrets confirmed inheriting (`ANTHROPIC_API_KEY`, `CLOUDFLARE_ACCOUNT_ID`)
- [ ] Org variable `ORG_ACTIONS_ENABLED` is set to `true` (confirm with org admin)
- [ ] Cloudflare KV namespaces created for all required environments

### Client Requirements
- [ ] Client environment URLs confirmed
- [ ] Client data access and credentials provided
- [ ] BAA executed (required before any PHI processing)
- [ ] Go-live stakeholders and approvers identified
- [ ] On-call contact for launch window confirmed

---

## Environment Overview

| Environment | Purpose | Deploy Trigger | Approval |
|-------------|---------|---------------|---------|
| POC | Build and initial dev | Push to `main` | None |
| TST | Integration testing | Auto after POC passes | None |
| SUP | Staging — 1 day behind PROD | Auto after TST + 1 reviewer | 1 reviewer |
| PROD | Production | Auto after SUP + 1 reviewer | 1 reviewer, no self-review |
| DEMO_SUP | Demo staging | Auto after TST (when `DEMO_ENABLED = true`) | 1 reviewer |
| DEMO_PROD | Demo production | Auto after DEMO_SUP | 1 reviewer, no self-review |
| [ORG]_SUP | Client org staging | Auto after TST (when `[ORG]_ENABLED = true`) | 1 reviewer |
| [ORG]_PROD | Client org production | Auto after [ORG]_SUP | 1 reviewer, no self-review |

---

## Configuration Reference

| Variable / Secret | Scope | Description |
|-------------------|-------|-------------|
| `ANTHROPIC_API_KEY` | Org Secret | Anthropic Claude API key — auto-inherited |
| `CLOUDFLARE_ACCOUNT_ID` | Org Secret | Cloudflare account — auto-inherited |
| `ORG_ACTIONS_ENABLED` | Org Variable | Master kill switch — auto-inherited. Must be `true` for any workflow to run |
| `ACTIONS_ENABLED` | Repo Variable | Repo kill switch — `true` to run, `false` to halt |
| `DEMO_ENABLED` | Repo Variable | Enables DEMO_SUP and DEMO_PROD deploy jobs |
| `[ORG]_ENABLED` | Repo Variable | Enables client org deploy jobs (e.g. `NOMS_ENABLED`) |
| `CF_KV_NAMESPACE_ID` | Env Variable | KV namespace — different per environment |
| `DEPLOY_URL` | Env Variable | Target URL per environment |
| `ENV_NAME` | Env Variable | Environment string (POC / TST / SUP / PROD / DEMO_SUP / etc.) |
| `ANTHROPIC_API_KEY` | Env Secret | Environment-scoped API key for per-environment billing tracking |

---

## Step-by-Step Setup

### Step 1 — Create the Repo from Template

1. Go to **github.com/HealthyX-ai** → click **"New repository"**
2. Under **"Repository template"** select `healthyx-service-template`
3. Name the repo (use `Healthy[Name]` convention)
4. Set visibility to **Private**
5. Click **"Create repository"**

### Step 2 — Configure Repo Variables

Navigate to the new repo → **Settings → Secrets and variables → Actions → Variables tab**

Add:

| Variable | Value | Purpose |
|----------|-------|---------|
| `ACTIONS_ENABLED` | `true` | Repo kill switch — set to `false` to halt all workflows |
| `DEMO_ENABLED` | `true` or `false` | Enables DEMO_SUP and DEMO_PROD deploy jobs |
| `[ORG]_ENABLED` | `true` or `false` | Enables client org deploy jobs (e.g. `NOMS_ENABLED`) |

> **Prerequisite:** The org-level variable `ORG_ACTIONS_ENABLED` must also be `true`. If it is not set, all workflows will skip. Confirm with an org admin.

### Step 3 — Configure Environment Variables

Navigate to **Settings → Environments** → click each environment → add variables:

For each environment (POC, TST, SUP, PROD):

| Variable | Value |
|----------|-------|
| `CF_KV_NAMESPACE_ID` | The KV namespace ID for this environment |
| `DEPLOY_URL` | The deployment URL for this environment |
| `ENV_NAME` | `POC` / `TST` / `SUP` / `PROD` |

### Step 4 — Update CLAUDE.md and README.md

Replace all `[placeholder]` fields with actual product details.

### Step 5 — Update CODEOWNERS

Replace `@HealthyX-ai/owners` placeholders with actual GitHub usernames or confirmed team names.

### Step 6 — Configure Branch Protection Rules

Navigate to **Settings → Branches → Add branch protection rule** for `main`:

**Pull request requirements:**
- [x] Require a pull request before merging
  - Required approving reviews: **1**
  - [x] Dismiss stale pull request approvals when new commits are pushed
  - [x] Require review from Code Owners

**Status checks:**
- [x] Require status checks to pass before merging
  - [x] Require branches to be up to date before merging
  - Add these as **required status checks:**
    - `Check PR Standards` (from `standards-check.yml`)
    - `Validate PR` (from `pr-checks.yml`)

**Other protections:**
- [x] Require conversation resolution before merging
- [x] Do not allow bypassing the above settings
- [x] Restrict who can push to matching branches — limit to repo admins only

> These settings block merges until a PR has an approving review and all checks pass. Do not weaken them without team agreement.

### Step 7 — Configure Environment Protection Rules

Navigate to **Settings → Environments** for each environment:

**POC and TST** — no protection rules needed.

**SUP:**
- [x] Required reviewers: add at least **1** reviewer
- Deployment branches: `main` only

**PROD:**
- [x] Required reviewers: add at least **1** reviewer
- [x] Prevent self-review (the person who triggered the deploy cannot approve it)
- Deployment branches: `main` only

**DEMO_SUP** (if using demos):
- [x] Required reviewers: add at least **1** reviewer
- Deployment branches: `main` only

**DEMO_PROD** (if using demos):
- [x] Required reviewers: add at least **1** reviewer
- [x] Prevent self-review
- Deployment branches: `main` only

**Client org environments** (e.g. NOMS_SUP, NOMS_PROD):
- Follow the same pattern as SUP/PROD — 1 reviewer, prevent self-review on PROD.

> Environment protection rules control who can approve deployments. The deploy pipeline pauses at SUP and PROD and waits for approval in the GitHub Actions UI. Without these rules, deployments flow through with no gate.

### Step 8 — Initial Deployment

1. Push initial code to `main`
2. Watch **Actions** tab — pipeline should flow POC → TST automatically
3. Approve the SUP deployment when prompted
4. Validate SUP for 24–48 hours before proceeding to PROD
5. Approve the PROD deployment

---

## Go-Live Checklist

### Pre-Launch
- [ ] POC pipeline passing cleanly
- [ ] TST integration tests passing
- [ ] SUP stable for 48 hours minimum
- [ ] Client sign-off on SUP
- [ ] PROD environment variables confirmed
- [ ] BAA in place (required before PROD with PHI)
- [ ] Rollback procedure reviewed with the team

### Launch
- [ ] PROD deployment approved by someone who did not trigger it
- [ ] Post-deploy smoke tests passing
- [ ] Client notified of go-live
- [ ] Monitoring confirmed active

### Post-Launch (first 24 hours)
- [ ] No P1 or P2 errors in logs
- [ ] Performance baseline established
- [ ] Client confirmed system operating as expected
- [ ] Support handoff completed

---

## Rollback Procedure

If a PROD issue is identified post-deployment:

1. **Immediately:** Set `ACTIONS_ENABLED` to `false` in repo variables to halt further pipeline runs
2. **Identify:** Determine last known good commit from [Changelog](VERSION_DOCUMENTATION.md)
3. **Revert:** `git revert [commit]` and push to `main` — pipeline will re-run through POC → TST → SUP → PROD with approval gates
4. **Communicate:** Notify stakeholders within 15 minutes of initiating rollback
5. **Document:** Record the incident in [Changelog](VERSION_DOCUMENTATION.md) and [SUPPORT.md](SUPPORT.md) known issues

---

## Post-Implementation Handoff

Deliver to the client with:

- [ ] This document updated with actual values filled in
- [ ] [User Documentation](USER_DOCUMENTATION.md) reviewed and accurate
- [ ] Monitoring and alerting active
- [ ] Support contacts established per [SUPPORT.md](SUPPORT.md)
- [ ] Client team trained on day-to-day use
