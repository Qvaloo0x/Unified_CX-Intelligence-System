# Social Setup

## Unified CX Intelligence System — Social Platform Setup Guide

This document provides configuration instructions for all supported social platforms in the Unified CX Intelligence System. All integrations follow the system's mandatory **read‑only**, **zero auto‑publishing**, and **human‑controlled** governance requirements.

---

# 1. Supported Platforms

The system supports ingestion from:

- **Twitter/X**
- **Reddit**
- **Instagram (Graph API)**
- **Discord** (see dedicated setup)
- **Telegram** (see dedicated setup)

Only **Twitter/X**, **Reddit**, and **Instagram** are documented here.

Each integration must operate in **strict read‑only mode**.

---

# 2. Shared Integration Rules

Regardless of platform:

### Allowed
✔ Read posts/comments/messages  
✔ Fetch metadata  
✔ Pull mentions or references  

### Forbidden
❌ Posting or replying  
❌ Direct messaging  
❌ Auto‑publishing  
❌ Moderation actions  
❌ Deleting or editing content  

### Credential Safety
- All tokens stored in n8n’s encrypted vault  
- Never commit credentials to the repository  
- Rotate tokens every 60–90 days  

---

# 3. Twitter/X Setup (Read‑Only)

Twitter/X access depends on available API level.

## 3.1 Obtain API Credentials
1. Go to: https://developer.x.com  
2. Create a project + App  
3. Generate:
   - API Key
   - API Secret
   - Bearer Token (preferred)

## 3.2 Required Permissions
Read‑only:
- `tweet.read`
- `users.read`

Forbidden:
- `tweet.write`
- `dm.write`
- `account.manage`

## 3.3 n8n Configuration
1. Go to **Credentials → Twitter API**  
2. Add bearer token  
3. Save in encrypted vault  

## 3.4 Polling Workflow Parameters
Typical settings:
- Query: brand keywords  
- Tweet fields: created_at, public_metrics  
- User fields: username, verified, id  
- Expand: referenced_tweets  

Workflow:
```
Twitter → Normalization → Diagnostics → Logging/EWS
```

---

# 4. Reddit Setup (Read‑Only)

## 4.1 Register Application
1. Visit: https://www.reddit.com/prefs/apps  
2. Create a **script app**  
3. Collect:
   - Client ID  
   - Client Secret  
   - Username  
   - Password  

## 4.2 Required Permissions
Reddit API is read‑only by default for script apps:
✔ Read public subreddits  
✔ Read comments  
✔ Read threads  

Forbidden actions:
❌ Posting  
❌ Moderation  
❌ Voting  
❌ Editing  

## 4.3 n8n Configuration
Use **Reddit API** credentials:
- Username  
- Password  
- Client ID  
- Client Secret  

## 4.4 Polling Parameters
Recommended:
- Subreddits: brand, industry, competitor spaces  
- Sort: new  
- Frequency: 1–5 minutes  

---

# 5. Instagram Setup (Graph API)

Instagram requires a Business or Creator account managed via Facebook.

## 5.1 Requirements
- Facebook Page  
- Instagram Business/Creator profile  
- Facebook Developer App  

## 5.2 Steps
1. Go to: https://developers.facebook.com  
2. Create an app (type: Business)  
3. Add:
   - **Instagram Basic Display**
   - **Instagram Graph API**
4. Link:
   - FB Page → IG Account  
   - App → FB Page  

## 5.3 Required Permissions (Read‑Only)
- `instagram_basic`  
- `pages_show_list`  
- `instagram_manage_insights` (optional)  

Forbidden:
- `instagram_content_publish`  
- `pages_manage_posts`  
- Any write‑capable permission  

## 5.4 n8n Configuration
Add credentials under **Facebook Graph API**:
- App ID  
- App Secret  
- Page Access Token  

## 5.5 Polling Setup
Use Instagram endpoint:
```
/{ig-user-id}/media
```

Retrieve:
- caption  
- comments_count  
- like_count  
- timestamp  

Then:
```
Normalization → Diagnostics → Logging/EWS
```

---

# 6. Normalization Layer (Shared)

All platforms must map into this unified schema:

```json
{
  "source": "twitter|reddit|instagram",
  "timestamp": "...",
  "user": "...",
  "user_id": "...",
  "message": "...",
  "url": "...",
  "metadata": {...}
}
```

This allows consistent diagnostics and scoring.

---

# 7. Diagnostics Routing (Shared)

All social events pass into the diagnostics workflow:

```
Sentiment Analysis → Incident Scoring → Risk Classification
```

Routing:
- Score < 5.0 → log only  
- Score ≥ 7.0 → EWS Slack Alert  

---

# 8. Governance & Safety Controls

All social integrations must comply with:

### Zero Auto‑Publishing Rule  
No workflow may contain:
- HTTP POST to social platforms  
- Automation capable of posting  
- Third‑party services used to publish  

### Least‑Privilege Access  
Only read scopes are allowed.

### Human‑Controlled Decisions  
All escalations must route into Slack for approval.

---

# 9. Troubleshooting

### No data from Twitter/X
- Bearer token expired  
- Query filters too narrow  
- Rate limiting (wait 15 min)  

### Reddit errors
- Incorrect Client Secret  
- OAuth failure  
- Account not authorized  

### Instagram issues
- Page not linked to IG account  
- Missing read permissions  
- Using Legacy Instagram API  

---

# 10. Summary

Social monitoring is fully integrated under strict governance rules:
- Read‑only ingestion  
- AI diagnostics  
- Logging  
- Human decision gating  
- No automatic publishing  

This ensures safe, compliant, and reliable CX intelligence across all active social channels.
