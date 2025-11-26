
# Workflow 01 — Central Logging Engine

## Overview
This workflow serves as the unified, enterprise-grade logging service for all other workflows.  
It receives POST requests containing log payloads, validates them, writes them into a Google Sheet, forwards them to an internal webhook, and responds immediately.

All logs follow the **extended format**:
```
timestamp | workflow | event_type | success | severity | actor | payload | error
```

Severity levels allowed: **low, medium, high, critical**

---

## Trigger
### **Webhook — POST /logging_engine**
This endpoint receives structured logging events.

---

## Workflow Steps

### **1. Validate Input**
Ensures required fields exist:
- event_type  
- workflow  
- timestamp  
- success  

Validates severity using:  
`low | medium | high | critical`

### **2. Write to Google Sheets**
Appends one row per event into the sheet defined in:
`$env.GS_LOGGING_SHEET_ID`

Columns:
1. timestamp  
2. workflow  
3. event_type  
4. success  
5. severity  
6. actor  
7. payload (JSON string)  
8. error  

### **3. Forward to Internal Webhook**
Posts the full JSON payload to:
`$env.INTERNAL_LOGGING_WEBHOOK`

### **4. Respond Immediately**
Returns:
```json
{"status": "logged"}
```

---

## Environment Variables
| Variable | Description |
|---------|-------------|
| GS_LOGGING_SHEET_ID | Google Sheet ID for logs |
| GOOGLE_CREDENTIALS_ID | Google API credentials |
| INTERNAL_LOGGING_WEBHOOK | URL for secondary logging |

---

## Error Handling
- Google Sheets and internal webhook use `continueOnFail: true`
- Validation errors stop execution
- Execution always responds with HTTP 200 to avoid retries from upstream workflows

---

## Safe Modification Guidelines
- Do not remove validation logic
- Modify Google Sheet range only if adding/removing columns
- Keep the response node as the final step
