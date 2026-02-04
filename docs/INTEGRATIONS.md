# Integrations Guide

## Google Workspace (via `gog`)

Molty uses [gogcli](https://github.com/steipete/gogcli) to interact with Google Services.

### Setup
1. **Credentials:** `client_secret.json` (Desktop App) required.
2. **Auth:** Run `gog auth add <account-email> --services gmail,calendar,drive...`
3. **Headless Auth:**
   - Copy the link generated in terminal.
   - Open in browser on local machine.
   - Copy the failed redirect URL (`http://127.0.0.1:xxxxx/...`).
   - Curl or paste that URL back to the agent.

### Capabilities & Limitations

| Service | Read | Create | Update Content | Note |
|:--------|:----:|:------:|:--------------:|:-----|
| **Gmail** | ✅ | ✅ | N/A | Use `--body-file` or heredoc for multiline emails. |
| **Calendar**| ✅ | ✅ | ✅ | Cannot create *new* calendars, only events. |
| **Drive** | ✅ | ✅ | ✅ | Search, move, rename working fine. |
| **Sheets** | ✅ | ✅ | ✅ | Full cell update support. |
| **Docs** | ✅ | ✅ | ❌ | **Read-only content.** Can create empty docs only. |
| **Slides** | ✅ | ✅ | ❌ | **Read-only content.** Can create empty decks only. |

### Calendar Color Coding
Since we use a single calendar, we distinguish event types by Color ID:

- 🔴 **Work** (`--event-color 11`): Red Hat, Customers, Business.
- 🟢 **Private** (`--event-color 10`): Family, Personal, Leisure.
- 🔵 **Molty** (`--event-color 9`): Maintenance, Updates, Internal Tasks.

### Common Commands

**Send Email (Multiline):**
```bash
gog gmail send --to "recipient@example.com" --subject "Hello" --body-file - <<EOF
Line 1
Line 2
EOF
```

**Create Work Event:**
```bash
gog calendar create <email> --summary "Meeting" --from <ISO> --to <ISO> --event-color 11
```

**Update Sheet Cell:**
```bash
gog sheets update <sheetId> "Sheet1!A1" "New Value"
```
