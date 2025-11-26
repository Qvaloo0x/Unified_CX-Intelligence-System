# Changelog (Updated)

All notable changes to this project are documented in this file.  
This project follows **Semantic Versioning (MAJOR.MINOR.PATCH)**.

---

## [1.1.0] – 2025-11-21
### Added
- **Reporting & Analytics Layer** introduced.
- Added new documentation:
  - `Metrics.md`
  - `Weekly_Report.md`
  - `Dashboard_Graphics_Guide.md`
- Added new workflow:
  - `07_Weekly_Report.json` (weekly executive summary)
- Added dashboard template:
  - `CX_Intel_Dashboard_Template.xlsx`
- Updated system documentation:
  - `Architecture.md` → now includes Reporting Layer
  - `System_Overview.md` → integrated new metrics & reporting sections
  - `Dataflows.md` → expanded dataflow diagrams for reporting
  - `Glossary.md` → added terms for Weekly Report & analytics

### Changed
- `README.md` updated with:
  - Reporting Layer overview
  - New repository structure sections
  - Dashboard references
  - Weekly report introduction

### Fixed
- Improved consistency across documentation and diagrams.
- Updated architectural flow to include reporting outputs.

---

## [1.0.0] – 2025-11-20
### Added
- Initial release of the **Unified CX Intelligence System (Human-Controlled MVP)**.
- Included the full set of n8n workflows:
  - `00_System_HealthCheck.json`
  - `01_Logging_Engine.json`
  - `02_Diagnostics.json`
  - `03_EWS_Alert_HumanDecision.json`
  - `04_Copywriter_Task_Creation.json`
  - `05_Content_Review_Approval.json`
  - `06_Final_Publishing_Confirmation.json`
- Added documentation suite in `/docs/`
- Added diagrams in `/assets/diagrams/`
- Added root files: README, LICENSE, CONTRIBUTING, CHANGELOG

