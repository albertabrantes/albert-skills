# Task: voice-ai-004 — Verify Conversations Endpoints (SMS + Search)

## What to Verify

**Endpoints:**
1. `POST /conversations/messages` — Send Message (SMS)
2. `GET /conversations/search` — Search Conversations

**Doc file to audit:** `/Users/Shared/1-personal-projects/2-areas/ghl-api/areas/ghl/api/voice-ai.md`
**Sections:** "Send Message (SMS)" (lines 264-298) and "Search Conversations" (lines 302-350)
**Official docs:** https://marketplace.gohighlevel.com/docs/

## Verification Checklist

### Step 1: Docs Review

Fetch official GHL API docs. Compare for BOTH endpoints:

**POST /conversations/messages:**
- [ ] HTTP method correct (POST)
- [ ] Path correct
- [ ] Scope correct (`conversations/message.write`)
- [ ] Version header correct (`2021-04-15`)
- [ ] Request body schema — we show `type`, `contactId`, `message`. Are there more fields?
- [ ] Response schema — not documented at all. Check official docs.
- [ ] Are there other message types beyond SMS? (e.g., Email, WhatsApp)

**GET /conversations/search:**
- [ ] HTTP method correct (GET)
- [ ] Path correct
- [ ] Scope correct (`conversations.readonly`)
- [ ] Version header — we say `2021-07-28` (different from other Voice AI endpoints). Confirm this.
- [ ] Query parameters — all names, types, required/optional
- [ ] `lastMessageType` values — we list `TYPE_SMS`, `TYPE_CALL`, `TYPE_EMAIL`, `TYPE_VOICEMAIL`. Verify these.
- [ ] Response schema — not documented. Check official docs.
- [ ] Pagination mechanism — we mention `startAfterDate` as cursor. Confirm.

### Step 2: Live API Test

**CRITICAL SAFETY NOTE:**
- **DO NOT test `POST /conversations/messages`.** This sends real SMS to real people. Mark as `needs_human_review`.
- Only test `GET /conversations/search` (read-only, safe).

**API Version Header:** `Version: 2021-07-28` (for search endpoint)

1. **Search conversations:** `GET /conversations/search?locationId={GHL_LOCATION_ID}&limit=5`
   - Verify: returns 200, document response structure
   - Check: field names, pagination fields

2. **Search with type filter:** `GET /conversations/search?locationId={GHL_LOCATION_ID}&lastMessageType=TYPE_CALL&limit=5`
   - Verify: type filter works, results are voice calls
   - Document: actual response schema

3. **DO NOT test POST /conversations/messages.** Mark as `needs_human_review`.

**Max API calls for this task: 2**
**Wait 5 seconds between each call.**

### Step 3: Report

Write findings to `reports/voice-ai-004-conversations-report.md`.

Key things to document:
- Full response schema for search (currently missing)
- All message type values confirmed
- Whether version header difference is correct (2021-07-28 vs 2021-04-15)
- POST /conversations/messages — docs-only review findings

### Step 4: Fix

Update voice-ai.md with:
- Response schemas for both endpoints
- Any corrected parameters or types
- Any missing fields or options

### Step 5: Update Progress

Update `progress.json`: set `voice-ai-004` to `completed`.
