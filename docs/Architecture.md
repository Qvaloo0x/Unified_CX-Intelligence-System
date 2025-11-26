# Architecture

## Unified CX Intelligence System — Full Architecture (Updated with Reporting Layer)

This document describes the complete architecture of the Unified CX Intelligence System, including the newly added **Reporting Layer**, the **Weekly Report workflow**, and the analytics dashboard.

---

# 1. High-Level Architecture

The system is composed of five major layers:

1. **Ingestion Layer**  
2. **Diagnostics Layer**  
3. **Decision Layer (Human-in-the-Loop)**  
4. **Content Workflow Layer**  
5. **Reporting & Analytics Layer**  ← NEW

Each layer is modular, isolated, and strictly governed to maintain safety and auditability.

---

# 2. Ingestion Layer

Sources include:

- Twitter/X (read-only)  
- Reddit (read-only)  
- Instagram (read-only)  
- Telegram (read-only)  
- Discord (read-only)  
- Slack (internal signals only)

All platforms feed normalized JSON into the Diagnostics workflow.

**Key properties:**
- No outbound communication  
- No moderation  
- Read-only API scopes  
- Credential vault enforced in n8n  

---

# 3. Diagnostics Layer

The Diagnostics workflow performs:

- Sentiment analysis  
- Incident scoring  
- Recurrence analysis  
- Topic/keyword extraction  
- Threat & anomaly detection  
- Platform weighting  
- Risk tier classification

Outputs flow into:

- Logging Engine  
- EWS Alert workflow  

AI is used strictly for **analysis**, never for action.

---

# 4. Decision Layer (Human‑in‑the‑Loop)

This layer enforces human control.

### Workflow: 03_EWS_Alert_HumanDecision

When Diagnostics identifies high-risk events:

- Slack notification is triggered  
- Human must choose **Approve / Reject / Clarify**  
- Workflow pauses until decision is made  
- Every step is logged

This ensures 100% human governance.

---

# 5. Content Workflow Layer

Includes three workflows:

### 04_Copywriter_Task_Creation
- Creates Asana tasks with diagnostic context  
- Assigns severity, metadata, and categories  
- Human approval required before task creation  

### 05_Content_Review_Approval
- Tracks status in Asana  
- Sends Slack notifications for:
  - Review requested  
  - Changes needed  
  - Approval granted  

### 06_Final_Publishing_Confirmation
- Ensures content is manually reviewed before publishing  
- No auto‑publishing permitted  
- System pauses for human validation  

---

# 6. Reporting & Analytics Layer (NEW)

This new layer provides **executive visibility** and consolidates intelligence into a weekly strategic summary.

Components:

### **6.1 Metrics.md**
Defines:
- System health metrics  
- Event intelligence metrics  
- Severity/risk metrics  
- Team responsiveness metrics  
- Sentiment & topic metrics  
- Governance & safety indicators  

### **6.2 Dashboard Template (Excel/Sheets)**
Transforms raw logs into:
- Volume trends  
- Severity distribution  
- Sentiment evolution  
- Cross‑platform comparisons  
- EWS & Asana performance  
- System health indicators  

Designed for CMO‑level insight.

### **6.3 Weekly Report Workflow (07_Weekly_Report.json)**
Runs every Monday:
- Aggregates logs  
- Computes weekly KPIs  
- Summarizes sentiment & severity  
- Highlights emerging themes  
- Measures operational responsiveness  
- Sends executive summary to Slack  

This elevates the system from technical tool to **executive intelligence asset**.

---

# 7. Data Flow Diagram (Updated)

```
External Inputs (Twitter, Reddit, Instagram, Discord, Telegram)
              |
              v
        02_Diagnostics
              |
     +--------+--------+
     |                 |
     v                 v
01_Logging_Engine   03_EWS_Alert_HumanDecision
                          |
                          v
                    Human Decision
                          |
                          v
            +-------------+-------------+
            |                           |
            v                           v
04_Copywriter_Task_Creation      (Rejected / Clarify)
            |
            v
05_Content_Review_Approval
            |
            v
06_Final_Publishing_Confirmation
            |
            v
     Manual Publishing Only
            |
            +-------------------------------+
            |                               |
            v                               v
   Logging Layer                     Reporting Layer (NEW)
                                         |
                               +---------+---------+
                               |                   |
                               v                   v
                 Weekly Executive Summary     Dashboard Analytics
```

---

# 8. Key Architectural Principles

### 8.1 Zero Auto‑Publishing  
No workflow can publish or communicate with public platforms.

### 8.2 Human Approval Required  
All critical flows require manual Slack approval.

### 8.3 Read‑Only External Integrations  
All ingestion uses the minimum permissions possible.

### 8.4 AI = Advisory Only  
AI provides scoring and analysis, never decision-making.

### 8.5 Full Auditability  
All workflows log actions to Google Sheets.

---

# 9. Security Architecture

- n8n credentials vault  
- Principle of least privilege for API keys  
- HealthCheck workflow monitors:
  - API connectivity  
  - Token validity  
  - Workflow uptime  
- Slack + Sheets used as internal-only control plane  

---

# 10. Summary

The Unified CX Intelligence System is now composed of:

- **Operational CX intelligence**  
- **Human-controlled decision frameworks**  
- **Content workflow integration**  
- **Executive reporting and analytics**  

The newly added **Reporting & Analytics Layer** enhances strategic visibility, enabling leadership to make informed decisions while preserving strict safety and governance.

