# Weekly_Report

## Unified CX Intelligence System — Weekly Executive Summary Workflow

This document provides full documentation for the **07_Weekly_Report** workflow, responsible for generating a weekly executive-level summary of CX intelligence signals, system activity, and operational performance.  
The summary is delivered automatically to Slack every Monday morning.

---

# 1. Purpose of the Weekly Report

The weekly report is designed to:

- Provide leadership (ex: CMO, CX Director) with **high‑level intelligence**
- Summarize cross‑platform activity for the week
- Highlight **risk trends** and **incident severity**
- Identify **common topics** and emerging issues
- Measure **team responsiveness** and workflow performance
- Strengthen executive visibility without exposing internal logs

This workflow transforms raw signals into actionable insights.

---

# 2. Trigger & Schedule

### **Trigger Type:** Cron  
### **Frequency:** Weekly  
### **Default Schedule:** Every Monday at 09:00 UTC  
(Adjustable depending on the organization’s timezone.)

Example n8n trigger configuration:
```
Mode: Weekly
Day: Monday
Hour: 09:00
Timezone: UTC
```

---

# 3. Data Sources

The weekly report reads from the **Google Sheets audit log** created by:

- 01_Logging_Engine  
- 02_Diagnostics  
- 03_EWS_Alert_HumanDecision  
- 04_Copywriter_Task_Creation  
- 05_Content_Review_Approval  
- 06_Final_Publishing_Confirmation  

This ensures that every important action is captured and included.

---

# 4. Metrics Extracted

The Weekly Report includes:

---

## 4.1 Volume Metrics
- Total number of inbound events  
- Events per platform (Twitter, Reddit, Telegram, Discord, Instagram)  
- Events per severity tier (Low / Mild / Moderate / High / Critical)

---

## 4.2 Risk & Incident Metrics
- Average incident score  
- Number of high‑severity events  
- Number of critical‑severity events  
- Top 5 topics/issues  
- Alerts escalated to Slack (EWS)

---

## 4.3 Team Performance Metrics
- Average time to acknowledge an EWS alert  
- Average time from approval → Asana task creation  
- Average time from task creation → approval  
- Tasks requiring revisions  
- Reviewer workload distribution

---

## 4.4 Sentiment Metrics
- Positive / Neutral / Negative distribution  
- Weekly sentiment trend  
- Platform sentiment comparison

---

## 4.5 Governance & System Health
- Workflow success rate  
- Workflow failures  
- Credential issues detected  
- HealthCheck warnings  
- Logging completeness

---

# 5. Output Format (Slack Message)

The message sent to Slack follows a concise executive format:

---

### **Example Weekly Report Message**

```
📊 CX Intelligence — Weekly Summary

🗓 Period: Nov 10–17

📌 Volume
• Total events: 742
• Twitter: 312 | Reddit: 188 | Discord: 124 | Telegram: 79 | Instagram: 39

🔥 Severity
• High severity: 22
• Critical severity: 6
• Avg incident score: 6.4

💬 Sentiment
• Positive: 34%
• Neutral: 41%
• Negative: 25%
• Trend vs last week: -3.2%

🚨 Escalations
• EWS alerts: 11
• Approved: 9 | Rejected: 2
• Avg response time: 16 minutes

📝 Content Production
• Asana tasks created: 9
• Approved this week: 7
• Tasks requiring changes: 3

🛡 System Health
• Workflow uptime: 99.6%
• Errors detected: 1
• Credential alerts: 0
```

---

# 6. Workflow Structure

The Weekly Report workflow includes:

```
Weekly Trigger (Cron)
        |
        v
Read Logs (Google Sheets)
        |
        v
Aggregate & Process Metrics (Function Node)
        |
        v
Format Slack Message (Function Node)
        |
        v
Send Slack Report (Slack Node)
```

---

# 7. Customization Options

You can customize:

### 7.1 Schedule  
Change the cron trigger to any day/time.

### 7.2 Slack Channel  
Default: `#cx-alerts`  
Can be changed to a dedicated analytics channel.

### 7.3 Metrics Included  
Add or remove:
- sentiment breakdown  
- reviewer performance  
- platform comparisons  
- category clusters  

### 7.4 Thresholds  
Adjust thresholds for:
- high severity  
- critical severity  
- EWS routing  

---

# 8. Troubleshooting

### **Report not sending**
- Check Slack token permissions  
- Verify the Slack node is linked to the correct credentials  

### **No data in report**
- Verify Logging Engine is writing to Sheets  
- Check sheet ID and range in the Google Sheets node  

### **Incorrect dates**
- Adjust timezone in Cron node  
- Ensure sheet timestamps are consistent  

### **Workflow errors**
- Check n8n execution logs  
- Ensure JSON parsing in Function nodes is correct  

---

# 9. Limitations

- This workflow summarizes **only what the system logs**  
- Does not access:
  - revenue metrics  
  - ad performance  
  - CRM data  
  - ticketing systems  

- Sentiment and incident scoring depend on diagnostic model accuracy  
- Requires Logging Engine to be healthy and complete  

---

# 10. Summary

The Weekly Report workflow enables leadership to receive a concise, data‑driven overview of CX intelligence and emerging risks.  
It converts raw cross‑platform signals into a single, human‑readable summary that supports:

- better decision-making  
- proactive risk management  
- improved team performance  
- executive-level oversight  

This elevates the system from an operational tool to an **executive intelligence asset**.

