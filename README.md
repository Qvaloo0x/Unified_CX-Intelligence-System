# CX Intelligence - n8n Workflows

This repository contains a collection of **n8n workflows** for managing and automating CX intelligence, health checks, diagnostics, review cycles, and reporting — all built on modular, reusable components.

---

## 📂 Folder Structure

```
/workflows               ← All .json workflows (importable into n8n)
/assets                  ← Diagrams and architecture visuals
/docs                    ← Setup guides for Slack, Asana, Discord, etc.
/README.md               ← This file
/LICENSE.md              ← MIT License
/CHANGELOG.md            ← Version history
/.gitignore              ← Excludes secrets, logs, etc.
```

---

## 🚀 Getting Started

### 1. Requirements

- ✅ [n8n (Self-hosted)](https://docs.n8n.io/hosting/overview/) (v1.38+ recommended)
- ✅ API access for:
  - Slack
  - Asana
  - Google Sheets
  - OpenAI

---

### 2. Importing Workflows

1. Open your [n8n Editor](https://n8n.io).
2. Go to `Workflows` > `Import from File`.
3. Select any `.json` file from the `/workflows/` folder.
4. Update credentials in each node (`HTTP Request`, `Slack`, `Google Sheets`, etc.).
5. Adjust any URLs like:

   ```
   https://your-n8n-instance/webhook-test/...
   ```

   Replace with your actual webhook URLs or set them via environment variables.

---

### 3. Environment Variables (.env)

Make sure to define these in your `.env` file or via n8n UI:

```bash
OPENAI_API_KEY=your-openai-key
SLACK_BOT_TOKEN=your-slack-token
ASANA_ACCESS_TOKEN=your-asana-token
ASANA_PROJECT_ID_COMM=your-asana-project-id
ASANA_COPYWRITER_ASSIGNEE_ID=asana-user-id
SLACK_CHANNEL_COPYWRITER=#copywriters
SLACK_CHANNEL_REVIEW=#review
SLACK_CHANNEL_EWS=#ews-alerts
SLACK_CHANNEL_PUBLISH=#publish
SLACK_CHANNEL_DIAGNOSTICS=#diagnostics
GS_LOGGING_SHEET_ID=google-sheet-id
LOGGING_WEBHOOK_URL=https://yourdomain.com/webhooks/logging
```

---

## 🧠 Workflows Overview

| Workflow                         | Purpose                                               |
|----------------------------------|-------------------------------------------------------|
| `00_System_HealthCheck`          | Daily integration tests for Slack, Asana, Sheets     |
| `01_Logging_Engine`             | Centralized logging to Sheets + webhook              |
| `02_Diagnostics`                | Weekly report with GPT summary from support data     |
| `03_EWS_Alert_HumanDecision`    | Human-in-the-loop decision for EWS alerts            |
| `04_Copywriter_Task_Creation`   | Auto-creates task in Asana + Slack alert             |
| `05_Content_Review_Approval`    | Sends Slack buttons to review/approve content        |
| `06_Final_Publishing_Confirmation` | Manual webhook confirming publication              |
| `07_Weekly_Report`              | Pulls log data weekly and summarizes in Slack        |
| `98_Stress_Test_Simulator`      | Simulates 500+ parallel items and success rates      |
| `99_Test_Unified_CX_Chain`      | End-to-end simulation of all CX webhook steps        |

---

## 🔒 Security Note

This repo:
- **Does not contain any hardcoded credentials**.
- Uses `{{$env.VARIABLE}}` for secure API tokens.
- `.gitignore` ensures secrets like `.env`, `credentials.json`, backups, etc. are ignored.

---

## 📜 License

MIT License. See [LICENSE.md](./LICENSE.md) for details.

---

## 🙌 Contributing

Pull requests welcome! Please open an issue first for major changes. Contributions must follow the structure and logging format used in existing workflows.

---