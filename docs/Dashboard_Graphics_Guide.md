# Dashboard_Graphics_Guide

## Unified CX Intelligence System — Dashboard Visualization Guide

This document provides a recommended set of data visualizations for the **CX Intelligence Dashboard**, based on the system’s metrics model. These charts are optimized for **CMO-level visibility**, **operational clarity**, and **trend detection**.

Each chart includes:
- Purpose  
- Type of visualization  
- Required data fields  
- Interpretation notes  
- Where to place it in the dashboard  

---

# 1. Event Volume Charts

## 1.1 Total Events Per Day (Line Chart)
**Purpose:** Track daily fluctuations in inbound signals.  
**Visualization:** Line chart  
**Required fields:**
- Date  
- Event Count  
**Interpretation:**
- Spikes = potential issues  
- Drops = quiet periods or platform outages  

---

## 1.2 Events Per Platform (Bar Chart)
**Purpose:** Compare activity across channels.  
**Visualization:** Vertical bar chart  
**Required fields:**
- Platform  
- Event Count  
**Interpretation:**
- Identifies the most active or risky channels  

---

# 2. Severity & Risk Charts

## 2.1 Severity Distribution (Donut Chart)
**Purpose:** Show proportion of Low/Mild/Moderate/High/Critical events.  
**Visualization:** Donut or pie chart  
**Required fields:**
- Severity Tier  
- Total Events  
**Interpretation:**
- Large High/Critical segments indicate urgent CX risk  

---

## 2.2 Incident Score Trend (Line Chart)
**Purpose:** Highlight changes in average severity across time.  
**Visualization:** Line chart  
**Required fields:**
- Date  
- Avg Incident Score  
**Interpretation:**
- Rising curve = worsening conditions  
- Falling curve = improvement  

---

# 3. Sentiment Charts

## 3.1 Sentiment Over Time (Stacked Area Chart)
**Purpose:** Visualize weekly or daily emotional tone trends.  
**Visualization:** Stacked area chart  
**Required fields:**
- Date  
- Positive %  
- Neutral %  
- Negative %  
**Interpretation:**
- Useful for evaluating brand perception shifts  

---

## 3.2 Sentiment by Platform (Grouped Bar Chart)
**Purpose:** Show which platforms have the most negative or positive sentiment.  
**Visualization:** Grouped bar chart  
**Required fields:**
- Platform  
- Sentiment %
**Interpretation:**  
- Helps determine platform-specific community mood  

---

# 4. EWS & Operational Performance

## 4.1 EWS Alerts Per Week (Column Chart)
**Purpose:** Track alert frequency.  
**Visualization:** Column chart  
**Required fields:**
- Week  
- EWS Alerts  
**Interpretation:**
- Spikes = operational or reputational issues  

---

## 4.2 Alert Decisions (Stacked Bar Chart)
**Purpose:** Compare approved, rejected, and clarification-needed alerts.  
**Visualization:** Stacked bar chart  
**Required fields:**
- Approved  
- Rejected  
- Clarify  
**Interpretation:**
- High rejected % = potential model calibration need  

---

## 4.3 Avg Response Time (Line or Area Chart)
**Purpose:** Measure human responsiveness to EWS alerts.  
**Visualization:** Line chart  
**Required fields:**
- Date  
- Avg Response Time (minutes)  
**Interpretation:**
- Rising = delays  
- Falling = operational improvement  

---

# 5. Content Workflow Charts (Asana)

## 5.1 Tasks Created vs Approved (Column Chart)
**Purpose:** Monitor content pipeline throughput.  
**Visualization:** Side-by-side bar chart  
**Required fields:**
- Week  
- Tasks Created  
- Tasks Approved  

---

## 5.2 Tasks Requiring Revisions (Bar Chart)
**Purpose:** Highlight revision burden.  
**Visualization:** Bar chart  
**Required fields:**
- Week  
- Tasks with Changes Required  

---

# 6. Top Topics & Patterns

## 6.1 Top 5 Topics (Horizontal Bar Chart)
**Purpose:** Show most frequent issue categories.  
**Visualization:** Horizontal bar chart  
**Required fields:**
- Topic  
- Event Count  
**Interpretation:**
- Shows where attention should be focused  

---

## 6.2 Topic Trends (Multi-Line Chart)
**Purpose:** Track changes in the top categories over time.  
**Visualization:** Multi-line chart  
**Required fields:**
- Date  
- Multiple topic counts  
**Interpretation:**
- Useful for predicting storms or emerging crises  

---

# 7. System Health

## 7.1 Workflow Success Rate (Gauge)
**Purpose:** Represent system stability visually.  
**Visualization:** Gauge chart  
**Required fields:**
- Success rate %  
**Interpretation:**
- <95% → investigate integrations  

---

## 7.2 Workflow Failures (Bar Chart)
**Purpose:** Identify error spikes.  
**Visualization:** Bar chart  
**Required fields:**
- Date  
- Number of errors  

---

# 8. Dashboard Layout (Recommended)

### **Top Section (Executive Overview)**
- Total Events (KPI tile)  
- Avg Incident Score (KPI tile)  
- % Negative Sentiment (KPI tile)  
- EWS Alerts This Week (KPI tile)

### **Middle Section (Trends)**
- Volume Per Day  
- Severity Distribution  
- Sentiment Over Time  
- Incident Score Trend  

### **Lower Section (Operations)**
- Alert Response Time  
- Tasks Created vs Approved  
- Revisions Required  

### **Bottom Section (Insights & System Health)**
- Top Topics  
- Topic Trends  
- Workflow Success Rate  
- Workflow Failures  

---

# 9. Notes for Google Sheets Implementation

### Use:
- Insert → Chart  
- Pivot tables for aggregation  
- Named ranges for dynamic updates  
- Slicers for platform selection  

### Avoid:
- Overcrowded visuals  
- Excessive colors  
- Raw data on the dashboard sheet  

---

# 10. Final Summary

These visualizations transform the CX Intelligence System into an **executive-ready analytics suite**, enabling:
- risk forecasting  
- operational improvement  
- trend understanding  
- sentiment tracking  
- platform prioritization  

This guide ensures the dashboard is clear, insightful, and aligned with CMO expectations.

