# Task: voice-ai-002 — Verify Get Single Call Log Endpoint

## What to Verify

**Endpoint:** `GET /voice-ai/dashboard/call-logs/{callId}`
**Doc file to audit:** `/Users/Shared/1-personal-projects/2-areas/ghl-api/areas/ghl/api/voice-ai.md`
**Section:** "Get Call Log" (lines 154-194 approximately)
**Official docs:** https://marketplace.gohighlevel.com/docs/

## Verification Checklist

### Step 1: Docs Review

Fetch the official GHL API documentation. Compare:

- [ ] HTTP method is correct (GET)
- [ ] Path is correct (`/voice-ai/dashboard/call-logs/{callId}`)
- [ ] Required scope is correct (`voice-ai-agents.readonly`)
- [ ] Version header is correct (`2021-04-15`)
- [ ] Path parameter `callId` — confirm name and type
- [ ] Response schema — our docs say "Returns the full call log object including transcript, summary, duration, contact info, and metadata" but don't show the actual schema. Check if we should add it.
- [ ] Confirm scope difference: List uses `voice-ai-dashboard.readonly` but single Get uses `voice-ai-agents.readonly` — is this actually correct?

### Step 2: Live API Test

**API Version Header:** `Version: 2021-04-15`

**Pre-requisite:** You need a real callId. Get one from the task voice-ai-001 results, OR make a quick call to List Call Logs first:

1. **Get a callId:** `GET /voice-ai/dashboard/call-logs?locationId={GHL_LOCATION_ID}&page=1&pageSize=1`
   - Extract the `id` or `callId` field from the first result

2. **Get single call:** `GET /voice-ai/dashboard/call-logs/{callId}`
   - Verify: returns 200 with full call log object
   - Document: all fields in the response (we need to add the full response schema)
   - Check: does it require `locationId` as a query param too, or just the path param?

3. **Invalid callId:** `GET /voice-ai/dashboard/call-logs/nonexistent-id-12345`
   - Verify: returns 404 or null (our docs say "Returns null/404")
   - Document: exact error response format

**Max API calls for this task: 3**
**Wait 5 seconds between each call.**

### Step 3: Report

Write findings to `reports/voice-ai-002-call-log-single-report.md`.

Key things to document:
- The full response schema (currently missing from our docs)
- Whether `locationId` query param is needed
- The actual scope required (verify the scope discrepancy)
- Error response format for invalid IDs

### Step 4: Fix

Update voice-ai.md with:
- Full response schema for this endpoint (if we can document it)
- Corrected scope if wrong
- Any missing query parameters
- Error response format

### Step 5: Update Progress

Update `progress.json`: set `voice-ai-002` to `completed`.
