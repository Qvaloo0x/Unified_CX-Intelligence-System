
# Workflow 05 — Content Review & Approval

## Overview
This workflow manages the **human editorial review** after the copywriter finishes a communication draft.  
It is triggered via an **Asana webhook** when the task is updated to `ready_for_review`.

The workflow:
1. Validates the incoming Asana event  
2. Notifies the Content Lead in Slack  
3. Waits for a human decision  
4. Updates Asana to either **approved** or **changes_required**  
5. Logs the decision through the central Logging Engine  

This preserves a fully accountable, human-in-the-loop approval chain.

---

## Trigger
### **Webhook — `/content_review`**
Triggered by Asana when:
```
custom_fields.review_status = "ready_for_review"
```

Payload includes at minimum:
- `task_gid`
- `status`
- `event_type`

---

## Workflow Steps

### **1. Validate Payload**
Ensures:
- `task_gid` exists  
- `status` exists  

If not, the workflow stops immediately.

---

### **2. Route Ready-for-Review Tasks**
Only tasks with:
```
status = "ready_for_review"
```
continue to the next step.

---

### **3. Notify Content Lead via Slack**
Slack message includes:
- Explanation of what is ready  
- Two buttons:
  - **Approve**
  - **Request Changes**

Sent to:
```
$env.SLACK_CHANNEL_REVIEW
```

---

### **4. Listen for Human Decision**
A second webhook:
```
/review_decision
```
captures Slack interactive button clicks.

Slack sends:
- `action_id = review_approve`
- `action_id = review_changes`

---

### **5. Update Asana Based on Decision**

#### **If Approved**
Asana task is updated with:
```
custom_fields.review_status = "approved"
```

#### **If Changes Required**
```
custom_fields.review_status = "changes_required"
```

---

### **6. Log Review Outcome**
Logs the event:
```
event_type: "content_review_decision"
severity: medium
actor: human
```

---

### **7. Respond to Webhook**
Responds:
```json
{"status":"reviewed"}
```

---

## Environment Variables

| Variable | Description |
|---------|-------------|
| ASANA_ACCESS_TOKEN | Needed to update tasks |
| SLACK_BOT_TOKEN | Needed to post messages |
| SLACK_CHANNEL_REVIEW | Where review requests go |
| LOGGING_WEBHOOK_URL | Central logging endpoint |

---

## Safe Modification Guidelines
- Never remove the human approval step — required for safe communication.
- Ensure Slack interactions respond within **3 seconds** to avoid timeout.
- If adding more decision options, update both router and Asana custom fields.
- Maintain consistency with Logging Engine format.

---

End of Document
