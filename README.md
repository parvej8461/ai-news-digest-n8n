# AI-Powered Competitor News Monitor & Weekly Digest

An n8n workflow that automatically collects the latest AI/industry news from
trusted RSS sources, summarizes it with an LLM, and posts a clean, scannable
digest to Slack — on a weekly schedule, with zero manual review.

---

##  Overview

I designed the workflow to automate the collection and summarization of AI
news from multiple trusted RSS sources. I chose **MarkTechPost** for
industry updates and **Berkeley AI Research (BAIR)** for research-focused
articles to provide a balanced mix of practical developments and academic
advances. After merging and limiting the latest articles, I aggregate them
into a single payload and use **Google Gemini** to generate a concise digest
highlighting the most important stories. The summarized digest is then
automatically posted to Slack, making it easy for a team to stay informed
without manually reviewing multiple websites.

---

##  Architecture

```
Schedule Trigger
        │
        ▼
RSS Feed (MarkTechPost)
RSS Feed (Berkeley AI)
        │
        ▼
      Merge
        │
        ▼
  Limit (Top 10)
        │
        ▼
    Aggregate
        │
        ▼
  Google Gemini
        │
        ▼
      Slack
```

---

##  Why These Design Choices

| Component | Reason |
|---|---|
| **Schedule Trigger** | Fully automated weekly execution without manual intervention. |
| **RSS Feed Read** | Simple, lightweight way to collect AI news. |
| **Merge** | Combines articles from multiple sources into one stream. |
| **Limit** | Prevents overwhelming the LLM with too many articles. |
| **Aggregate** | Sends one request to the LLM instead of multiple requests, reducing API calls. |
| **Google Gemini** | Generates concise summaries in natural language. |
| **Slack** | Delivers information where teams already collaborate. |

---

##  AI Prompt Used

```
You are an AI news editor.
Summarize the latest AI news articles into a Slack-friendly digest.

Requirements:
- Select the five most important stories.
- For each story include:
  • Title
  • One-sentence summary
  • Why it matters
  • Article URL if available

Keep the output concise, easy to scan, and suitable for posting in a Slack channel.
```

---

##  AI Tools Used

**Tool:** ChatGPT (GPT-5.5)

**Purpose:**
- Designed the workflow architecture
- Improved the LLM prompt
- Reviewed node configuration
- Helped troubleshoot Slack expressions
- Assisted in preparing this documentation

---

##  Setup

1. Import the workflow JSON into your n8n instance (**Workflows → Import from File**).
2. Add credentials:
   - **Slack** — OAuth connection to your workspace, select the target channel.
   - **Google Gemini** — API key from Google AI Studio.
3. Confirm the two RSS feed URLs (MarkTechPost, BAIR) still resolve correctly.
4. Activate the workflow — it will run automatically on the configured schedule.
5. (Optional) Trigger a manual execution once to verify the digest formatting in Slack before relying on the schedule.

---

##  Possible Improvements

- Removing duplicate articles across sources
- Ranking stories by importance before summarization
- Supporting additional RSS sources
- Storing historical digests in a database for search and analysis

---

