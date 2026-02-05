# Strategy & Architecture

## Resilience: The Multi-Provider Fallback

To ensure Molty is always available, we use a robust fallback chain that mixes providers. Relying on a single provider (e.g., Google Direct) risks a complete outage if the account hits a quota limit.

**Current Fallback Chain:**
1.  **Google Gemini 3 Pro (Preview):** High intelligence, primary target.
2.  **OpenRouter (Claude Haiku 4.5):** *Hard Break.* Immediate switch to a paid, external provider to bypass Google quotas.
3.  **Google Gemini 2.5 Pro:** Fallback within Google ecosystem.
4.  **Google Gemini 2.5 Flash:** Fast, cheap, high-quota fallback.

**Logic:**
If Google returns `429 Quota Exceeded`, OpenClaw cools down the *entire* Google provider. By placing OpenRouter at position #2, we ensure continuity even during a total Google outage.

## Cost & Token Optimization

### The "English First" Rule
LLMs tokenize English text much more efficiently than German.
- **English:** ~1 token per word.
- **German:** ~2-4 tokens per word (complex grammar, compound words).

**Decision:**
- Molty speaks English by default.
- All internal memory files (`MEMORY.md`, `SOUL.md`, `USER.md`) are stored in English to save tokens on every context load.
- Result: Reduced context window usage and lower inference costs.

### Budget
- **Target:** ~$50/month.
- **Monitoring:** Daily cost checks via cron.
- **Alerts:** Immediate notification if daily usage spikes > $2.00.

## Update Policy
- **Auto-Update:** Disabled.
- **Process:**
  1. Molty checks for updates.
  2. Molty requests permission.
  3. Chris approves.
  4. Molty executes update (requires `sudo` for global npm packages).
