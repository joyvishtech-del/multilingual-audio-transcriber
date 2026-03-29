# 🌍 Multilingual Audio Transcriber & Summariser

An automated workflow that transcribes audio recordings in **57 languages** into English, generates a structured summary with action items, and delivers the results as a beautifully formatted email — all triggered by a simple drag-and-drop upload.

Built with **n8n** (workflow automation) and **OpenAI Whisper + GPT-4o-mini** (AI transcription & summarisation).

---

## 🎯 What it does

1. **Upload** an audio file in any of 57 supported languages through a polished web interface
2. **Select** the source language (or let Whisper auto-detect it)
3. **Transcribe** the speech into English text using OpenAI Whisper
4. **Summarise** the transcript into a structured report with action items
5. **Email** the formatted HTML result to your inbox via Gmail

---

## 🌐 Supported languages (57)

| | | | |
|---|---|---|---|
| Afrikaans | Arabic | Armenian | Azerbaijani |
| Belarusian | Bosnian | Bulgarian | Catalan |
| Chinese (Mandarin) | Croatian | Czech | Danish |
| Dutch | English | Estonian | Finnish |
| French | Galician | German | Greek |
| Hebrew | Hindi | Hungarian | Icelandic |
| Indonesian | Italian | Japanese | Kannada |
| Kazakh | Korean | Latvian | Lithuanian |
| Macedonian | Malay | Marathi | Maori |
| Nepali | Norwegian | Persian | Polish |
| Portuguese | Romanian | Russian | Serbian |
| Slovak | Slovenian | Spanish | Swahili |
| Swedish | Tagalog | **Tamil** | Thai |
| Turkish | Ukrainian | Urdu | Vietnamese |
| Welsh | | | |

> Whisper auto-detects the language, so selecting it manually is optional — but choosing it can improve accuracy for some languages.

---

## 🏗️ Architecture

```
┌─────────────────┐     ┌──────────────┐     ┌──────────────┐     ┌───────────────┐     ┌─────────┐
│   Upload Page    │────▶│   Webhook    │────▶│ OpenAI       │────▶│ OpenAI GPT    │────▶│  Gmail  │
│   (HTML/JS)      │     │   (n8n)      │     │ Whisper      │     │ Summarise     │     │  Send   │
│                  │     │              │     │ Any lang→EN  │     │ Format HTML   │     │         │
└─────────────────┘     └──────────────┘     └──────────────┘     └───────────────┘     └─────────┘
     User selects             Receives            Transcribes          Structures           Delivers
     language &               file + title         & translates         into sections        to inbox
     drops audio              + language            to English           with actions
```

---

## 📁 Project structure

```
multilingual-audio-transcriber/
├── upload-page.html                           # Frontend — drag-and-drop upload with language selector
├── multilingual-transcriber-workflow.json      # n8n workflow — import directly into n8n
├── SETUP-GUIDE.md                             # Step-by-step beginner setup instructions
├── LICENSE                                    # MIT license
├── .gitignore                                 # Git ignore rules
└── README.md                                  # This file
```

---

## 🚀 Quick start

### Prerequisites

| Tool | Purpose | Link |
|------|---------|------|
| n8n | Workflow automation | [n8n.io](https://n8n.io) (cloud or self-hosted) |
| OpenAI API key | Whisper transcription + GPT summarisation | [platform.openai.com](https://platform.openai.com/api-keys) |
| Gmail account | Sending the output email | Your existing Gmail |

### Step 1 — Import the workflow into n8n

1. Open your n8n instance
2. Click **Add workflow** → **⋯ menu** → **Import from file**
3. Select `multilingual-transcriber-workflow.json`
4. All 5 nodes will appear pre-wired on your canvas

### Step 2 — Configure credentials

1. **OpenAI API key** — Click on the Whisper node → Create New Credential → paste your API key
2. **Gmail** — Click on the Gmail node → Create New Credential → follow the OAuth2 flow
3. **Email address** — Update the "To" field in the Gmail node with your email

### Step 3 — Publish & get the webhook URL

1. Click **Publish** (top right) to activate the workflow
2. Open the Webhook node → switch to **Production URL** tab → copy the URL

### Step 4 — Update the upload page

1. Open `upload-page.html` in any text editor
2. Find the line: `const WEBHOOK_URL = '...'`
3. Replace with your production webhook URL
4. Save the file

### Step 5 — Launch

```bash
# Serve the upload page locally
cd multilingual-audio-transcriber
python -m http.server 8080

# Open http://localhost:8080/upload-page.html
```

---

## 📧 Email output

The email arrives with a polished HTML layout containing:

- **Clean English script** — Grammar-corrected, filler-free transcript
- **Summary** — 3-5 sentence overview of the recording
- **Action items & assignments** — Tasks, owners, and deadlines extracted automatically
- **Key details** — Important names, tools, platforms, and dates mentioned

Subject line format: `AI automated transcription for the audio file titled "[your title]"`

---

## 💰 Cost

| Component | Cost |
|-----------|------|
| OpenAI Whisper | ~$0.006 per minute of audio |
| GPT-4o-mini | ~$0.00015 per 1K input tokens |
| **Per 2-min audio file** | **~$0.01–0.02** |

---

## 🛠️ Tech stack

- **Frontend**: Vanilla HTML/CSS/JS — no frameworks, no build step
- **Workflow**: n8n (open-source workflow automation)
- **Transcription**: OpenAI Whisper API (translate mode: any language → English)
- **Summarisation**: OpenAI GPT-4o-mini (HTML-formatted output)
- **Email delivery**: Gmail API via n8n's Gmail node

---

## ⚙️ Supported audio formats

M4A, MP3, WAV, OGG, FLAC, AAC, WebM, MP4, MPEG — up to 25 MB

---

## 🐛 Troubleshooting

| Problem | Solution |
|---------|----------|
| "Failed to fetch" error | Check that the webhook URL in the HTML matches the n8n Production URL |
| CORS errors | Serve the HTML via `python -m http.server` instead of opening as `file://` |
| n8n shows no executions | Make sure the workflow is Published (green dot at the top) |
| Whisper returns empty text | Audio file may be corrupted or exceed 25 MB |
| Gmail fails | Re-authenticate Gmail credentials in n8n |
| Slow processing | Normal — Whisper takes 30–90 seconds for longer audio |
| Poor accuracy for a language | Manually select the correct language instead of using auto-detect |

---

## 🔮 Future enhancements

- [ ] Add Slack notification after email is sent
- [ ] Store all transcripts in Google Sheets for a searchable archive
- [ ] Support output in languages other than English
- [ ] Add speaker diarisation (who said what)
- [ ] Deploy the upload page to Vercel/Netlify for public access
- [ ] Add progress tracking via WebSocket

---

## 📄 License

MIT License — free to use, modify, and distribute.

---

## 🙏 Acknowledgements

- [n8n](https://n8n.io) — Open-source workflow automation
- [OpenAI Whisper](https://openai.com/research/whisper) — Multilingual speech recognition model
- [OpenAI GPT-4o-mini](https://openai.com/index/gpt-4o-mini-advancing-cost-efficient-intelligence/) — Language model for summarisation
