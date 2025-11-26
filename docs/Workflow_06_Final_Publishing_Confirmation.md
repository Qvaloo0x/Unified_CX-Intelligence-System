
# Workflow 06 — Final Publishing Confirmation (Manual Only)

## Overview
This workflow is intentionally simple.  
Publishing is 100% manual — done by the Content Lead.

When the Content Lead manually updates the Asana task with:

```
publish_status = "published"
```

the workflow:

1. Validates the Asana webhook payload  
2. Confirms publication was marked manually  
3. Notifies leadership in Slack  
4. Logs the manual publication event  
5. Responds to Asana  

There is **no auto-publishing**, **no scheduling**, and **no automation beyond logging**.

---

## Trigger
### Webhook — `/publish_confirmation`

Expected payload:
```json
{
  "task_gid": "...",
  "publish_status": "published"
}
```

---

## Steps

### 1. Validate Payload
Ensures:
- `task_gid` exists  
- `publish_status` exists  

If invalid → workflow stops.

---

### 2. Check for Manual Publication
Switch node checks:
```
publish_status == "published"
```

If not true → workflow exits silently.

---

### 3. Notify Leadership (Slack)
Message:
- Confirms the Content Lead manually published the communication  
- Reinforces human control  
- Provides visibility to leadership  

Sent to:
```
$env.SLACK_CHANNEL_PUBLISH
```

---

### 4. Log Event
Logs:
```json
{
  "event_type": "manual_publication_confirmed",
  "actor": "human",
  "severity": "low"
}
```

This preserves a full audit trail.

---

### 5. Respond
Response:
```json
{"status":"manual_publication_confirmed"}
```

---

## Environment Variables

| Variable | Description |
|---------|-------------|
| SLACK_BOT_TOKEN | Slack authentication |
| SLACK_CHANNEL_PUBLISH | Channel for publication confirmations |
| LOGGING_WEBHOOK_URL | Logging Engine endpoint |

---

## Safe Modification Guidelines
- Never automate publication.  
- Keep manual confirmation as the only trigger for this workflow.  
- Ensure the Asana custom field is controlled exclusively by humans.  
- Maintain this workflow as a low-severity, low-risk confirmation step.

---

End of Document
