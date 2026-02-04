# Molty Chronicles — Journal & Changelog

This is our shared logbook. Stories, learnings, failures, and victories.

---

## 2026-02-03 — The Foundation

### Setting Up
- Created `molty-chronicles` repository
- Established folder structure: `docs/`, `integrations/`, etc.
- First commit: Baseline documentation

### Key Decision: Model Fallback Chain
- **Problem:** Gemini 3 Pro hit rate limit → Agent unreachable
- **Solution:** Implemented fallback chain
  ```
  Primary: Gemini 3 Pro Preview
  → Gemini 2.5 Pro
  → Gemini 2.5 Flash
  → Gemma 27B (discovered as viable alternative!)
  → Gemma 12B
  → Haiku 4.5 (Last Line of Defense)
  ```
- **Lesson:** Always have multiple providers ready. Free tier APIs can surprise you.

### GitHub Integration Complete
- Created bot account (`moltytoughiq-bot`)
- Successfully forked & contributed to `toughIQ/ocp-etcd-audit` (PR #2 merged ✅)
- Established Fork → Commit → PR workflow

### Critical Security Rule Established
- **NEVER commit secrets:** API keys, tokens, passwords, .env files
- Git history is permanent — secrets can't be deleted
- Using `.gitignore` as gatekeeper

---

## 2026-02-04 — Hardening & Cleanup

### Resilience: Fallback Chain Refined
- **Observation:** Google-heavy fallback chain is vulnerable if the entire provider has issues.
- **Change:** Moved **Claude Haiku 4.5** (OpenRouter) to position #3 (before Flash).
- **Goal:** Ensure a *provider switch* happens quickly if Google APIs are unstable.

### Cleanup & Localization
- **Nginx Config:** Found German comments in `integrations/knowledge-base/nginx.conf`. Translated to English to match repo language.
- **Roadmap:** Added `TODO.md` to track technical debt and future integrations (like Google Workspace).

### Security Audit
- **Incident:** Found a GitHub PAT in local `.git/config` remote URL.
- **Fix:** Removed token from config. Enforced clean auth via credential helper or env vars.
- **Lesson:** Always audit your local git config (`.git/config`) before assuming you are safe. `.gitignore` doesn't protect local config files!

---

## Template for Future Entries

```markdown
## YYYY-MM-DD — [Title]

### [Topic]
- [What happened]
- [How we solved it]
- [Lesson learned]
```

---

**Last Updated:** 2026-02-03  
**Pages of Molty:** [README](README.md) | [Docs](docs/) | [Integrations](integrations/)
