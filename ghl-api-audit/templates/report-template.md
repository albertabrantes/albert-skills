# Audit Report: {task-id}

**Task:** {task name}
**Date:** {YYYY-MM-DD}
**Endpoint(s):** {METHOD /path}
**Doc File:** {path to .md file}

---

## Summary

| Category | Count |
|----------|-------|
| Critical Issues | 0 |
| Major Issues | 0 |
| Minor Issues | 0 |
| Info/Suggestions | 0 |

**Overall Status:** {PASS / FAIL / PARTIAL}

---

## Docs Review Findings

### Checklist Results

| Check | Status | Notes |
|-------|--------|-------|
| HTTP method correct | PASS/FAIL | |
| Path correct | PASS/FAIL | |
| Scope correct | PASS/FAIL | |
| Version header correct | PASS/FAIL | |
| Parameters complete | PASS/FAIL | |
| Response schema correct | PASS/FAIL | |

### Discrepancies Found

{List each discrepancy with:}
- **What our docs say:** ...
- **What official docs say:** ...
- **Severity:** critical / major / minor
- **Fix:** describe the fix applied (or "needs_human_review")

---

## Live API Test Results

### Test 1: {description}

**Request:**
```
{METHOD} {URL}
Headers: ...
```

**Response:**
```json
{abbreviated response}
```

**Status Code:** {200/404/422/etc}
**Findings:** {what we learned}

---

## Changes Made

| File | Change Description |
|------|-------------------|
| {path} | {what was changed} |

---

## Items Needing Human Review

- {List anything flagged as needs_human_review}

---

## API Calls Used

| # | Endpoint | Status | Rate Limit Remaining |
|---|----------|--------|---------------------|
| 1 | {path} | {code} | {remaining} |
