# Task: voice-ai-005 — Verify Scopes Reference & Cross-Cutting Concerns

## What to Verify

**Doc file to audit:** `/Users/Shared/1-personal-projects/2-areas/ghl-api/areas/ghl/api/voice-ai.md`
**Sections:** "API Scopes Reference" (lines 354-366), file header (lines 1-8), and all "Important Notes" sections throughout
**Official docs:** https://marketplace.gohighlevel.com/docs/

## Verification Checklist

### Step 1: Docs Review

This task is primarily docs-review only. Verify:

**File Header:**
- [ ] Base URL correct (`https://services.leadconnectorhq.com`)
- [ ] Version header correct (`Version: 2021-04-15`)
- [ ] Required scopes listed match what each endpoint actually needs

**Scopes Table:**
- [ ] `voice-ai-agents.readonly` — exists in official docs, correct description
- [ ] `voice-ai-agents.write` — exists in official docs, correct description
- [ ] `voice-ai-agent-goals.readonly` — exists in official docs (we don't have endpoints for goals — should we note this?)
- [ ] `voice-ai-agent-goals.write` — exists in official docs
- [ ] `voice-ai-dashboard.readonly` — NOT in our scopes table but IS referenced in the List Call Logs endpoint. **This is likely a bug in our docs.** Verify and fix.
- [ ] `conversations.readonly` — correct scope name and description
- [ ] `conversations.write` — correct scope name and description
- [ ] `conversations/message.readonly` — correct (note the slash, not dot)
- [ ] `conversations/message.write` — correct (note the slash, not dot)

**Cross-Cutting Concerns:**
- [ ] All "Common Mistakes" and "Important Notes" sections — are they accurate?
- [ ] The warning about `GET /conversations/messages?type=VOICE` being wrong — verify this is still true
- [ ] The `pageSize` vs `limit` gotcha — confirmed in task voice-ai-001
- [ ] The `callLogs` vs `data` response field — confirmed in task voice-ai-001
- [ ] Timestamp examples in "Date Parameter Reference" — are the Unix ms values correct for the dates shown?

**Timestamp Verification (this was a known issue):**
- `1679308800000` should be "2023-03-20T00:00:00Z" — verify with: `new Date(1679308800000).toISOString()`
- `1679395199000` should be "2023-03-20T23:59:59Z" — verify
- `1771027200000` should be "2026-02-14T00:00:00Z" — verify
- `1771113600000` should be "2026-02-15T00:00:00Z" — verify
- If ANY of these are wrong, that's the timestamp bug the project owner mentioned. Fix them.

### Step 2: Live API Test

**No API calls needed for this task.** This is a pure documentation review.

If previous task reports are available in `reports/`, cross-reference their findings.

**Max API calls for this task: 0**

### Step 3: Report

Write findings to `reports/voice-ai-005-scopes-meta-report.md`.

Key things to document:
- Any missing scopes in the scopes table
- Timestamp accuracy (the known bug)
- Any inconsistencies between endpoint-level scope docs and the scopes table
- Whether we should document Voice AI Goals endpoints (if they exist)

### Step 4: Fix

Update voice-ai.md with:
- Corrected scopes table (add `voice-ai-dashboard.readonly` if missing)
- Fixed timestamps if wrong
- Any other corrections found

### Step 5: Update Progress

Update `progress.json`: set `voice-ai-005` to `completed`.
