# Dataflows (Updated)

## Unified CX Intelligence System — Data Flow Specification (Including Reporting Layer)

This document defines the updated end‑to‑end data flows across all layers of the Unified CX Intelligence System, incorporating the newly added **Reporting & Analytics Layer** and the **Weekly Report workflow**.

---

# 1. High-Level Data Flow

Below is the complete, updated data flow from ingestion to reporting:

```
External Inputs → Diagnostics → Logging → EWS Decision → Content Workflows → Manual Publishing → Reporting Layer
```

Each stage is isolated, logged, and governed by strict human approval.

---

# 2. Ingestion Data Flow

### **Sources**
- Twitter/X  
- Reddit  
- Instagram  
- Telegram  
- Discord  
- Slack (internal submissions)

### **Flow**
```
Platform APIs (Read-Only)
        |
        v
n8n Ingestion Nodes
        |
        v
Normalization Node (unified schema)
        |
        v
02_Diagnostics
```

**Normalization Schema Example:**
```json
{
  "source": "twitter",
  "timestamp": "2025-11-20T15:12:09Z",
  "user": "username",
  "user_id": "12345",
  "message": "Example content",
  "sentiment_raw": null,
  "metadata": {...}
}
```

---

# 3. Diagnostics Data Flow

### Inputs:
- Normalized social events  
- Internal signals from Slack  

### Processing:
- Sentiment analysis  
- Severity scoring  
- Keyword/topic detection  
- Recurrence analysis  
- Platform weighting  
- Risk tier classification  

### Outputs:
- Diagnostic summary  
- Incident score  
- Routing decision  

### Flow:
```
Normalized Input
       |
       v
AI-Assisted Analysis
       |
       +-------------------------+
       |                         |
       v                         v
01_Logging_Engine         03_EWS_Alert_HumanDecision
```

---

# 4. Logging Data Flow

All events and workflow actions are logged in a unified structure inside Google Sheets.

### Logged Fields:
- Timestamp  
- Source platform  
- Sentiment score  
- Incident score  
- Alert triggered (Y/N)  
- Human decision outcome  
- Workflow name  
- Status (success/failure)  

### Flow:

```
System Event
      |
      v
01_Logging_Engine
      |
      v
Google Sheets Log (Persistent Storage)
```

This sheet now also serves as the **data foundation for the dashboard and weekly reporting**.

---

# 5. EWS Decision Data Flow

Triggered for high‑risk events (incident score ≥ threshold).

### Inputs:
- Diagnostic outputs  
- Event metadata  

### Flow:

```
High-Risk Event
      |
      v
03_EWS_Alert_HumanDecision
      |
      +---------------------------+
      |                           |
      v                           v
Human Approves             Human Rejects / Clarifies
      |
      v
04_Copywriter_Task_Creation
```

The workflow pauses until the human acts.

---

# 6. Content Workflow Data Flow

### Flow:

```
Human Approval
      |
      v
04_Copywriter_Task_Creation
      |
      v
Asana Task (with diagnostics context)
      |
      v
05_Content_Review_Approval
      |
      v
06_Final_Publishing_Confirmation
      |
      v
Manual Human Publishing
```

No automatic publishing occurs at any point.

---

# 7. Reporting & Analytics Data Flow (NEW)

This new data flow extracts intelligence from system logs and transforms it into executive insights.

---

## 7.1 Weekly Report Data Flow

```
Google Sheets Log
        |
        v
07_Weekly_Report (Cron Triggered)
        |
        v
Aggregate Weekly Metrics
        |
        v
Format Executive Summary
        |
        v
Slack Delivery (CX/CMO Channel)
```

Metrics extracted:
- Volume  
- Severity distribution  
- Sentiment trends  
- Escalation patterns  
- Response times  
- Top topics  
- System health indicators  

---

## 7.2 Dashboard Data Flow

```
Google Sheets Log
        |
        v
Pivot Tables (Volume, Severity, Sentiment)
        |
        v
Charts & Visualizations
        |
        v
CX Intelligence Dashboard (Sheets/BI Tool)
```

Dashboard visuals include:
- Volume per day  
- Severity distribution  
- Sentiment evolution  
- Platform comparisons  
- Workflow performance  
- System uptime  

---

# 8. End-to-End Updated Dataflow Diagram

```
[ Read-Only Platform APIs ]
            |
            v
[ Ingestion Nodes ]
            |
            v
[ Normalization ]
            |
            v
[ 02_Diagnostics ]
      /                        v                     v
[ 01_Logging ]    [ 03_EWS_Alert_HumanDecision ]
                          |
                          v
                    Human Decision
                          |
                          v
           +--------------+--------------+
           |                             |
           v                             v
 [ 04_Copywriter_Task_Creation ]   [ Rejected ]
           |
           v
 [ 05_Content_Review_Approval ]
           |
           v
 [ 06_Final_Publishing_Confirmation ]
           |
           v
    Manual Publishing
           |
           +----------------------------------------------+
           |                                              |
           v                                              v
Google Sheets Log (Historical Data)        Reporting Layer (NEW)
                                                    |
                      +-----------------------------+----------------------------+
                      |                                                              |
                      v                                                              v
           Weekly Executive Summary                                       Dashboard Analytics
              (Slack → Leadership)                                  (Sheets / BI Visualization)
```

---

# 9. Summary

This updated dataflow demonstrates how the system now supports:

- Real‑time diagnostics  
- Human‑controlled routing  
- Structured content workflows  
- Full audit logging  
- **Executive‑level reporting** (NEW)  
- Weekly summaries and dashboards (NEW)

The Reporting & Analytics Layer completes the feedback loop, transforming operational insights into strategic intelligence.

