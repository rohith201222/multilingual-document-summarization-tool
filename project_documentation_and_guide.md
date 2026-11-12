# Project Documentation: Multilingual Document Summarization Tool

## 1. Architecture Overview
This automation pipeline uses **n8n** to ingest newly uploaded PDF documents from Google Drive, extract raw text, identify the document language, summarize the content using an LLM, translate the summary into six languages (**English, Hindi, Spanish, French, German, Japanese**), and export a formatted report into Google Docs.

**Workflow Sequence:**
`Google Drive Trigger` $ightarrow$ `Download file` $ightarrow$ `Extract from File (PDF)` $ightarrow$ `Language Detection` $ightarrow$ `Summarization` $ightarrow$ `Translations (EN, HI, ES, FR, DE, JA)` $ightarrow$ `Create a document (Google Docs)` $ightarrow$ `Update a document (Google Docs)`

---

## 2. Setup Instructions

### Step A: Google Drive Folders
1. Open Google Drive (`drive.google.com`).
2. Create an input folder named: `Document_Uploads_For_Summarization`
3. Create an output folder named: `Summary_Folder`

### Step B: Import Workflow into n8n
1. Open n8n (Cloud or Self-Hosted) and click **Create Workflow**.
2. Click the top-right **Options (`...`)** menu and select **Import from File**.
3. Choose `n8n_multilingual_summarization_workflow.json`.

### Step C: Configure Credentials
1. **Google Drive & Google Docs (OAuth2):**
   - In Google Cloud Console, enable **Google Drive API** and **Google Docs API**.
   - Create an OAuth 2.0 Client ID and paste the n8n **OAuth Redirect URL** into *Authorized redirect URIs*.
   - Under *OAuth consent screen*, add your email under **Test Users** (or publish the app to Production so tokens do not expire after 7 days).
   - Connect your Google account inside the Google Drive and Google Docs nodes in n8n.
2. **LLM Chat Models (Free Tier Friendly):**
   - Generate a free API key from **Google AI Studio** (`aistudio.google.com`) or use an OpenAI key (`platform.openai.com`).
   - Attach the credential to all 8 Chat Model sub-nodes (`gemini-2.5-flash` or `gpt-4o-mini`).

---

## 3. Testing Guide
1. Upload a sample multi-page PDF document into `Document_Uploads_For_Summarization` in Google Drive.
2. In n8n, open the **Google Drive Trigger** node and click **Fetch Test Event** to verify the file metadata loads.
3. Click **Test Workflow** (or **Execute Workflow**) at the bottom of the n8n canvas.
4. Confirm each node turns green sequentially.
5. Open the `Summary_Folder` in Google Drive and verify that a new Google Doc has been created containing the document title, detected language, and all six translated summaries.

---

## 4. Troubleshooting Checklist

| Issue / Error | Likely Cause | Resolution |
| :--- | :--- | :--- |
| `Error 400: redirect_uri_mismatch` | Callback URL in Google Cloud does not match n8n. | Copy the exact OAuth Redirect URL from n8n and save it in Google Cloud Console under *Authorized redirect URIs*. |
| `Access blocked: App hasn't been verified` | Google Cloud OAuth screen is in Testing mode without your email listed. | Add your Gmail address under **Test Users** in the Google Cloud OAuth consent screen. |
| `Invalid_grant` after 7 days | Testing-mode OAuth tokens expire weekly. | Click **Publish App** on the Google Cloud OAuth consent screen to move it to Production status. |
| `401 Unauthorized` / `402 Quota` on OpenAI | Trailing space in API key or $0 credit balance. | Re-paste key without spaces, or switch Chat Model nodes to **Google Gemini (`gemini-2.5-flash`)** using a free Google AI Studio key. |
| Empty text output after PDF node | Uploaded PDF is a scanned image rather than selectable text. | Test with a text-based PDF, or add an OCR step prior to text extraction. |
