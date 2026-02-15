# Task: voice-ai-003 — Verify Agent Config Endpoints (GET + PATCH)

## What to Verify

**Endpoints:**
1. `GET /voice-ai/agent/{agentId}` — Get Agent Config
2. `PATCH /voice-ai/agent/{agentId}` — Update Agent Prompt

**Doc file to audit:** `/Users/Shared/1-personal-projects/2-areas/ghl-api/areas/ghl/api/voice-ai.md`
**Sections:** "Get Agent Config" (lines 199-219) and "Update Agent Prompt" (lines 223-258)
**Official docs:** https://marketplace.gohighlevel.com/docs/

## Verification Checklist

### Step 1: Docs Review

Fetch official GHL API docs. Compare for BOTH endpoints:

**GET /voice-ai/agent/{agentId}:**
- [ ] HTTP method correct (GET)
- [ ] Path correct
- [ ] Scope correct (`voice-ai-agents.readonly`)
- [ ] Version header correct (`2021-04-15`)
- [ ] Response schema — our docs don't have one! Check official docs for schema.

**PATCH /voice-ai/agent/{agentId}:**
- [ ] HTTP method correct (PATCH)
- [ ] Path correct
- [ ] Scope correct (`voice-ai-agents.write`)
- [ ] Version header correct (`2021-04-15`)
- [ ] Request body schema — we only show `{"prompt": "..."}`. Are there other fields?
- [ ] Response schema — not documented at all
- [ ] Propagation delay note (2-3 seconds) — is this in official docs?

### Step 2: Live API Test

**API Version Header:** `Version: 2021-04-15`

**IMPORTANT:** For PATCH testing, we have a special constraint:
- **DO NOT actually patch the agent prompt.** This could break a production Voice AI agent.
- Only test GET. The PATCH endpoint verification is DOCS REVIEW ONLY unless you can confirm the agentId belongs to a test agent with no real callers.

1. **Discover agent IDs:** We need an agentId. Check if there's a list agents endpoint, or look for agentId in call logs from previous tasks.
   - If call logs from voice-ai-001 contain an `agentId` field, use that.
   - Otherwise: `GET /voice-ai/dashboard/call-logs?locationId={GHL_LOCATION_ID}&page=1&pageSize=1` to extract an agentId.

2. **Get agent config:** `GET /voice-ai/agent/{agentId}`
   - Document: full response schema (every field)
   - Check: does it return the prompt text? What other config fields exist?

3. **DO NOT test PATCH.** Mark PATCH testing as `needs_human_review` in the report.

**Max API calls for this task: 2**
**Wait 5 seconds between each call.**

### Step 3: Report

Write findings to `reports/voice-ai-003-agent-config-report.md`.

Key things to document:
- Full GET response schema (currently completely missing)
- All configurable fields on the agent object
- Whether PATCH accepts fields beyond `prompt`
- Any discrepancies between official docs and our docs

### Step 4: Fix

Update voice-ai.md with:
- Full response schema for GET agent config
- Any additional PATCH fields if official docs show them
- Correct any wrong scopes or paths

### Step 5: Update Progress

Update `progress.json`: set `voice-ai-003` to `completed`.
