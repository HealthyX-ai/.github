# Security Policy

> **HealthyX-ai Organization**
> This policy applies to all repositories in the HealthyX-ai organization.

---

## Reporting a Vulnerability

**Do not open a public GitHub issue for security vulnerabilities.**

If you discover a security vulnerability in any HealthyX repository or product, report it privately and directly to the HealthyX security team.

**Email:** analytics@nomshealthcare.com
**Subject line:** `[SECURITY] Brief description — do not include details in subject`

Include in your report:
- Which repository or product is affected
- A description of the vulnerability and how it can be reproduced
- The potential impact if exploited
- Any suggested remediation if you have one

---

## PHI Exposure Incidents

If you believe Protected Health Information (PHI) has been accidentally exposed — in a commit, an issue, a log, or any other medium — this is a HIPAA-reportable incident.

**Report PHI exposure immediately:**

1. Contact a HealthyX organization administrator directly — do not wait
2. Do **not** attempt to investigate or remediate the exposure yourself
3. Do **not** open a GitHub issue describing the exposure
4. Preserve all relevant context (commit hash, timestamp, what was exposed) for the incident report

PHI incidents reported in good faith are not treated as conduct violations. PHI incidents that are concealed are.

---

## Response Process

| Step | Timeframe |
|------|-----------|
| Acknowledgment of report | Within 24 hours |
| Initial severity assessment | Within 48 hours |
| Remediation plan communicated to reporter | Within 5 business days |
| Resolution and follow-up | Dependent on severity |

We will keep you informed of progress. We will not disclose your identity without your permission.

---

## Supported Versions

HealthyX products follow a continuous deployment model. Only the current production version (`PROD` environment) receives security patches. Organizations running older versions should contact HealthyX support to arrange an upgrade path.

---

## Scope

This security policy covers all repositories in the HealthyX-ai GitHub organization, including:

- All external suite products (HealthyClaim, HealthyBase, etc.)
- All platform and shared service repositories
- Internal operations repositories
- The organization's CI/CD pipeline and infrastructure repositories

---

## What We Ask of Contributors

All contributors to HealthyX repositories are expected to:

- Never commit secrets, API keys, tokens, or credentials to any repository
- Never include PHI, patient data, or client-identifying information in any GitHub content
- Enable two-factor authentication (2FA) on their GitHub account — this is required for org membership
- Report suspected vulnerabilities or PHI exposures immediately, not after attempting to resolve them independently
- Follow the principle of least privilege — request only the access you need

---

## Hardening Standards

All HealthyX repositories implement:

- Branch protection on `main` requiring PR review before merge
- Secret scanning with push protection enabled — GitHub will block commits containing known secret patterns
- Dependabot alerts for vulnerable dependencies
- Environment protection rules requiring reviewer approval before PROD deployments
- Three-layer kill switch architecture for pipeline control

---

## Acknowledgment

HealthyX appreciates responsible disclosure. Researchers and contributors who report valid vulnerabilities in good faith will be acknowledged (with permission) in the relevant repository's changelog.
