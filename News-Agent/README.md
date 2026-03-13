# News-Agent (n8n Workflow)

Automates fetching the latest tech + AI news, summarizes it via a Large Language Model (Google Gemini), and emails a concise daily briefing.

---

## 🔍 What this workflow does

When triggered, it:

1. Reads RSS feeds (AI & Tech news sources)
2. Fetches additional results from a Google Events search (via SerpAPI)
3. Merges and aggregates the collected items
4. Sends the combined content to a Gemini chat model with a fixed prompt
5. Emails the formatted summary to a recipient via Gmail

---

## 🧩 Workflow nodes (what you see in n8n)

### 1) Manual Trigger
- **Node:** `When clicking 'Execute workflow'`
- Starts the workflow when you click **Execute workflow**.

### 2) RSS Feed Readers
- **Node:** `RSS Read` (source: `https://aibusiness.com/rss.xml`)
- **Node:** `RSS Read1` (source: `http://techcrunch.com/feed/`)

These pull the latest RSS items from each feed.

### 3) HTTP Request (SerpAPI)
- **Node:** `HTTP Request`
- Queries the SerpAPI `google_events` engine for `AI Tech Events in India`.
- Adds more news/event-like content into the mix.

### 4) Merge + Aggregate
- **Node:** `Merge` – combines the 3 input streams (2 RSS feeds + HTTP request)
- **Node:** `Aggregate` – merges all items into a single array for the LLM

### 5) LLM Summary
- **Node:** `Basic LLM Chain`
- Uses **Google Gemini Chat Model** to summarize the items.
- The prompt is configured to produce a two-section “Tech Brief” with:
  - **AI NEWS** (up to 3 items)
  - **TECHNOLOGY UPDATES** (up to 3 items)

### 6) Email Output
- **Node:** `Send a message` (Gmail)
- Sends the generated summary to a configured email address.

---

## ✅ Credentials / Setup

You must configure the following credentials in n8n for the workflow to work:

- **Google Gemini (PaLM) API**
  - Credential type: `Google PaLM API` (used by `Google Gemini Chat Model` node)
  - Provide **API key** / **service account** as required.

- **Gmail OAuth2**
  - Credential type: `Gmail OAuth2` (used by `Send a message` node)
  - Authorize n8n to send email from your account.

- **SerpAPI key** (used in `HTTP Request` node)
  - Replace the `api_key` parameter in the `HTTP Request` node with your SerpAPI key.

---

## 🔧 Customization

### Change feed sources
Update the `url` fields in the `RSS Read`/`RSS Read1` nodes.

### Change the summary prompt
Edit the `text` field in the `Basic LLM Chain` node to change the instructions sent to Gemini.

### Change email recipient or subject
Modify the `sendTo` and `subject` fields in the `Send a message` node.

---

## ▶️ Running the workflow
1. Open this workflow in n8n.
2. Make sure all credentials are set and tested.
3. Click **Execute workflow** (or schedule it if desired).

---

## 📝 Notes
- The prompt explicitly tells the model not to hallucinate and to only use the provided items.
- The workflow currently selects “up to 3” items in each category in the prompt, but the selection logic is handled by the LLM, not n8n.

---

Happy summarizing! 🚀
