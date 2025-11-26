# Telegram Setup

## Unified CX Intelligence System — Telegram Integration Guide

This document provides the complete technical instructions to integrate Telegram as a **read‑only monitoring source** inside the Unified CX Intelligence System.  
Telegram ingestion supports group monitoring, channel monitoring, and direct messages, all under the strict **Zero Auto‑Publishing Rule**.

---

# 1. Purpose of Telegram Integration

Telegram is used to capture:
- Fast‑moving group discussions  
- Outage reports  
- Rumor propagation  
- User frustration patterns  
- Private community sentiment  

The bot **never posts**, **never replies**, **never moderates**, and **never interacts**.

Its function is exclusively:
```
Read → Normalize → Diagnostics → Logging → EWS → Human Decision
```

---

# 2. Creating the Telegram Bot

## 2.1 Create Bot with BotFather
1. Open Telegram  
2. Search for **@BotFather**  
3. Send:
```
/start
/newbot
```
4. Provide:
   - Bot name  
   - Bot username (must end in `_bot`)  

5. BotFather returns a token:
```
1234567890:ABC-EXAMPLE-TOKEN
```

This token is required for n8n.

---

# 3. Required Permissions (Read‑Only)

To guarantee safety:

### The bot must **NOT**:
- Be an admin  
- Have posting privileges  
- Have moderation privileges  
- Send messages automatically  

### The bot MUST:
- Be a **regular member** if added to a group  
- Have **privacy mode disabled** (optional but recommended)

---

# 4. Add Bot to Groups (Read‑Only Mode)

## 4.1 Add to a Public or Private Group
1. Open group → **Add member**  
2. Search for your bot username  
3. Add as **regular member only**  
4. Confirm the bot **cannot post**

## 4.2 Disable Privacy Mode (BotFather)
This allows the bot to read group messages:
```
/setprivacy
Disable
```

---

# 5. n8n Configuration

## 5.1 Add Telegram Credentials
1. n8n → **Credentials**  
2. Add **Telegram Bot API**  
3. Paste your token  
4. Save in encrypted vault  

## 5.2 Use Telegram Trigger Node
Configure:
- Trigger On: **messages**  
- Types: text, media, forwarded messages  
- Polling interval: default (recommended)

This node outputs all incoming updates.

---

# 6. Normalized Event Schema

All Telegram messages must be normalized into the system’s unified JSON format.

### Example:
```json
{
  "source": "telegram",
  "timestamp": "2025-11-20T14:33:12Z",
  "chat_id": "-1001234567890",
  "chat_name": "Support Group",
  "user": "John Doe",
  "user_id": 4411223344,
  "message": "Is checkout down?",
  "message_type": "text",
  "url": null,
  "metadata": {
    "forwarded": false
  }
}
```

---

# 7. Telegram → System Dataflow

```
Telegram Message
        |
        v
Telegram Trigger (n8n)
        |
        v
Normalization Layer
        |
        v
02_Diagnostics
        |
   +----+-----------------+
   |                      |
   v                      v
01_Logging_Engine   03_EWS_Alert_HumanDecision
                          |
                          v
                    Human Decision
                          |
                          v
               04_Copywriter_Task_Creation
```

---

# 8. Diagnostics Behavior

Telegram messages are analyzed for:
- sentiment  
- anomaly patterns  
- repeated user complaints  
- urgency indicators  
- mention of outages  
- misinformation  
- escalation keywords  

If the **incident score ≥ 7.0**, the system triggers an EWS alert.

---

# 9. EWS Escalation Structure

Slack receives:
- Source: Telegram  
- User + group  
- Message content  
- Risk score  
- Summary  
- Incident score  
- Approve / Reject / Clarify buttons  

The workflow pauses until review.

---

# 10. Security & Compliance

### 10.1 Token Safety
- Store in n8n credentials vault  
- Never commit to GitHub  
- Rotate every 90 days  

### 10.2 Bot Behavior Restrictions
The bot cannot:
- Post messages  
- Forward messages  
- Delete or edit messages  
- Moderate groups  
- DM users  

### 10.3 Privacy Requirements
- Warn community members if required by policy  
- Follow Telegram Bot API guidelines:
  https://core.telegram.org/bots/api

---

# 11. Testing Checklist

### ✔ Bot reads group messages  
### ✔ n8n detects updates in real time  
### ✔ Normalization works correctly  
### ✔ Diagnostics compute scores  
### ✔ Logs are created in Google Sheets  
### ✔ EWS alerts appear in Slack  
### ✔ Human decisions unblock workflow  
### ✔ Bot never sends outbound messages  

Once all steps pass, Telegram is fully integrated.

---

# 12. Troubleshooting

### Bot not receiving group messages
- Privacy mode ON → disable  
- Bot added incorrectly → re-add  
- Group permissions restrictive → adjust  

### n8n not receiving updates
- Check credentials  
- Ensure workflow is active  
- Validate Telegram Trigger polling  

### Duplicate messages
- Ensure state tracking is configured  
- Enable “Ignore repeated updates” if needed  

---

# 13. Summary

Telegram acts as a powerful monitoring extension for capturing community chatter, urgent inquiries, and early signs of customer issues.  
The bot is **strictly read‑only** and cannot publish under any circumstances.  
All escalations, decisions, and content flows remain fully human-controlled.

