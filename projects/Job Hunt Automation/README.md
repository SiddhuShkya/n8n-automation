# Job Hunt Automation

An n8n (self-hosted) workflow that automatically tracks job applications by reading your Gmail inbox, using an AI model to classify and extract application details, and logging/updating them in a Google Sheet.

No more manually copy-pasting company names and statuses into a spreadsheet every time a recruiter replies.

## How it works

```
Gmail Trigger  →  AI Classifier/Extractor  →  Filter  →  Google Sheets (Append or Update)
   (new email)      (is this job-related?      (drop        (one row per
                      company / role / status)   irrelevant   application,
                                                   emails)     updates over time)
```

An email thread that goes `applied → interview invite → rejection` doesn't create three rows — it updates the *same* row as the application's status evolves, matched on Company + Role.

## Stack

| Component | Role | Cost |
|---|---|---|
| [n8n](https://n8n.io) (self-hosted) | Workflow orchestration | Free |
| Gmail API | Read incoming application emails | Free |
| Google Sheets API | Store/update tracked applications | Free |
| Gemini API (Flash / Flash-Lite) *or* Ollama (local) | Classify emails & extract structured fields | Free |

## Prerequisites

- A running self-hosted n8n instance
- A Google Cloud project with the **Gmail API** and **Google Sheets API** enabled
- OAuth2 credentials configured in n8n for Gmail and Google Sheets
- An AI provider credential in n8n — either:
  - Google Gemini API key (free tier), or
  - A local model running via Ollama

## Setup

### 1. Google Cloud & credentials
1. Create/select a project in [Google Cloud Console](https://console.cloud.google.com).
2. Enable the **Gmail API** and **Google Sheets API**.
3. Create OAuth2 credentials and add your n8n instance's redirect URI as an authorized redirect URI.
4. In n8n, go to **Credentials → New** and add:
   - Gmail OAuth2
   - Google Sheets OAuth2
   - Gemini API key (or Ollama credential, if running locally)

### 2. Create the tracker sheet
Create a Google Sheet with these columns:

| Company | Role | Status | Date Applied | Last Update | Email Subject | Thread Link |
|---|---|---|---|---|---|---|

This sheet is the single source of truth the workflow reads from and writes to.

### 3. (Recommended) Create a Gmail label/filter
In Gmail, create a filter that labels job-application-related emails (e.g. `Job Apps`) so the trigger only scans relevant mail instead of your whole inbox — keeps things fast and keeps API/LLM usage low.

### 4. Build the n8n workflow
1. **Gmail Trigger** — poll every 15 min (or your preference), scoped to the `Job Apps` label or a search query like `label:inbox newer_than:1d`.
2. **AI node** (Agent / Information Extractor) — prompt it to return structured JSON:
   ```json
   {
     "is_job_related": true,
     "company": "Acme Corp",
     "role": "Backend Engineer",
     "status": "interview",
     "confidence": 0.92
   }
   ```
   `status` should be one of: `applied`, `interview`, `assessment`, `offer`, `rejected`, `other`.
3. **IF node** — continue only when `is_job_related == true` and `confidence` is above your threshold (e.g. `0.7`).
4. **Google Sheets node** — operation `Append or Update`, matched on `Company + Role`.

### 5. Test before going live
Temporarily widen the Gmail trigger's date range (e.g. `newer_than:30d`) to backfill your sheet from historical emails and sanity-check the AI's extraction accuracy. Once it looks right, narrow the query back and activate the workflow.

## Notes & gotchas

- **Company name normalization** — "Google" vs "Google LLC" vs a recruiter's personal domain can break the Company+Role match key. Consider having the AI extract a normalized company slug, or match on Gmail thread ID instead.
- **Gemini free tier data usage** — inputs/outputs on the free tier may be used by Google to improve their models. Fine for most job-hunt emails, but worth knowing.
- **Billing trap** — if you ever enable billing on the same Google Cloud project (for another purpose), it removes that project's Gemini free tier entirely. Use a separate project if you want to keep this workflow free.
- **False positives** — newsletters or marketing emails that mention "your application" can trip up a loose prompt. Feed the AI node a few real examples from your own inbox (of both job-related and not) to sharpen classification.

## Roadmap ideas

- [ ] Weekly digest (Slack/email) summarizing new applications and status changes
- [ ] Auto-flag applications with no response after N days
- [ ] Dashboard/chart view of application funnel (applied → interview → offer)