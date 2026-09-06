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