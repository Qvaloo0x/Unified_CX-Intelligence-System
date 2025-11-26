# Workflow 00 — System Health Check  
Daily functional verification of all critical dependencies.

## Overview
This workflow ensures that the Unified CX Intelligence System is operational. It performs real functional tests across all external dependencies:
- OpenAI API  
- Slack API  
- Asana API (create and delete a test task)  
- Google Sheets API (append a test row)  
- The central Logging Engine  

All tests run regardless of earlier failures.

## Trigger
**Cron — Daily @ 06:00 UTC**  
Can be adjusted based on operational requirements.

## Functional Tests Performed

### 1. OpenAI Test
Sends a simple “ping” prompt using Chat Completions.

### 2. Slack Test
Posts:  
“Daily system health check: Slack connectivity test (ping).”  
to **#system-alerts**.

### 3. Asana Functional Test
- Creates a task "system_healthcheck_test"  
- Verifies project permissions  
- Deletes the task

### 4. Google Sheets Test
Appends one row:  
`timestamp | system | healthcheck_ping | Google Sheets connectivity OK`

### 5. Logging Engine Test
Sends a structured diagnostic payload to `$env.LOGGING_WEBHOOK_URL`.

## Error Handling
- All tests use `continueOnFail: true`
- A Switch node detects any failures
- If failures exist:
  - Slack alert sent  
  - Log is stored via Logging Engine  

## Timeout Logic
- HTTP requests: **30s**
- Expected total runtime: < 15 seconds

## Retry Logic
Recommended:
```
Attempt 1 → wait 1s  
Attempt 2 → wait 5s  
Attempt 3 → wait 30s  
```

## Environment Variables
| Variable | Description |
|---------|-------------|
| OPENAI_API_KEY | OpenAI authentication |
| SLACK_BOT_TOKEN | Slack bot token |
| ASANA_ACCESS_TOKEN | Asana access token |
| ASANA_PROJECT_ID_HEALTHCHECK | Project for healthcheck task |
| GS_HEALTHCHECK_SHEET_ID | Google Sheet ID used for ping row |
| GOOGLE_CREDENTIALS_ID | Google API credentials |
| LOGGING_WEBHOOK_URL | Central logging webhook |

## Safe Modification Guidelines
- Keep `continueOnFail` enabled  
- Ensure Router node remains in place  
- Update Slack/Asana targets only if necessary  
- Confirm write permissions before changing sheets  

## Success Criteria
The workflow is healthy if:
- All APIs respond OR  
- Failures are properly logged + alerted  
