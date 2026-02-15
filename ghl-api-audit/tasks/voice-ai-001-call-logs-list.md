# Task: voice-ai-001 — Verify List Call Logs Endpoint

## What to Verify

**Endpoint:** `GET /voice-ai/dashboard/call-logs`
**Doc file to audit:** `/Users/Shared/1-personal-projects/2-areas/ghl-api/areas/ghl/api/voice-ai.md`
**Section:** "List Call Logs" (lines 11-151 approximately)
**Official docs:** https://marketplace.gohighlevel.com/docs/

## Verification Checklist

### Step 1: Docs Review

Fetch the official GHL API documentation for Voice AI endpoints. Compare against our docs for:

- [ ] HTTP method is correct (GET)
- [ ] Path is correct (`/voice-ai/dashboard/call-logs`)
- [ ] Required scope is correct (`voice-ai-dashboard.readonly`)
- [ ] Version header is correct (`2021-04-15`)
- [ ] All query parameters listed (names, types, required/optional)
- [ ] Parameter `pageSize` vs `limit` — confirm which is correct
- [ ] `startDate` and `endDate` types — confirm they are Unix ms timestamps (number), not strings
- [ ] `timezone` parameter exists and is IANA format
- [ ] `sortBy` and `sort` values are correct
- [ ] Response field name is `callLogs` (not `data`)
- [ ] Pagination fields: `total`, `page`, `pageSize`
- [ ] Response object schema — all fields documented match official docs

### Step 2: Live API Test

**API Version Header:** `Version: 2021-04-15`

Make these GET requests (safe, read-only):

1. **Basic call:** `GET /voice-ai/dashboard/call-logs?locationId={GHL_LOCATION_ID}&page=1&pageSize=5`
   - Verify: returns 200, response has `callLogs` array, has `total` field
   - Check: actual field names in response objects

2. **With date filter:** `GET /voice-ai/dashboard/call-logs?locationId={GHL_LOCATION_ID}&startDate={yesterday_ms}&endDate={today_ms}&page=1&pageSize=5`
   - Verify: date filtering works with Unix ms timestamps
   - Check: does filtering actually filter (fewer results than unfiltered)?

3. **Invalid param test:** `GET /voice-ai/dashboard/call-logs?locationId={GHL_LOCATION_ID}&limit=5`
   - Verify: using `limit` instead of `pageSize` returns 422 error
   - This confirms the documented gotcha

**Max API calls for this task: 3**
**Wait 5 seconds between each call.**

### Step 3: Report

Write findings to `reports/voice-ai-001-call-logs-list-report.md` using the report template.

### Step 4: Fix

If any discrepancies found, edit the voice-ai.md file at the path above.

Focus on:
- Incorrect parameter names or types
- Wrong response field names
- Missing or incorrect scope
- Incorrect version header
- Wrong example timestamps (the doc has examples from 2023 — check if they're just examples or if they're misleading)

### Step 5: Update Progress

Update `progress.json`: set `voice-ai-001` to `completed`.
