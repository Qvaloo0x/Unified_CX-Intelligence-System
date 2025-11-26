
# Workflow 08 — Schedule Metadata Setter

## Overview
This workflow allows the Content Lead (or another authorized workflow) to set or update the **scheduled publish date** of a communication task in Asana.  
It also updates the `publish_status` to `"scheduled"`, notifies leadership in Slack, and logs the operation.

This supports Workflow 07 (Scheduled Publish Checker) and maintains consistency across the publishing lifecycle.

---

## Trigger
### Webhook — `/set_scheduled_metadata`

Payload example:
```json
{
  "task_gid": "123456789",
  "scheduled_date": "2025-01-25"
}
```

---

## Steps

### 1. Validate Payload
Ensures:
- `task_gid` is present  
- `scheduled_date` is present and ISO formatted (`YYYY-MM-DD`)

If invalid → workflow stops.

---

### 2. Update Scheduled Publish Date in Asana
Updates:
```
task.due_on = scheduled_date
custom_fields.publish_status = "scheduled"
```

This ensures:
- Workflow 07 can detect when the scheduled date arrives  
- Workflow 06 will only run when manually marked "published"  

---

### 3. Notify Leadership in Slack
Sends a message to:
```
$env.SLACK_CHANNEL_PUBLISH
```

Message confirms:
- A scheduled date was set  
- A task is now awaiting publishing day  

---

### 4. Log Event
Logs to the central Logging Engine:
```
event_type: scheduled_publish_set
severity: low
actor: human
```

---

### 5. Respond
Returns:
```json
{"status":"scheduled_metadata_set"}
```

---

## Environment Variables

| Variable | Description |
|---------|-------------|
| ASANA_ACCESS_TOKEN | Needed to update tasks |
| SLACK_BOT_TOKEN | Slack notification |
| SLACK_CHANNEL_PUBLISH | Channel for scheduling notifications |
| LOGGING_WEBHOOK_URL | Logging Engine endpoint |

---

## Safe Modification Guidelines
- Keep `publish_status = "scheduled"` to maintain compatibility with Workflow 07.
- Always validate the scheduled date format.
- If adding additional metadata fields, document them.
- Do not allow auto-publishing — it breaks human control and safety.

---

End of Document
