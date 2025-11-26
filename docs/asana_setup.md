# Asana Setup

## Unified CX Intelligence System — Asana Integration Guide

This document provides complete configuration steps to integrate Asana into the Unified CX Intelligence System. Asana is used as the **content workflow hub**, where approved EWS alerts are transformed into structured tasks for content teams. All actions remain fully human-controlled.

---

# 1. Purpose of Asana Integration

Asana is used for:

- Creating content tasks triggered by EWS human approval  
- Structuring context, diagnostics, and recommendations  
- Managing content review workflows  
- Tracking content readiness  
- Providing an audit trail of decisions  

Asana **never publishes content** — it exists purely for internal coordination.

---

# 2. Requirements

Before starting:

- An Asana workspace or organization  
- A project dedicated to CX or Comms workflows  
- Asana Personal Access Token (PAT)  
- n8n instance with access to credentials vault  

---

# 3. Creating an Asana Personal Access Token (PAT)

1. Log in to Asana  
2. Go to **My Settings → Apps → Developer Apps**  
3. Click **Create New Token**  
4. Name it: `Unified CX Intelligence Token`  
5. Copy the generated token

**Security:**  
- Store token **ONLY** in n8n’s encrypted vault  
- Never commit it to GitHub  
- Rotate every 90 days  

---

# 4. Asana Project Configuration

Create a project such as:

**Project Name:** `CX Content Pipeline`

Recommended sections:

- **To Do**  
- **In Review**  
- **Needs Edits**  
- **Approved for Publishing**  
- **Archived**  

Recommended fields:

| Custom Field | Type | Purpose |
|--------------|-------|---------|
| Severity | Dropdown | Mirrors incident score tiers |
| Source | Dropdown | Twitter, Reddit, Instagram, Discord, Telegram |
| Reviewer | Text | Human reviewer name |
| Approval Status | Dropdown | Pending, Approved, Needs Changes |

---

# 5. n8n Integration

## 5.1 Set Up Asana Credentials in n8n

1. n8n → **Credentials**  
2. Choose **Asana API**  
3. Add:
   - Personal Access Token  
   - Optional: Default Workspace ID  
4. Save credentials securely  

## 5.2 Find Required IDs

### Workspace ID
Use GET:
```
https://app.asana.com/api/1.0/workspaces
```

### Project ID
Open project in Asana and copy last numeric segment of URL.

### Section IDs
Fetch via:
```
https://app.asana.com/api/1.0/projects/{project_id}/sections
```

---

# 6. Workflow: Copywriter Task Creation (04_Copywriter_Task_Creation)

Triggered after human approval in Slack.

### Task Structure

**Title Example:**  
`[EWS] Payment outage complaints increasing — Twitter`

**Description Includes:**

- Incident summary  
- Original message  
- Source platform  
- Incident score + risk factors  
- Diagnostic outputs  
- Slack thread link  
- Recommended next steps  

**Tags:**  
- `EWS`  
- `High Severity`  
- Channel-specific tags (optional)

**Custom Fields:**  
- Severity  
- Source  
- Approval Status (initially: Pending)  

**Assignments:**  
- Assigned to content lead or writer  

---

# 7. Workflow: Content Review (05_Content_Review_Approval)

This workflow monitors Asana for content task updates.

### Review events include:
- Status changed to “In Review”  
- Content Lead leaves feedback  
- Approval status updated  

### Slack notifications sent for:
- Review requested  
- Changes required  
- Approval granted  

All actions logged in Google Sheets.

---

# 8. Workflow: Final Publishing Confirmation (06_Final_Publishing_Confirmation)

Triggered when:

- Asana Approval Status = **Approved**  
- Section = **Approved for Publishing**

Slack sends a message:

```
Content Approved ✓

Final check required before manual publishing.
Buttons: [Confirm] [Hold] [Reject]
```

Workflow pauses until a human confirms.

**Important:**  
Publishing is always **manual**, outside of system automation.

---

# 9. Governance & Safety

### 9.1 Allowed
✔ Creating tasks  
✔ Updating fields  
✔ Moving tasks between sections  
✔ Reading comments  
✔ Sending Slack notifications  

### 9.2 Forbidden
❌ Auto-publishing  
❌ Auto-responding to customers  
❌ Auto-sending Asana comments  
❌ Removing human review steps  
❌ Using AI to publish drafts automatically  

The system ensures complete human governance.

---

# 10. Testing Checklist

### Task Creation
✔ EWS approval → Asana task created  
✔ Custom fields populated  
✔ Diagnostic summary included  

### Review Flow
✔ Reviewer updates task  
✔ Slack notifications trigger  
✔ Logging entries created  

### Final Publishing Flow
✔ Approved task triggers Slack final check  
✔ Human confirmation required  
✔ No auto-publishing occurs  

---

# 11. Troubleshooting

### Task not created
- Wrong project ID  
- Invalid token  
- Missing Asana credentials  

### Review notifications missing
- Incorrect Asana trigger node  
- Missing field mapping  
- Slack channel misconfigured  

### Errors in logs
- Check Sheets format  
- Verify Asana API rate limits  
- Inspect n8n execution logs  

---

# 12. Summary

Asana acts as the **content production hub** for the Unified CX Intelligence System:
- Generates structured tasks  
- Supports multi-step human review  
- Enforces approval discipline  
- Integrates directly with Slack  
- Guarantees zero auto-publishing  

This workflow ensures the organization maintains safe, compliant, and high-quality communication processes.
