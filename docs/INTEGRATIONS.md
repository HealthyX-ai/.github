# Integrations

> **Product:** [Product Name]
> **Last Updated:** YYYY-MM-DD

All external systems, APIs, and HealthyX suite connections this product depends on or exposes.

---

## Integration Summary

| Integration | Type | Direction | Environments | Status |
|-------------|------|-----------|-------------|--------|
| Anthropic Claude API | AI / LLM | Outbound | All | Active |
| Cloudflare Workers | Infrastructure | — | All | Active |
| Cloudflare KV | Storage | Read/Write | All | Active |
| [Add others] | — | — | — | — |

---

## Anthropic Claude API

**Purpose:** AI and language model capabilities.

| Property | Value |
|----------|-------|
| Secret | `ANTHROPIC_API_KEY` (org-level, auto-inherited) |
| Default model | `claude-sonnet-4-6` |
| Endpoint | `https://api.anthropic.com/v1/messages` |
| Docs | https://docs.anthropic.com |

**Notes:**
- Default to `claude-sonnet-4-6` unless the task demonstrably requires Opus
- API key is managed org-wide — do not create product-specific keys

---

## Cloudflare Workers

**Purpose:** Serverless compute hosting the application runtime.

| Property | Value |
|----------|-------|
| Secret | `CLOUDFLARE_ACCOUNT_ID` (org-level, auto-inherited) |
| Deploy tool | Wrangler CLI |
| Docs | https://developers.cloudflare.com/workers |

**Environment mapping:**

| GitHub Environment | Cloudflare Environment |
|-------------------|----------------------|
| POC | `poc` |
| TST | `tst` |
| SUP | `sup` |
| PROD | `production` |

---

## Cloudflare KV

**Purpose:** Key-value storage for application state and caching.

| Property | Value |
|----------|-------|
| Variable | `CF_KV_NAMESPACE_ID` (environment-level — different per environment) |
| Docs | https://developers.cloudflare.com/kv |

**Namespace IDs:**

| Environment | KV Namespace ID |
|-------------|----------------|
| POC | Set in GitHub environment variables |
| TST | Set in GitHub environment variables |
| SUP | Set in GitHub environment variables |
| PROD | Set in GitHub environment variables |

---

## HealthyX Suite Connections

<!-- Document any other HealthyX products this integrates with -->

| Product | Connection type | Direction | Notes |
|---------|----------------|-----------|-------|
| HealthyBase | [API / data read] | [Inbound/Outbound] | [Description] |
| [Other] | — | — | — |

---

## Adding a New Integration

When adding a new external system:

1. Document it in this file **before** implementing
2. Add any new secrets to GitHub org or repo secrets — never hardcode
3. Add the secret name to `CLAUDE.md` under Secrets & Variables
4. Update `deploy.yml` if the integration requires environment-specific configuration
5. Note if behavior differs across environments (POC vs PROD)
6. Confirm HIPAA compliance requirements if the integration touches PHI
