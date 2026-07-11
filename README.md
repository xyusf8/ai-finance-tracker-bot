# [Bot Name] — Personal Finance Telegram Bot

One-liner: A Telegram bot that automatically tracks and analyzes personal 
finances — supports text, receipt photos, and voice notes.

## 🎯 Problem
Brief: what problem this bot solves (e.g., manual expense tracking is 
tedious and easy to abandon; people need a fast way to log spending 
without opening a separate app).

## ✨ Features
- 📝 Text input — log transactions in natural language ("coffee 25k")
- 📸 Photo input — automatic OCR on receipt images
- 🎙️ Voice note input — automatic transcription & parsing
- 📊 On-demand & scheduled reports (daily/weekly)
- 💬 Interactive chat about your own financial data
- 🔀 Intent-based routing (Transaction / Report / Chat / Fallback)

## 🛠️ Tech Stack
- n8n (workflow automation)
- Groq (LLM — text parsing, fast & cheap)
- Gemini 2.0 Flash (multimodal — OCR & voice transcription)
- Google Sheets (data store)
- Telegram Bot API

## 🏗️ Architecture
[Screenshot/diagram of your n8n canvas here]
Brief explanation of the flow: all inputs are normalized into `$json.text`, 
then routed based on intent into one of 4 branches.

## 🚀 Setup
1. Clone this repo
2. Import `workflow.json` into n8n
3. Copy `.env.example` → fill in your own credentials
4. Set up Google Sheets using the template in `/sheets-template`
5. Connect your Telegram bot token, activate the workflow

## 📸 Demo
[GIF/demo video link]

## 💡 Lessons Learned (optional but adds portfolio value)
Keep it short — 2-3 interesting technical points, e.g. why Groq and 
Gemini were split by use case, or why parsing logic lives in Sheets 
expressions instead of n8n itself.

## 📄 License
MIT
