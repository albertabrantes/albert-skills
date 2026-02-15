# Session Continuation — GHL API Audit Project

**Date:** 2026-02-15
**Previous session:** Built the entire project from scratch
**Next session goal:** Git init, push to GitHub, and review files before handing to OpenClaw

---

## What Was Done

Built a complete OpenClaw-compatible project at:
```
/Users/Shared/1-personal-projects/albert-skills/ghl-api-audit/
```

This project is designed for an OpenClaw bot running GLM-5 to audit our GHL API documentation by comparing it against official docs and making safe read-only API calls.

### Files Created (16 total)

| File | Purpose |
|------|---------|
| `AGENTS.md` | Core agent instructions — OpenClaw loads this every session. Defines the full workflow: read progress.json, pick next task, do docs review + API testing, write report, fix docs, update progress, commit, stop. |
| `SOUL.md` | Agent personality — methodical, precise, cautious. Structured markdown output. Never guesses. |
| `CONTEXT.md` | Session-start orientation — GHL API fundamentals (base URL, auth, version headers, rate limits), knowledge base file inventory, test environment details, known issues being investigated. |
| `SAFETY.md` | Hard safety rules — forbidden operations (no SMS, no calls, no writes), rate limits (5s between calls, max 20/task, max 30/session), API call logging requirements, test location details, emergency procedures. |
| `progress.json` | JSON task tracker — 5 Voice AI tasks, all `pending`. Agent reads this to find next task, updates it when done. |
| `.env` | Real credentials — `pit-d0a4f9a9-7e11-46b2-9ede-b9040b88c424` token, `mlqwsXxwqYU6DDpTKQNG` location ID. **Gitignored.** |
| `.env.example` | Credential template (no real values). |
| `.gitignore` | Excludes `.env`, `.DS_Store`, `node_modules/`. |
| `README.md` | Project overview for humans. |
| `reports/api-call-log.json` | Empty call log — every API call gets logged here with timestamp, status, rate limit info. |
| `templates/report-template.md` | Markdown template for audit reports. |
| `tasks/voice-ai-001-call-logs-list.md` | Task: verify `GET /voice-ai/dashboard/call-logs` — 3 API calls budgeted. |
| `tasks/voice-ai-002-call-log-single.md` | Task: verify `GET /voice-ai/dashboard/call-logs/{callId}` — 3 API calls budgeted. |
| `tasks/voice-ai-003-agent-config.md` | Task: verify `GET/PATCH /voice-ai/agent/{agentId}` — 2 API calls (GET only, PATCH flagged for human review). |
| `tasks/voice-ai-004-conversations.md` | Task: verify `POST /conversations/messages` + `GET /conversations/search` — 2 API calls (search only, SMS flagged for human review). |
| `tasks/voice-ai-005-scopes-and-meta.md` | Task: verify scopes table, timestamps, cross-cutting concerns — 0 API calls (pure docs review). |

### The Doc Being Audited

The voice-ai.md file lives at:
```
/Users/Shared/1-personal-projects/2-areas/ghl-api/areas/ghl/api/voice-ai.md
```

Known issues that triggered this audit:
- **Timestamp bugs** — Unix ms values may not match the dates they claim to represent
- **Scope inconsistencies** — `voice-ai-dashboard.readonly` used in endpoints but missing from the scopes table
- **Missing response schemas** — several endpoints don't show what the API actually returns

---

## What Still Needs To Be Done

### 1. Git init and push to GitHub (BLOCKED in previous session)

Bash tool had persistent stream/permission errors when running git commands on the `/Users/Shared/1-personal-projects/albert-skills/` directory. Git was initialized (`git init -b main`) and files were staged (`git add -A`), but the commit never went through.

**Run these commands manually or in the new session:**

```bash
# Check if the repo is already initialized and staged
git -C /Users/Shared/1-personal-projects/albert-skills status

# If files are staged, commit
git -C /Users/Shared/1-personal-projects/albert-skills commit -m "Initial commit: GHL API audit project for OpenClaw + GLM-5"

# Create GitHub repo and push
gh repo create albertabrantes/albert-skills --public --source=/Users/Shared/1-personal-projects/albert-skills --push --description "AI agent skill projects and automation tasks"
```

If git status shows nothing staged, re-stage:
```bash
git -C /Users/Shared/1-personal-projects/albert-skills add -A
```

### 2. Review all files before handing to OpenClaw

Before the agent starts, review these files for accuracy:

- [ ] **`.env`** — Confirm the token and location ID are correct and have the right scopes
- [ ] **`SAFETY.md`** — Confirm your phone number is correct (+16047277664) and the location ID matches
- [ ] **`CONTEXT.md`** — Read through and confirm GHL API fundamentals are accurate
- [ ] **`AGENTS.md`** — Review the workflow instructions
- [ ] **Task files** — Skim each task to make sure the instructions make sense

### 3. Verify the .env token has required scopes

The token needs these OAuth scopes for the Voice AI audit:
- `voice-ai-dashboard.readonly` — for call logs
- `voice-ai-agents.readonly` — for agent config
- `voice-ai-agents.write` — for PATCH agent (docs review only, won't actually call)
- `conversations.readonly` — for search conversations
- `conversations/message.readonly` — for reading messages
- `conversations/message.write` — for send message (docs review only, won't actually call)

If the Private Integration Token (PIT) doesn't have all these scopes, some tasks will be marked `blocked`.

### 4. Configure OpenClaw to use this workspace

Point OpenClaw at this project directory:
```
/Users/Shared/1-personal-projects/albert-skills/ghl-api-audit/
```

OpenClaw will automatically pick up `AGENTS.md` and `SOUL.md` from the workspace root.

The agent's first session should:
1. Read CONTEXT.md, SAFETY.md, progress.json
2. Pick up `voice-ai-001` (first pending task)
3. Execute it and write a report

### 5. After Phase 1 completes — Evaluate and expand

Once all 5 Voice AI tasks are done:
1. Review all 5 reports in `reports/`
2. Check `api-call-log.json` for any issues
3. Review the fixes made to voice-ai.md
4. If quality is good, create Phase 2 tasks for the next API file (contacts.md has 27 endpoints — will need ~6-8 tasks)

---

## Project Design Decisions (for reference)

**Why JSON for progress tracking:** OpenClaw docs recommend JSON over YAML/Markdown for progress files to avoid LLM-induced corruption during updates.

**Why 5-second delay between API calls:** GHL allows 100 requests/10 seconds. Our 5-second delay is 20x slower, giving massive safety margin. The agent will make at most 10-20 API calls across all 5 tasks.

**Why one task per session:** OpenClaw's recommended two-agent pattern — each session picks one task, completes it fully, commits, and stops. This prevents context window exhaustion and ensures the codebase is always in a clean state.

**Why GET-only by default:** GHL is a production CRM platform. POST/PUT/PATCH/DELETE could send SMS, trigger calls, modify contacts, or delete data. Every write operation is forbidden unless the specific task file explicitly approves it and explains why it's safe.

**Why we flag PATCH and POST as `needs_human_review`:** Even in a test sub-account, PATCH to a voice AI agent could change its behavior for real callers. POST to conversations/messages sends real SMS. These need a human to decide whether testing is safe.

---

## Quick Reference

| Item | Value |
|------|-------|
| Project path | `/Users/Shared/1-personal-projects/albert-skills/ghl-api-audit/` |
| Knowledge base path | `/Users/Shared/1-personal-projects/2-areas/ghl-api/areas/ghl/api/` |
| Doc being audited | `voice-ai.md` |
| GHL API base URL | `https://services.leadconnectorhq.com` |
| GHL Location ID | `mlqwsXxwqYU6DDpTKQNG` |
| GHL Token | In `.env` (gitignored) |
| Voice AI version header | `2021-04-15` |
| Default version header | `2021-07-28` |
| Rate limit | 5 seconds between calls |
| Max calls per task | 20 |
| Total tasks (Phase 1) | 5 |
| GitHub repo | `albertabrantes/albert-skills` (needs to be created) |
