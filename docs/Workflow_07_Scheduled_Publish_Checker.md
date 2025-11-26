
# Workflow 07 — Scheduled Publish Checker

## Overview
Checks Asana tasks that have a **scheduled publishing date**.  
If any task is due today, it notifies the Content Lead and logs the event.

---

## Trigger
### Daily at 06:00  
Ensures tasks scheduled for publication today are flagged early.

---

## Steps

### 1. Fetch Scheduled Tasks
Pulls all tasks from:
```
ASANA_PROJECT_ID_COMM
```

### 2. Filter Tasks Due Today
Compares task.due_on with today's date (ISO format).

### 3. Notify Content Lead
Sends an alert to:
```
SLACK_CHANNEL_PUBLISH
```

### 4. Log Event
Logs:
```
event_type: scheduled_publish_check
actor: system
success: true
```

---

## Environment Variables

| Variable | Description |
|---------|-------------|
| ASANA_ACCESS_TOKEN | Authentication for Asana |
| ASANA_PROJECT_ID_COMM | Project containing communication tasks |
| SLACK_BOT_TOKEN | Slack auth |
| SLACK_CHANNEL_PUBLISH | Channel to notify publishing events |
| LOGGING_WEBHOOK_URL | Logging engine endpoint |

---

## Safe Modification Guidelines
- Keep time trigger early in the day.
- Ensure due_on is managed by Content Lead or editor.
- Combine with Workflow 08 for more precise scheduled publishing flows.

