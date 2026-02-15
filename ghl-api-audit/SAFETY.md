# SAFETY RULES - READ THIS EVERY SESSION

**Last Updated:** 2026-02-15

## NEVER DO THESE THINGS

These actions are **absolutely forbidden**. If you are about to do any of these, STOP immediately.

| Forbidden Action | Why |
|------------------|-----|
| Send SMS or messages (`POST /conversations/messages`) | Contacts real people |
| Trigger Voice AI calls | Calls real phone numbers |
| Create/update contacts with real phone numbers | Could trigger automations |
| Trigger any workflow | Workflows may send emails/SMS/calls |
| Add contacts to campaigns | Campaigns send automated messages |
| Delete any production data | Irreversible |
| Make POST/PUT/PATCH/DELETE requests unless the task EXPLICITLY says it's safe | Could modify production data |

## SAFE OPERATIONS (GET requests only by default)

These are the only operations you may perform without explicit task approval:

- `GET` requests to read data (contacts, call logs, calendars, etc.)
- Reading response headers for rate limit info
- Fetching official documentation via web

## Rate Limits

**Official GHL API Rate Limits:**

| Limit Type | Value | Source |
|------------|-------|--------|
| Burst Limit | 100 requests / 10 seconds | Per app, per location |
| Daily Limit | 200,000 requests / day | Per app, per location |
| On 429 | Stop immediately, read `Retry-After` header | - |

**Our Self-Imposed Limits (much stricter):**

| Our Rule | Value | Why |
|----------|-------|-----|
| Min delay between calls | **5 seconds** | 20x slower than burst limit for safety |
| Max calls per task | **20** | No task should need more than this |
| Max calls per session | **30** | Hard ceiling including retries |
| On ANY error | Log and stop testing | Don't retry blindly |

## API Call Logging

**Every single API call must be logged** in `reports/api-call-log.json` before the call is made.

Log format:
```json
{
  "calls": [
    {
      "id": "call-001",
      "task_id": "voice-ai-001",
      "timestamp": "2026-02-15T10:30:00Z",
      "method": "GET",
      "url": "/voice-ai/dashboard/call-logs?locationId=xxx&page=1&pageSize=5",
      "status_code": 200,
      "response_time_ms": 450,
      "rate_limit_remaining": 95,
      "rate_limit_daily_remaining": 199950,
      "notes": "Verified call logs endpoint returns callLogs array"
    }
  ]
}
```

## Test Location

- **Location ID:** `mlqwsXxwqYU6DDpTKQNG` (test sub-account)
- **Token type:** Private Integration Token (PIT)
- **Owner contact (DO NOT CALL/SMS):** +16047277664

## If Something Goes Wrong

1. **Stop all API calls immediately**
2. Log what happened in the report
3. Mark the task as `blocked` in progress.json
4. Add a clear note explaining what went wrong
5. Do NOT try to fix it by making more API calls
