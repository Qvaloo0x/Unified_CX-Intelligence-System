# Slack Setup

## Unified CX Intelligence System — Slack Integration Guide

Slack is the central decision-making interface of the Unified CX Intelligence System. All Early Warning System (EWS) alerts, human approvals, error notifications, and final publishing confirmations flow through Slack. This document explains how to configure the Slack App, required permissions, interactive components, n8n connections, and governance constraints.

---

# 1. Purpose of Slack in the System

Slack acts as:
- The **alert hub** for all EWS escalations  
- The **mandatory human approval checkpoint**  
- The **error & system health reporting channel**  
- The **final publishing confirmation layer**  

No automated decisions can proceed without Slack interaction.

---

# 2. Creating the Slack App

1. Visit: https://api.slack.com/apps  
2. Click **Create New App → From Scratch**  
3. App Name: `Unified CX Intelligence Bot`  
4. Choose the workspace where alerts will be received.

---

# 3. Required OAuth Scopes

The system must use the **minimum safe permissions** to preserve the Zero Auto-Publishing rule.

## 3.1 Allowed Scopes (Required)
| Scope | Purpose |
|-------|---------|
| `chat:write` | Allows posting alerts into internal channels |
| `chat:write.customize` | Enables formatting + blocks |
| `chat:write.public` | Optional—post in public internal channels |
| `commands` | Enables slash commands (optional) |
| `users:read` | Used to identify human reviewers |
| `users:read.email` | Optional—used for audit logs |

## 3.2 Forbidden Scopes (Never Allowed)
❌ `im:write`  
❌ `users:write`  
❌ `groups:write`  
❌ `channels:manage`  
❌ Any scope that allows interaction with customers  
❌ Any scope enabling automated direct messaging  

Slack must remain **internal-only** and **controlled**.

---

# 4. Event Subscriptions & Interactivity

### 4.1 Enable Interactivity
1. Go to Slack App → **Interactivity & Shortcuts**  
2. Toggle **Interactivity ON**  
3. Request URL:  
```
https://<your-n8n-domain>/webhook/slack_interactive
```

This URL receives:
- Approve / Reject / Clarify responses  
- Button interactions  
- Message actions  

### 4.2 (Optional) Enable Events
For slash commands or message-based triggers:
- Enable **Event Subscriptions**
- Add `app_mention` or `message.channels` as needed

---

# 5. Slack Channel Structure

### 5.1 #cx-alerts
Used for:
- EWS alerts  
- High/critical severity briefing  
- Human approval requests  

### 5.2 #cx-system
Used for:
- System errors  
- Workflow failures  
- HealthCheck reports  
- Logging notifications  

---

# 6. Slack → n8n Integration

Slack communicates with n8n via interactive payloads.

## 6.1 Inbound (Slack → n8n)
- Button clicks  
- Slash commands  
- Interactive components  

Delivered to:
```
https://<your-n8n>/webhook/slack_interactive
```

## 6.2 Outbound (n8n → Slack)
n8n uses:
- Slack Node  
- Bot Token with write-only internal permissions  

Sent messages include:
- Alerts  
- Decisions required  
- Review requests  
- Final confirmations  
- Error logs  

---

# 7. EWS Alert Structure

All alerts sent by Workflow `03_EWS_Alert_HumanDecision` follow a structured Block Kit format:

### Example:
```
[EWS Alert: HIGH SEVERITY]

Source: Twitter
User: @example
Message: "Your product is broken again!"
Sentiment: -0.82
Incident Score: 8.1/10

Actions:
[Approve] [Reject] [Request More Info]
```

Workflow pauses until Slack returns a human response.

---

# 8. Human Decision Flow

### 8.1 Approve
→ Triggers Asana task creation  
→ Logged in Sheets

### 8.2 Reject
→ Logged  
→ Workflow ends

### 8.3 Clarify
→ Sends additional details to Slack  
→ System waits again

---

# 9. Error & Health Reporting

Workflow `00_System_HealthCheck` posts:
- API failures  
- Rate limit warnings  
- Token issues  
- Platform outages  

Workflow errors across the system post detailed blocks to **#cx-system**.

---

# 10. Security Requirements

### 10.1 Workspace Governance
- Only internal team members may receive EWS alerts  
- No customer-facing channels allowed  
- Use a dedicated service account when possible  

### 10.2 Token Storage
- Store Slack Bot Token in **n8n’s encrypted vault**  
- Rotate token every 90 days  
- Never store credentials in code or GitHub  

### 10.3 Permission Hardening
- Disable DM permissions  
- Disable external posting  
- Disable channels:manage or groups:manage  
- Disable file uploads unless explicitly required  

---

# 11. Testing Checklist

### ✔ Bot can post to #cx-alerts  
### ✔ Interactive buttons work  
### ✔ n8n receives events from Slack  
### ✔ Logging entries created  
### ✔ Asana task is generated after approval  
### ✔ Reject flow logs correctly  
### ✔ Errors post to #cx-system  
### ✔ HealthCheck posts daily report  

Once all pass, Slack is fully configured.

---

# 12. Summary

Slack is the central operational and approval layer of the Unified CX Intelligence System.  
It ensures:
- human decision-making  
- safe gating of content workflows  
- zero auto-publishing  
- clear, auditable communication  

Slack is the **brainstem** of the system’s governance model.

