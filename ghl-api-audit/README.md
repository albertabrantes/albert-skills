# GHL API Documentation Audit

Automated verification of GoHighLevel API documentation using AI agents (OpenClaw + GLM-5).

## Overview

This project audits our GHL API knowledge base by:
1. Comparing documented endpoints against official GHL API docs
2. Making safe, read-only API calls to verify real behavior
3. Fixing inaccuracies and writing detailed reports
4. Tracking progress through a structured JSON file

## Project Structure

```
ghl-api-audit/
├── AGENTS.md              # Agent instructions (loaded by OpenClaw)
├── SOUL.md                # Agent personality/style
├── CONTEXT.md             # Session-start orientation (GHL fundamentals, known issues)
├── SAFETY.md              # Rate limits, banned operations, test constraints
├── progress.json          # Task tracker (JSON, updated by agent each session)
├── .env                   # API credentials (not committed)
├── .env.example           # Credential template
├── tasks/                 # One file per audit task
│   ├── voice-ai-001-call-logs-list.md
│   ├── voice-ai-002-call-log-single.md
│   ├── voice-ai-003-agent-config.md
│   ├── voice-ai-004-conversations.md
│   └── voice-ai-005-scopes-and-meta.md
├── reports/               # Agent-written audit reports
│   └── api-call-log.json  # Log of every API call made
└── templates/
    └── report-template.md # Template for audit reports
```

## How It Works

1. OpenClaw agent starts a session, reads CONTEXT.md + SAFETY.md + progress.json
2. Agent picks the next `pending` task from progress.json
3. Agent reads the task file for specific instructions
4. Agent performs docs review + safe API testing
5. Agent writes a report, fixes docs if needed, updates progress.json
6. Agent commits and stops — one task per session

## Phases

| Phase | API Domain | Tasks | Status |
|-------|-----------|-------|--------|
| 1 | Voice AI | 5 | Current |
| 2+ | TBD after Phase 1 review | - | Planned |

## Safety

- GET-only API testing (no writes unless explicitly approved per task)
- 5-second delay between API calls
- Max 20 calls per task, 30 per session
- All API calls logged in reports/api-call-log.json
- No outbound communications (SMS, calls, emails) ever

## Knowledge Base Location

The documentation being audited lives at:
```
/Users/Shared/1-personal-projects/2-areas/ghl-api/areas/ghl/api/
```
