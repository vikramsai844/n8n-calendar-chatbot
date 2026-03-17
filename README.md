# 🗓️ n8n Calendar AI Chatbot

A fully functional AI-powered Google Calendar chatbot built with **n8n**, **Google Gemini**, and a custom frontend. Talk to it in natural language to create and retrieve calendar events instantly.

**Live Demo → [vikramsai844.github.io/n8n-calendar-chatbot](https://vikramsai844.github.io/n8n-calendar-chatbot/)**

---

## ✨ Features

- 📅 **Create events** using natural language — *"Book a dentist at 3pm this Friday"*
- 🔍 **Retrieve events** — *"What do I have today?"*, *"Show my schedule this week"*
- 🧠 **Conversation memory** — remembers context within the same session
- ⚡ **Smart defaults** — auto-calculates end times (movies = 2hr, meetings = 1hr)
- 🌐 **Works from any device** — hosted on GitHub Pages
- 🔒 **No data stored** — all processing happens in your own n8n instance

---

## 🛠️ Tech Stack

| Layer | Technology |
|-------|-----------|
| Frontend | HTML, CSS, JavaScript (single file) |
| Automation | n8n (self-hosted) |
| AI Model | Google Gemini via n8n LangChain |
| Calendar | Google Calendar API (OAuth2) |
| Tunnel | ngrok (exposes local n8n publicly) |
| Hosting | GitHub Pages |

---

## 🏗️ Architecture

```
User (Browser)
     ↓
GitHub Pages (index.html)
     ↓  fetch POST
ngrok public URL
     ↓  tunnel
n8n localhost:5678
     ↓
Calendar AI Agent (Gemini + LangChain)
     ↓
Google Calendar API
```

---

## 🚀 Setup Guide

### Prerequisites
- [n8n](https://n8n.io) installed and running locally
- Google Cloud project with Calendar API enabled
- [ngrok](https://ngrok.com) account (free)
- Google Gemini API key

---

### Step 1 — Import the n8n Workflow

1. Open n8n → click **+** New Workflow
2. Click the **⋮** menu → **Import from JSON**
3. Paste the workflow JSON (see `/workflow/calendar-workflow.json`)
4. Connect your **Google Calendar** and **Google Gemini** credentials

---

### Step 2 — Configure Credentials

**Google Calendar:**
- n8n → Settings → Credentials → New → Google Calendar OAuth2
- Authorize with your Google account

**Google Gemini:**
- n8n → Settings → Credentials → New → Google PaLM API
- Paste your Gemini API key

---

### Step 3 — Enable CORS

In n8n → open the **"When chat message received"** node → Options → **Allowed Origins (CORS)** → set to:
```
*
```
Save → deactivate → re-activate the workflow.

---

### Step 4 — Start ngrok

```bash
# Authenticate (first time only)
ngrok config add-authtoken YOUR_AUTHTOKEN

# Start tunnel to n8n
ngrok http 5678
```

Copy the forwarding URL — it looks like:
```
https://xxxx-xx-xx-xxx-xx.ngrok-free.app
```

---

### Step 5 — Connect the Chatbot

1. Open the chatbot → click **⚙** (settings icon)
2. Paste your full webhook URL:
```
https://xxxx.ngrok-free.app/webhook/YOUR-WEBHOOK-ID/chat
```
3. Click **Save & Connect**

---

## 💬 Example Commands

| You say | What happens |
|---------|-------------|
| `Book a dentist at 3pm this Friday` | Creates event Friday 15:00–16:00 |
| `Add a team meeting tomorrow morning` | Creates event next day 09:00–10:00 |
| `What do I have today?` | Lists all today's events |
| `Show my schedule this week` | Lists events Mon–Sun |
| `Get the dentist appointment details` | Searches by keyword |

---

## 📁 Project Structure

```
n8n-calendar-chatbot/
├── index.html          ← entire chatbot frontend (single file)
├── README.md           ← this file
```

---

## ⚠️ Important Notes

- **ngrok free URL changes** every time you restart ngrok. Update the webhook URL in chatbot settings (⚙) each time.
- To get a **permanent ngrok URL**, claim a free static domain at [dashboard.ngrok.com/domains](https://dashboard.ngrok.com/domains)
- Keep the ngrok terminal **open** while using the chatbot — closing it breaks the connection
- The Google OAuth token may expire after 7 days if your Google Cloud app is in **Testing** mode — re-authorize in n8n credentials when this happens

---

## 👤 Author

**Vikram**
AI Automation Consultant — Caddam Technology Pvt Ltd

---

## 📄 License

MIT License — free to use and modify.
