# Project Context — Read This First Every Session

## What This Project Is

This is a **GHL API documentation audit**. We maintain a knowledge base of GoHighLevel API endpoint documentation. Your job is to verify that documentation is accurate by:

1. Comparing our docs against the official GHL API documentation
2. Making safe, read-only API calls to confirm real behavior
3. Fixing any inaccuracies you find
4. Writing detailed reports of your findings

## The Knowledge Base

Our API documentation lives at:
```
/Users/Shared/1-personal-projects/2-areas/ghl-api/areas/ghl/api/
```

Files in that directory:
| File | API Domain | Endpoints |
|------|-----------|-----------|
| `voice-ai.md` | Voice AI + Conversations | 6 |
| `contacts.md` | Contacts | 27 |
| `opportunities.md` | Opportunities & Pipelines | 11 |
| `custom-fields.md` | Custom Fields (Legacy + V2) | 14 |
| `custom-values.md` | Custom Values | 5 |
| `calendars.md` | Calendars (all sections) | 34 |

Master index: `/Users/Shared/1-personal-projects/2-areas/ghl-api/areas/ghl/index.md`

## GHL API Fundamentals

**Base URL:** `https://services.leadconnectorhq.com`

**Authentication:**
```
Authorization: Bearer {token from .env GHL_API_TOKEN}
```

**Version Headers:**
- Most endpoints: `Version: 2021-07-28`
- Voice AI endpoints: `Version: 2021-04-15`
- Conversations Messages: `Version: 2021-04-15`

**Rate Limits:**
- Burst: 100 requests / 10 seconds (per app, per location)
- Daily: 200,000 requests / day (per app, per location)
- **Our self-imposed limit: 5 seconds between calls, max 20 per task**

**Response Headers to Watch:**
- `X-RateLimit-Remaining` — burst requests remaining
- `X-RateLimit-Daily-Remaining` — daily requests remaining
- On 429: stop immediately, log the error

## Test Environment

- **Location ID:** Read from `.env` file (`GHL_LOCATION_ID`)
- **Token:** Read from `.env` file (`GHL_API_TOKEN`) — Private Integration Token (PIT)
- **This is a TEST sub-account.** Still treat it carefully — no writes without explicit task approval.

## Official Documentation

- GHL API Docs: https://marketplace.gohighlevel.com/docs/
- OAuth Scopes: https://marketplace.gohighlevel.com/docs/Authorization/Scopes/index.html
- Developer Portal: https://marketplace.gohighlevel.com/

## Your Workflow Each Session

**The full session workflow is defined in `AGENTS.md`.** Follow it exactly. The summary:

**Startup (8 mandatory steps — do not skip any):**
1. Read `.env` — load credentials
2. Read `SAFETY.md` — refresh on forbidden actions and rate limits
3. Read `CONTEXT.md` (this file) — project context and known issues
4. Read `progress.json` — find the next `pending` task
5. Read the task file from `tasks/`
6. Read the documentation file being audited (path is in the task file)
7. Read `templates/report-template.md` — your report must use this format
8. Check `reports/` for previous task reports (may contain values you need)

**Work (5 phases):**
1. Official docs review — compare our docs against GHL official docs
2. Live API testing — make the GET calls specified in the task
3. Write report — using the template, to `reports/`
4. Fix documentation — correct critical/major issues in the doc file
5. Update progress + git commit

**Then STOP.** One task per session.

## Known Issues Being Investigated

- **Timestamp accuracy:** Some Unix ms timestamps in voice-ai.md may not match the dates they claim to represent. This was the original trigger for this audit.
- **Scope inconsistencies:** The List Call Logs endpoint may use `voice-ai-dashboard.readonly` but the scopes table at the bottom of voice-ai.md doesn't list it.
- **Missing response schemas:** Several endpoints in voice-ai.md don't have response schemas documented.

## Current Audit Phase

**Phase 1: Voice AI** (5 tasks)
- We are auditing voice-ai.md first as a pilot
- After Phase 1 completes and results are reviewed by a human, more phases will be added for other API files
