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

## 2026-02-04 — The Google Integration Saga

### Google Workspace Integration (GOG)
- **Goal:** Enable Molty to manage Calendar, Gmail, and Drive.
- **Tool:** `gogcli` (Go Google CLI).
- **Challenge:** OAuth flow on a headless server.
- **Solution:** Manually pasted the localhost redirect URL from Chris's browser back to Molty's terminal. *Hackerman mode activated.* 😎

### Gmail Mastery
- Successfully connected to `molty.toughiq@gmail.com`.
- **Capabilities:**
  - Reading emails (and unreading/labeling them).
  - Sending emails (Success!).
  - **Lesson Learned:** CLI arguments need careful escaping. My first email had literal `\n` characters instead of newlines. *Awkward.* Fixed by using proper heredoc syntax or file inputs.

### Calendar Strategy
- **Constraint:** `gogcli` cannot create *new* calendars via API, only events.
- **Solution:** Used **Color IDs** to categorize events within the primary calendar.
  - 🔴 **Work (ID 11):** Official business.
  - 🟢 **Private (ID 10):** Chris's personal schedule.
  - 🔵 **Molty (ID 9):** Internal maintenance & tasks.
- **Result:** Successfully distinguished between work, private, and internal events.

### The Office Suite Limitation
- **Success:** Can create Google Docs, Sheets, and Slides.
- **Limitation:**
  - **Sheets:** Fully editable (update cells, append rows). ✅
  - **Docs & Slides:** **Read-Only for content.** Can create empty files, rename them, move them, but *cannot write text into them* via CLI.
- **Verdict:** Great for scaffolding/organizing, but I can't write your essays or build your slide decks (yet).

---

## 2026-02-05 — Resilience & Optimization

### The Great Fallback Fail
- **Incident:** Google API Quota hit hard limit (`429 Daily Limit Exceeded`).
- **Failure:** The fallback chain was `Google -> Google -> Google`. When Google locked the account, *all* models failed simultaneously.
- **Fix:** Reconfigured `openclaw.json` to insert **OpenRouter (Claude Haiku)** at Position #2.
- **Result:** True multi-provider resilience. If Google dies, we switch to Anthropic immediately.

### Optimization: English Migration
- **Observation:** German text consumes significantly more tokens (2-3x) than English due to tokenizer efficiency.
- **Action:**
  - Migrated `MEMORY.md`, `USER.md`, and `SOUL.md` to English.
  - Switched default agent language to English.
  - Enforced "No Fluff" policy in system prompts.
- **Goal:** Lower costs, faster inference, larger effective context window.

### Documentation
- Added `docs/STRATEGY.md` to document these architectural decisions.

### Tooling Upgrade: GitHub CLI (`gh`)
- **Problem:** Managing PRs via `curl` and raw Git commands is error-prone and requires manual token handling in scripts.
- **Solution:** Installed `gh` CLI via Homebrew.
- **Hiccup:** Authentication failed initially because the PAT lacked `read:org` scope.
- **Fix:** Chris updated the token scopes. Re-authentication succeeded.
- **Outcome:** Native GitHub management capabilities (PRs, Issues, Forking) enabled.

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
