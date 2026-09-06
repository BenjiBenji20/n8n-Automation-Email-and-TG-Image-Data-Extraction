# 🚀 Built my first two n8n automation workflows — sharing what I learned!

As part of a technical assessment, I designed and built two end-to-end automation pipelines using n8n, Google Workspace APIs, Telegram, and Google Gemini. Coming in with zero prior n8n experience, this was a solid crash course in workflow automation architecture.

## Workflow 1: Multi-Label Gmail → Sheets & Drive
📧 Polls Gmail across multiple labels on a schedule
📊 Extracts sender details into Google Sheets automatically
📁 Downloads attachments to Drive, auto-organized by Label → Date → Sender — with duplicate-folder prevention via a search-before-create pattern

### Architecture

<img width="1680" height="283" alt="Image" src="https://github.com/user-attachments/assets/ad6373db-c7f9-47aa-99e0-1c9be9c2475a" />

**Watch Demo**: [Workflow 1 Demo](https://youtu.be/PYuRil0SWfY)

## Workflow 2: Telegram Receipt Processing with AI
🤖 Accepts receipt photos sent via Telegram
🧠 Uses Google Gemini's vision capabilities to extract merchant name, amount, and date
📊 Logs structured data to Google Sheets
📁 Archives original photos to Drive, organized by submission date

### Architecture

<img width="1712" height="412" alt="Image" src="https://github.com/user-attachments/assets/0aea9a03-b079-4ce2-9500-cfc1b84f6f6d" />

**Watch Demo**: [Workflow 2 Demo](https://youtu.be/LcCgv6-p99I)

## Key technical challenges I worked through:
- Handling binary data (attachments/images) correctly across multiple node types — easy to accidentally strip it without noticing
- Building idempotent folder creation in Google Drive (Search → conditional Create, to avoid duplicate folders on repeated runs)
- Looping over dynamic attachment counts (1 email could have 1 or several files)
- Prompt engineering Gemini for reliable structured JSON output from images

It's a great reminder that "no-code" automation still demands real engineering thinking — data flow, state management, and edge cases don't disappear just because you're dragging nodes instead of writing functions.

---

### Sanitized Files
1. [Job Application Assessment_ Email Extraction Workflow.json](file:///c:/Users/imper/Downloads/n8n/Job%20Application%20Assessment_%20Email%20Extraction%20Workflow.json)
2. [Job Application Assessment_ Telegram Receipt Photo Processing.json](file:///c:/Users/imper/Downloads/n8n/Job%20Application%20Assessment_%20Telegram%20Receipt%20Photo%20Processing.json)

---

### Security Risk & Explanation of Placeholders

Exposing raw identifiers and internal API keys in exported JSON source code creates severe account vulnerability risks. Below is why each key category was converted to a placeholder:

1. **Google Sheets Document IDs & Direct URLs (`documentId`, `cachedResultUrl`)**
   - **Risk**: Directly exposes private Google Sheet keys and internal URL parameters. Anyone with these IDs can view or edit sensitive spreadsheet records or discover internal document structures.
   - **Placeholder Used**: `YOUR_GOOGLE_SHEET_ID`

2. **Google Drive Root Folder IDs & URLs (`folderId`, `cachedResultUrl`)**
   - **Risk**: Hardcoded Google Drive folder IDs grant direct paths to your personal/company Drive directory structures and stored files.
   - **Placeholder Used**: `YOUR_GOOGLE_DRIVE_FOLDER_ID`

3. **Gmail Label IDs (`filters.labelIds`)**
   - **Risk**: Internal Gmail system label strings disclose private inbox classification rules and internal mail organization structure.
   - **Placeholders Used**: `YOUR_GMAIL_LABEL_ID_1`, `YOUR_GMAIL_LABEL_ID_2`, `YOUR_GMAIL_LABEL_ID_3`

4. **Telegram Webhook Tokens (`webhookId`)**
   - **Risk**: Webhook IDs form the secret HTTP endpoint route in n8n. If leaked, unauthorized actors could send forged HTTP POST requests directly to your Telegram trigger node, injecting malicious payloads into your pipeline.
   - **Placeholders Used**: `YOUR_TELEGRAM_WEBHOOK_ID_1`, `YOUR_TELEGRAM_WEBHOOK_ID_2`

5. **n8n Credential Account Identifiers (`credentials.<type>.id`)**
   - **Risk**: Maps to specific OAuth2 / API credential entries on your local n8n instance (`gmailOAuth2`, `googleSheetsOAuth2Api`, `googleDriveOAuth2Api`, `telegramApi`, `googlePalmApi`). Exposing these IDs leaks internal system configuration details and can cause credential binding conflicts when imported on external n8n instances.
   - **Placeholders Used**: `YOUR_GMAIL_CREDENTIAL_ID`, `YOUR_GOOGLE_SHEETS_CREDENTIAL_ID`, `YOUR_GOOGLE_DRIVE_CREDENTIAL_ID`, `YOUR_TELEGRAM_CREDENTIAL_ID`, `YOUR_GEMINI_CREDENTIAL_ID`

6. **n8n Server Instance Metadata (`meta.instanceId`)**
   - **Risk**: Uniquely identifies your self-hosted or cloud n8n server instance in global telemetry.
   - **Placeholder Used**: `YOUR_N8N_INSTANCE_ID`

---
