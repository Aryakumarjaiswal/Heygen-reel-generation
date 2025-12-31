# 🎥 Instagram to AI Avatar Repurposing Workflow (n8n)

####  Demo: https://drive.google.com/file/d/1fwel170y11lRth4ub3BX9O_DFr9uExVj/view?usp=sharing
**Automate the conversion of viral Instagram Reels into original, AI-hosted "Podcast" style videos.**

This n8n workflow scrapes high-performing Instagram content, analyzes the video context using Computer Vision, rewrites the script to be unique, and renders a new video using an AI Avatar via HeyGen.

---

## 🚀 How It Works

1.  **Scrape & Filter:** Uses **Apify** to scrape Instagram Reels from specific accounts or hashtags. It filters for high-quality content (Likes > 700) and ensures the media type is a video.
2.  **Multimodal Analysis:** Downloads the video and sends it to **Google Gemini (Vision)** to extract key themes, emotional tone, quotes, and central text.
3.  **Creative Rewriting:** **OpenAI (GPT-4o-mini)** acts as a "Cinematic Director," rewriting the insights into a fresh, spoken-word monologue suitable for a podcast host.
4.  **Video Generation:** Sends the generated script and styling instructions to **HeyGen** to render a vertical (9:16) AI Avatar video.
5.  **Logging:** All data (scraped metrics, analysis, and job IDs) is logged to **Google Sheets**.

---

## 🛠️ Tech Stack & Integrations

*   **[n8n](https://n8n.io/):** The workflow orchestration engine.
*   **[Apify](https://apify.com/):** For Instagram scraping (Actor: `instagram-scraper`).
*   **[Google Gemini](https://deepmind.google/technologies/gemini/):** For video analysis and transcription using Gemini 2.5pro.
*   **[OpenAI](https://openai.com/):** Scriptwriting and metadata generation using OpenAI 4o mini.
*   **[HeyGen](https://www.heygen.com/):** AI Avatar video generation using heygen api.
*   **Google Workspace:** Sheets (Logging), Drive (Storage), Gmail (Notifications).

---

## 📋 Prerequisites

Before importing this workflow, ensure you have the following accounts and API keys:

1.  **n8n Instance:** Self-hosted or Cloud.
2.  **Apify:** API Token with access to Instagram Scrapers.
3.  **Google Cloud Project:**
    *   Enabled APIs: Google Sheets, Drive, Gmail, Generative Language API (Gemini).
    *   OAuth Credentials configured in n8n.
4.  **OpenAI:** API Key with access to `gpt-4o-mini`.
5.  **HeyGen:** API Key (requires a paid plan with API access).

---

## ⚙️ Setup Instructions

### 1. Import the Workflow
1.  Download the `workflow.json` file from this repository.
2.  Open your n8n dashboard.
3.  Click **"Add Workflow"** -> **"Import from..."** -> **"Local File"**.

### 2. Configure Credentials
You must set up the following credentials in n8n:
*   `Google Sheets OAuth2 API`
*   `Google Gemini(PaLM) Api`
*   `OpenAI API`
*   `Google Drive OAuth2 API` (Optional, if enabling upload)
*   `Gmail OAuth2 API` (Optional, if enabling approval)

### 3. Google Sheets Setup
Create a new Google Sheet (or use an existing one) and create three tabs. You will need to replace the **Spreadsheet ID** and **Sheet Names/IDs** in the Google Sheets nodes with your own.

**Suggested Headers:**
*   **Sheet 1 (Scrapped Data):** `type`, `caption`, `hashtags`, `url`, `likesCount`, `videoUrl`
*   **Sheet 2 (Generated Data):** `video_id`, `status`
*   **Sheet 3 (Final Output):** `fileId`, `directViewLink`, `caption`, `hashtags`

### 4. Update API Keys
**⚠️ Important:** Some keys in the template are hardcoded placeholders. You **must** update them:

*   **Node:** `Instagram Hashtag Scraper`
    *   Update the `token=` query parameter in the URL field with your Apify Token.
*   **Node:** `HTTP Request2` (HeyGen Generation)
    *   Go to **Headers**.
    *   Replace the value `apikeysemujhereplacekaro` with your actual HeyGen API Key.

---

## 🚧 Workflow Status & Customization

### Active Logic
The current version of the workflow stops after **initiating** the video generation job with HeyGen. It successfully:
*   ✅ Scrapes Data
*   ✅ Analyzes Content
*   ✅ Writes Script
*   ✅ Sends Job to HeyGen
*   ✅ Logs Job ID

### Disabled Logic (Optional)
The following nodes are present but **disabled** (greyed out) in the workflow. You can enable them to create a full end-to-end automation:
*   **Polling Loop:** Checks HeyGen every 2 minutes to see if the video is done.
*   **Google Drive Upload:** Saves the MP4 to your Drive.
*   **Gmail Approval:** Sends an email to a manager to approve/reject the video before posting.
*   **Facebook/Instagram Posting:** Automatically uploads the video to social media.

To enable these, simply select the nodes in the n8n canvas and toggle them to **Active**.

---

## 📝 License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

---
