# Workflow 02 — Diagnostics

## Overview
This workflow runs weekly diagnostics across **Support** and **Community** data sources.  
It aggregates recent events, sends them to OpenAI (gpt-4.1) for pattern and trend analysis, formats a human-readable report, posts it to Slack for the CMO and Content Lead, and logs the run via the central Logging Engine.

It is designed as a **read-only, analytical** workflow. It does NOT modify tickets or community data.

---

## Trigger

**Cron — Weekly (Monday @ 07:00)**

This can be adjusted depending on the organisation's cadence (e.g., daily for high-volume environments).

---

## Data Sources

### 1. Support Data
- Pulled from: `$env.SUPPORT_API_URL`
- Method: HTTP GET
- Query: `limit=100` (most recent 100 events)
- Purpose: capture recent tickets, issues, escalations.

### 2. Community / Social Data
- Pulled from: `$env.COMMUNITY_API_URL`
- Method: HTTP GET
- Query: `limit=100`
- Purpose: capture recent public or semi-public sentiment and narratives.

Both endpoints are assumed to be pre-filtered or pre-aggregated by date on the backend side.

---

## OpenAI Analysis

The workflow uses:

- **Model:** `gpt-4.1`
- **Role:** CX diagnostics assistant
- **Input:** a combined text block containing:
  - `SUPPORT_EVENTS: <JSON of recent support>`  
  - `COMMUNITY_EVENTS: <JSON of recent community messages>`

The system message instructs the model to:

- Extract recurring patterns  
- Identify emerging issues  
- Describe sentiment trends  
- Highlight potential risks  
- Present findings as structured bullet points with clear headings.

Temperature is set to **0.2** for stability and repeatability.

---

## Slack Output

After obtaining the OpenAI summary, the workflow:

1. Extracts the assistant text from the OpenAI response.
2. Builds a Slack message body:

   ```text
   *Weekly CX Diagnostics Report*

   <summary from OpenAI>
   ```

3. Sends it to the channel:

   - `channel: $env.SLACK_CHANNEL_DIAGNOSTICS`

Intended audience: CMO, Content Lead, CX leadership, and any stakeholders in the diagnostics channel.

---

## Logging

Finally, the workflow posts a log entry to:

- `$env.LOGGING_WEBHOOK_URL`

With payload:

```json
{
  "event_type": "diagnostics_run",
  "workflow": "02_Diagnostics",
  "timestamp": "<now>",
  "success": true,
  "severity": "low",
  "actor": "system",
  "payload": { ... }  // Slack-ready summary text
}
```

This ensures that the existence and timing of diagnostics runs are fully auditable.

---

## Error Handling & Limitations

- External HTTP calls (`Support`, `Community`, `OpenAI`, `Slack`) use `continueOnFail: true`.
- If one of the data sources fails:
  - The combined input may be partially empty.
  - OpenAI may still generate a useful summary from the remaining source.
- If OpenAI fails or returns an unexpected shape:
  - The summary text falls back to: `No diagnostic summary available (OpenAI response missing or invalid).`
- This workflow assumes:
  - Support and Community APIs return JSON payloads.
  - Proper authentication is handled by API gateways or environment-level configuration.

For stricter error handling, you may later introduce:
- Dedicated error routes
- Slack alerts on diagnostics failure
- More granular logging of partial failures.

---

## Environment Variables

| Variable                   | Description                                             |
|---------------------------|---------------------------------------------------------|
| SUPPORT_API_URL           | Endpoint providing recent support events               |
| COMMUNITY_API_URL         | Endpoint providing recent community/social messages    |
| OPENAI_API_KEY            | OpenAI authentication key                              |
| SLACK_BOT_TOKEN           | Slack bot token used to post the report                |
| SLACK_CHANNEL_DIAGNOSTICS | Slack channel ID/name for diagnostics reports          |
| LOGGING_WEBHOOK_URL       | URL for the central logging engine                     |

---

## Safe Modification Guidelines

- Do not remove the `Prepare AI Input` step: it ensures data is safely merged and serialized.
- When changing the `limit` parameter, consider OpenAI token limits.
- If switching models (e.g., from `gpt-4.1` to `gpt-4o-mini`), revalidate the quality of summaries.
- If changing the Slack channel, ensure the bot user has permission to post there.
- If you add more data sources (e.g., NPS, CSAT surveys), extend the `Prepare AI Input` function accordingly.

---

End of Document
