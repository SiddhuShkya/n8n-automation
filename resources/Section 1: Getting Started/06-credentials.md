# Introduction to Credentials

Credentials in n8n are authentication details (such as API keys, passwords, or OAuth tokens) provided by an external service that allow your workflows to securely connect to it. They are stored encrypted and can be reused across multiple workflows and nodes, so you don't have to re-enter sensitive information each time you build an automation.

### What credentials are

- Credentials = private info apps give you to confirm who you are.
- They let n8n "talk" to other apps (Google Sheets, Slack, Notion, etc.).
- Forms: API key, username/password, token, etc.
- Why it matters: n8n can't connect without them.

### How credentials work in n8n

- Stored safely inside n8n.
- Each workflow node uses credentials to connect to a service.
- You just select/add the right credential — no technical skills needed.

### Where to find credentials (UI)

Step-by-step:

1. Open project.
2. Select Credentials from the top menu.

**Credentials section**

