# ToDo & Roadmap

Open items, ideas, and technical debt we want to address.

## Infrastructure & Resilience
- [ ] **Model Fallback Chain:**
  - Chain has been configured (Gemini -> Claude -> Flash), but needs validation in a real-world scenario (simulation).
  - Goal: Ensure automatic and reliable switching if the primary provider (Google) fails.

## Integrations
- [ ] **Google Workspace (GOG):**
  - Skill `gog` is available system-side (`openclaw/skills/gog`).
  - Needs evaluation, configuration, and testing (Gmail, Calendar, Drive).
  - Goal: Real calendar checks and email summaries directly in chat.
