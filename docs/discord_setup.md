# Discord Setup

## Unified CX Intelligence System — Discord Integration Guide

This document describes the full setup of Discord as a **read-only monitoring source** inside the Unified CX Intelligence System.  
The integration follows the system’s strict governance rules of **zero auto‑publishing**, **no outbound communication**, and **100% human‑controlled decision‑making**.

---

# 1. Purpose of Discord Integration

Discord is used to capture community‑driven conversations, including:
- User frustration or complaint patterns  
- Feature feedback  
- Service outage reports  
- Rumor or misinformation spread  
- Private or semi‑private group discussions  

Discord data flows into:
```
Diagnostics → Logging → EWS → Human Decision
```

The bot **never posts**, **never replies**, **never moderates**, and **never interacts**.

---

# 2. Creating the Discord Bot

## 2.1 Create a Discord Application
1. Go to: https://discord.com/developers/applications  
2. Click **New Application**  
3. Name: `Unified CX Intelligence Bot`  

## 2.2 Add a Bot User
Inside your application:
1. Navigate to **Bot**  
2. Click **Add Bot**  
3. Enable:
   - **MESSAGE CONTENT INTENT**  
   - **SERVER MEMBERS INTENT** (optional)

## 2.3 Retrieve the Bot Token
Bot → Token → **Reset & Copy**

Store in n8n credentials vault.

---

# 3. Required Permissions (Read‑Only Mode)

### 3.1 Required
| Permission | Purpose |
|-----------|---------|
| Read Messages / View Channels | Allows bot to ingest messages |
| Read Message History | Enables processing of previous messages |
| Connect | Required for presence |

### 3.2 Forbidden Permissions (Must NEVER be enabled)
❌ Send Messages  
❌ Send Messages in Threads  
❌ Create Public/Private Threads  
❌ Manage Messages  
❌ Mention Everyone  
❌ Moderate Members  
❌ Administrator  

The bot must stay **read‑only**.

---

# 4. Inviting the Bot to the Server

## 4.1 Generate Safe OAuth2 URL
1. Application → OAuth2 → URL Generator  
2. Scopes:
   - `bot`
3. Bot Permissions:
   - **ONLY**: Read Messages, Read Message History  

Copy and use the generated invite link.

## 4.2 Add to Selected Channels
Add the bot **only** to channels intended for monitoring.

---

# 5. n8n Integration

Discord connects to n8n via:

## 5.1 Discord Credentials
1. n8n → **Credentials**  
2. Add **Discord API**  
3. Paste bot token  
4. Save securely  

## 5.2 Discord Trigger Node
Configure event:
- `messageCreate`

This triggers whenever a monitored channel receives a message.

---

# 6. Normalized Event Schema

All Discord events must be normalized into the system’s unified JSON format.

### Example:
```json
{
  "source": "discord",
  "timestamp": "2025-11-20T15:12:09Z",
  "channel_id": "996633441",
  "channel_name": "support-chat",
  "user_id": "5522994411",
  "username": "user42",
  "message": "Is the new update causing crashes?",
  "message_type": "text",
  "metadata": {
    "guild_id": "900442211"
  }
}
```

---

# 7. Dataflow (Discord → System)

```
Discord Message
     |
     v
Discord Trigger (n8n)
     |
     v
Normalization
     |
     v
02_Diagnostics
     |
  +--+----------------+
  |                   |
  v                   v
01_Logging_Engine   03_EWS Alert (Slack)
                         |
                         v
                  Human Decision
```

---

# 8. Diagnostics Behavior

Discord messages undergo:
- sentiment scoring  
- language intensity analysis  
- pattern and keyword detection  
- recurrence comparison  
- incident scoring (0–10)  

If **incident score ≥ 7.0**, EWS sends an alert to Slack.

---

# 9. EWS Alert Behavior

Slack alert contains:
- channel name  
- user  
- message  
- diagnostics summary  
- incident score  
- risk category  
- Approve / Reject / Clarify actions  

Workflow pauses until a human responds.

---

# 10. Security Requirements

### 10.1 Token Safety
- Stored only in n8n’s encrypted vault  
- Never committed to GitHub  
- Rotate regularly  

### 10.2 Permission Hardening
Ensure bot **cannot**:
- post  
- DM users  
- moderate  
- react to messages  

### 10.3 Compliance
Follow the official Discord Bot API Guidelines:
https://discord.com/developers/docs/intro

---

# 11. Testing Checklist

### ✔ Bot appears online  
### ✔ Bot reads messages in monitored channels  
### ✔ n8n receives events from Discord Trigger  
### ✔ Normalization works  
### ✔ Diagnostics evaluate and score events  
### ✔ Logs appear in Google Sheets  
### ✔ Slack EWS alerts trigger correctly  
### ✔ Human decisions unblock workflow  
### ✔ Bot never sends outbound messages  

---

# 12. Troubleshooting

### Bot not receiving messages
- Missing “Message Content Intent”  
- Insufficient channel permissions  
- Bot not added to channel  

### n8n not receiving events
- Wrong credentials  
- Trigger misconfiguration  
- Workflow inactive  

### Unexpected behavior
- Ensure bot has **no** send message permissions  
- Restart bot after permission updates  

---

# 13. Summary

Discord provides a powerful channel for monitoring community sentiment and surfacing early risk indicators.  
When configured in strict read‑only mode, it safely feeds into the system’s diagnostics and human decision pipeline—preserving governance, transparency, and operational safety.

