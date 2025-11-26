
# Workflow 03 — EWS Alert + Human Decision

## Purpose
This workflow receives **Early Warning System (EWS)** insights and requests **human decision-making** from the Content Lead and CMO.

It ensures:
- No automated communication is created without review.
- Humans approve or reject each EWS-triggered opportunity.
- Activity is logged for full auditability.

---

## Trigger
### **Webhook — `/ews_alert`**
Called by Workflow 02 (Diagnostics) whenever an insight meets EWS threshold.

---

## Steps

### **1. Validate Insight**
Checks required fields:
- insight_summary
- severity
- source_workflow
- timestamp

If any field is missing → workflow fails immediately.

---

### **2. Send Slack Message With Buttons**
Message goes to:
```
$env.SLACK_CHANNEL_EWS
```

Buttons:
- **Approve Communication Task** (action_id: `ews_approve_comm`)
- **No Action Needed** (action_id: `ews_no_action`)

Audience:
- CMO  
- Content Lead  

---

### **3. Listen for Human Decision**
Webhook `/ews_decision` listens for Slack interactive button events.

---

### **4. Route Decision**
A Switch node checks `action_id`.

#### If `ews_approve_comm`:
Triggers Workflow 04 to create a task for the copywriter.

#### If `ews_no_action`:
Logs the dismissal with:
```
event_type: ews_no_action
severity: low
actor: human
```

---

### **5. Respond to Slack Immediately**
Slack requires fast responses (< 3 seconds).  
We respond with:
```json
{"status":"received"}
```

---

## Environment Variables

| Variable | Description |
|---------|-------------|
| SLACK_BOT_TOKEN | Slack bot authentication |
| SLACK_CHANNEL_EWS | Channel for EWS alerts |
| WORKFLOW_04_TRIGGER_URL | Endpoint for Workflow 04 |
| LOGGING_WEBHOOK_URL | Logging engine endpoint |

---

## Logging Events
- `ews_approve_comm` → when humans approve communication  
- `ews_no_action` → when humans reject  

---

## Safe Modification Guidelines
- Never remove the human decision step.
- Slack must respond fast; keep Respond OK before long operations.
- Update `WORKFLOW_04_TRIGGER_URL` only if Workflow 04 changes location.
