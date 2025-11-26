# Incident Scoring

## Unified CX Intelligence System — Severity & Risk Scoring Model

This document defines the full incident scoring framework used across the Unified CX Intelligence System. The scoring system is designed for **consistency**, **transparency**, and **enterprise governance**, ensuring all inbound events are classified objectively and escalated appropriately — always with **100% human control** over final decisions.

---

# 1. Purpose of the Incident Scoring Model

The scoring model answers:

- How severe is this event?  
- Does it require human review?  
- Should it trigger an EWS alert?  
- How should CX or Comms prioritize their response?  

It ensures:
- Structured prioritization  
- Early detection of emerging issues  
- Reduced noise  
- Full consistency across platforms  
- A transparent, reviewable escalation chain  

---

# 2. Scoring Overview

Each inbound event receives a score from **0.0 to 10.0**.

### Score Tiers
| Score Range | Severity | System Behavior |
|-------------|----------|------------------|
| 0.0–2.4 | Low | Log only |
| 2.5–4.9 | Mild | Log + monitor |
| 5.0–6.9 | Moderate | Optional Slack alert |
| 7.0–8.4 | High | Required Slack alert |
| 8.5–10.0 | Critical | Immediate Slack alert |

Only **human reviewers** authorize next steps.

---

# 3. Incident Score Formula

```
Incident Score =
(0.35 × Sentiment Severity) +
(0.25 × Recurrence Level) +
(0.20 × Impact Score) +
(0.10 × Spread Potential) +
(0.10 × Platform Weight)
```

Each component scored 0–10.

---

# 4. Scoring Factors (Detailed)

## 4.1 Sentiment Severity (35%)
Measures emotional intensity.

| Description | Score |
|-------------|--------|
| Neutral/positive | 0–2 |
| Mild frustration | 3–4 |
| High dissatisfaction | 5–6 |
| Angry, urgent complaints | 7–8 |
| Crisis, threats, severe harm language | 9–10 |

Derived from AI-assisted tone analysis.

---

## 4.2 Recurrence Level (25%)
Tracks how often similar messages appear.

| Frequency | Score |
|----------|--------|
| Single isolated event | 0–2 |
| Occasional recurrence | 3–4 |
| Several cases in last 24h | 5–6 |
| Multi-platform trend | 7–8 |
| Sustained spike across channels | 9–10 |

Helps detect emerging crises.

---

## 4.3 Impact Score (20%)
Evaluates potential operational or reputational harm.

| Impact Type | Score |
|-------------|--------|
| Minor confusion or feedback | 0–2 |
| Small feature issues | 3–4 |
| Service degradation | 5–6 |
| Payment issues / major bug | 7–8 |
| Outages, security flags, data issues | 9–10 |

---

## 4.4 Spread Potential (10%)
Likelihood of issue spreading or going viral.

| Level | Score |
|--------|--------|
| Low engagement | 0–3 |
| Moderate | 4–6 |
| High virality risk | 7–10 |

Based on engagement metrics and platform velocity.

---

## 4.5 Platform Weight (10%)
Not all platforms carry equal risk exposure.

| Platform | Weight | Notes |
|----------|--------|--------|
| Reddit | 8 | Deep, long-form issues |
| Twitter/X | 6 | High visibility + reach |
| Instagram | 5 | Moderate spread |
| Telegram | 4 | Fast group discussions |
| Discord | 4 | Tight-knit communities |
| Slack | 3 | Internal signal only |

Normalized to 0–10.

---

# 5. Example Calculations

## Example 1 — Frustrated tweet about checkout
```
Sentiment: 8
Recurrence: 6
Impact: 7
Spread: 3
Platform: 6
```

Final:
```
(0.35*8) + (0.25*6) + (0.20*7) + (0.10*3) + (0.10*6)
= 2.8 + 1.5 + 1.4 + 0.3 + 0.6
= 6.6 (Moderate)
```

---

## Example 2 — Reddit post claiming data breach
```
Sentiment: 10
Recurrence: 8
Impact: 10
Spread: 9
Platform: 8
```

Final:
```
= 9.2 (Critical)
```

Triggers immediate EWS alert.

---

## Example 3 — Minor Telegram complaint
```
Sentiment: 4
Recurrence: 1
Impact: 2
Spread: 2
Platform: 4
```

Final:
```
= 2.65 (Mild)
```

Logged only.

---

# 6. Escalation Logic

### IF Score ≥ 7.0  
→ Slack EWS alert required  
→ Human must approve/reject  

### IF Score < 7.0  
→ Logged only  
→ No escalation  

### Human override always allowed
Human reviewers may override severity at any time.

---

# 7. Logging Requirements

Every scored event logs:

- final score  
- scoring breakdown  
- source platform  
- timestamp  
- human decision (if applicable)  
- diagnostic details  

Stored in Google Sheets.

---

# 8. Adjusting the Scoring Model

Updates may be needed for:
- new platforms  
- industry risk changes  
- crisis sensitivity  
- product evolution  

All changes must be:
- reviewed by governance  
- recorded in `CHANGELOG.md`  
- tested before deployment  

---

# 9. Summary

The incident scoring system ensures:
- consistent risk assessment  
- safe routing of events  
- full adherence to human-in-the-loop governance  
- zero automation in customer communication  

It is the backbone of early warning and CX intelligence operations.

