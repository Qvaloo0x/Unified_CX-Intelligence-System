# Contributing (Updated)

## Unified CX Intelligence System — Contribution Guidelines (Updated for Reporting Layer)

Thank you for contributing to the **Unified CX Intelligence System (Human‑Controlled MVP)**.  
This updated guideline includes new rules and responsibilities introduced with the **Reporting & Analytics Layer**, `Metrics.md`, dashboard tools, and the `07_Weekly_Report` workflow.

---

# 1. Code of Conduct

Contributors must follow professional conduct at all times:
- Clear communication
- Respect for governance rules
- No introduction of unsafe automation
- English-only documentation

---

# 2. What You Can Contribute

### Allowed Contributions
- Improvements to ingestion and diagnostics workflows
- Enhancing the reporting layer
- Refining dashboard logic and metrics
- Improving documentation or diagrams
- Fixes to system reliability or stability
- Enhancements to logging completeness
- Extensions to read‑only platform integrations

### Forbidden Contributions
❌ Auto‑publishing  
❌ Auto‑replying to customers  
❌ Any outbound messaging to external platforms  
❌ Bypassing human‑approval layers  
❌ Reducing logging or auditability  
❌ AI‑driven content publication  

All contributions must preserve **human‑controlled governance**.

---

# 3. Repository Structure (Updated)

```
/workflows
    00_System_HealthCheck.json
    01_Logging_Engine.json
    02_Diagnostics.json
    03_EWS_Alert_HumanDecision.json
    04_Copywriter_Task_Creation.json
    05_Content_Review_Approval.json
    06_Final_Publishing_Confirmation.json
    07_Weekly_Report.json        <-- NEW

/docs
    Architecture.md              <-- Updated
    System_Overview.md           <-- Updated
    Dataflows.md                 <-- Updated
    Glossary.md                  <-- Updated
    Metrics.md                   <-- NEW
    Weekly_Report.md             <-- NEW
    Dashboard_Graphics_Guide.md  <-- NEW

/assets/diagrams
    *.png, *.mmd (diagrams)

/CX_Intel_Dashboard_Template.xlsx <-- NEW

LICENSE.md
README.md (updated)
CHANGELOG.md (updated)
```

---

# 4. Workflow Contribution Rules

### 4.1 Workflows Must Remain Modular
Each workflow must perform **one purpose only**.

### 4.2 Logging is Mandatory
Every workflow must log:
- Workflow name
- Event details
- Timestamp
- Output
- Errors
- Human decisions (if any)

### 4.3 Human Approval Must Be Preserved
High‑risk flows must route through Slack:
- Approve  
- Reject  
- Clarify  

### 4.4 Least Privilege Rule
API tokens must use minimum necessary permissions.
No contributor may increase platform permissions without approval.

### 4.5 Read‑Only Ingestion Only
External platform integration cannot include POST, DM, moderation, or write actions.

---

# 5. Reporting Layer Contribution Rules (NEW)

The Reporting & Analytics Layer introduces new contribution requirements.

### 5.1 Metrics Consistency
If you update Diagnostics, Logging, or EWS workflows:
- Update `Metrics.md`
- Ensure new fields appear in the dashboard template
- Update the Weekly Report workflow if required

### 5.2 Dashboard Safety
Dashboards must:
- Use Sheets/BI platforms only
- Not expose personal data publicly
- Be aligned with CMO‑level visibility

### 5.3 Weekly Report Governance
Contributors modifying `07_Weekly_Report.json` must:
- Maintain weekly cron schedule
- Preserve safe Slack summaries
- Ensure no sensitive data is included
- Keep executive summary clean and concise

---

# 6. Documentation Standards

### All documentation must be:
- Complete (no placeholders)
- Clear and professional
- Written in English
- Updated when workflows change
- Synchronized across README, docs, and CHANGELOG

### Diagram Requirements
- Use Mermaid or PNG assets under `/assets/diagrams/`
- Diagrams must reflect reporting and workflow changes

---

# 7. Commit Message Guidelines

Use conventional commits:

```
feat: add new severity scoring logic
fix: correct Slack EWS trigger formatting
docs: update Reporting Layer documentation
chore: refresh dashboard template
refactor: optimize Diagnostics function node
```

Avoid vague messages like:
- “update”
- “fixes”
- “misc changes”

---

# 8. Pull Request Requirements

Each PR must include:

### 8.1 Description
- What changed  
- Why it changed  
- Which workflows or docs were impacted  

### 8.2 Screenshots
Only required for workflow UI changes.

### 8.3 Safety Review
Show how changes comply with:
- Zero auto-publishing  
- Human-in-the-loop design  
- Read-only ingestion  
- Full audit logging  

### 8.4 Documentation Updates
Any workflow change must include:
- README updates if applicable  
- Docs updates under `/docs`  
- CHANGELOG entry  

---

# 9. Versioning Policy

Semantic Versioning:
- MAJOR: Breaking changes
- MINOR: New features (ex: reporting layer)
- PATCH: Fixes and optimizations

All versions must be added to `CHANGELOG.md`.

---

# 10. Issue Reporting

Use GitHub Issues for:
- Workflow errors
- Dashboard inconsistencies
- Reporting inaccuracies
- Documentation gaps
- Governance violations

Include:
- Expected behavior  
- Current behavior  
- Logs or screenshots if possible  

---

# 11. Summary

Contributors must preserve the system’s:
- Safety  
- Governance  
- Auditability  
- Modularity  
- Reporting accuracy  

The newly added Reporting Layer significantly enhances strategic visibility.  
Contributions must maintain alignment across Diagnostics, Logging, and Reporting to ensure consistent intelligence delivery.

