
# Workflow 04 — Copywriter Task Creation

## Overview
This workflow receives an **approved EWS insight** (from Workflow 03), creates a detailed communication task for the copywriter in Asana, notifies the copywriter via Slack, and logs the entire event through the Logging Engine.

It does **not** generate any content by itself.  
It only orchestrates task creation and notification for a fully human-driven communication process.

---

## Trigger

**Webhook — `/create_comm_task`**

- Called by Workflow 03 when a human clicks **“Approve Communication Task”**.
- The incoming payload contains:
  - `insight_summary`
  - `severity`
  - `timestamp`
  - Optional metadata (e.g., `requested_by`, `source_workflow`, `related_links`)

---

## Steps

### 1. Validate EWS Payload

Validates that:
- `insight_summary` exists  
- `severity` exists  
- `timestamp` exists  

Normalizes:
- `requested_by` → defaults to `unknown`  
- `source_workflow` → defaults to `unknown`  

If validation fails, the workflow stops with an error.

---

### 2. Build Asana Task Payload

Creates:
- **Task name**:  
  `EWS Communication: <SEVERITY> incident`

- **Notes** (body of the task), including:
  - Severity  
  - Approved by  
  - Source workflow  
  - Insight summary  
  - Optional related links  

- **Project**: `$env.ASANA_PROJECT_ID_COMM`  
- **Assignee**: `$env.ASANA_COPYWRITER_ASSIGNEE_ID`  
- **Due date**: next calendar day from “now”

This produces an `asana_payload` object to send directly to Asana.

---

### 3. Create Asana Task

- Endpoint: `https://app.asana.com/api/1.0/tasks`
- Method: `POST`
- Auth: Bearer `$env.ASANA_ACCESS_TOKEN`
- Body: `asana_payload`

On success, Asana returns a task `gid`.

---

### 4. Build Slack Notification

Builds a Slack message for the copywriter:

- Includes a direct link to the Asana task:  
  `https://app.asana.com/0/$env.ASANA_PROJECT_ID_COMM/<task_gid>`

- Message text:  
  `"A new EWS-based communication task has been created for you in Asana.
<task_url>"`

---

### 5. Notify Copywriter on Slack

- Endpoint: `https://slack.com/api/chat.postMessage`
- Channel: `$env.SLACK_CHANNEL_COPYWRITER`
- Auth: `$env.SLACK_BOT_TOKEN`
- Text: the message built in the previous step.

---

### 6. Log Task Creation Event

Posts a log entry to:

- `$env.LOGGING_WEBHOOK_URL`

Payload example:

```json
{
  "event_type": "comm_task_created",
  "workflow": "04_Copywriter_Task_Creation",
  "timestamp": "<now>",
  "success": true,
  "severity": "medium",
  "actor": "system",
  "payload": { ... }  // Asana + Slack data
}
```

---

### 7. Respond to Caller

Responds to the original webhook caller (Workflow 03) with:

```json
{"status":"task_created"}
```

This confirms the orchestration has completed.

---

## Environment Variables

| Variable                     | Description                                          |
|-----------------------------|------------------------------------------------------|
| ASANA_ACCESS_TOKEN          | Asana API token                                     |
| ASANA_PROJECT_ID_COMM       | Asana project ID where communication tasks live     |
| ASANA_COPYWRITER_ASSIGNEE_ID| Asana user ID for the copywriter                    |
| SLACK_BOT_TOKEN             | Slack bot token                                     |
| SLACK_CHANNEL_COPYWRITER    | Slack channel (or user) for copywriter notifications|
| LOGGING_WEBHOOK_URL         | Central logging engine endpoint                     |

---

## Safe Modification Guidelines

- Do not remove or bypass the validation step.
- If changing the due date logic, maintain clarity and predictability.
- Always ensure the Asana project and assignee IDs are valid.
- If switching the Slack target (channel vs DM), ensure the bot has permission.
- Any structural change should be reflected in this documentation and in the Logging Engine expectations.

---

End of Document
