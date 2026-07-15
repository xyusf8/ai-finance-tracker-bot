![Stars](https://img.shields.io/github/stars/xyusf8/ai-finance-tracker-bot?style=flat)
![Forks](https://img.shields.io/github/forks/xyusf8/ai-finance-tracker-bot?style=flat)
![Last Commit](https://img.shields.io/github/last-commit/xyusf8/ai-finance-tracker-bot)

# 💸 FinBot — AI-Powered Personal Finance Assistant

> An intelligent Telegram bot that captures, organizes, and analyzes your personal finances in seconds using AI.

![Banner](docs/banner.png)

## 🎯 Why FinBot?

Traditional expense tracking is slow, repetitive, and easy to abandon. Most finance apps require multiple manual steps just to record a single transaction.

FinBot removes that friction.

Simply send a text message, receipt photo, or voice note through Telegram, and the AI automatically understands, categorizes, and stores your transaction—without opening another app.

## ✨ Key Features

- 💬 **Natural Language Input** — Log expenses in plain conversational text — no rigid command format required.
  Supports flexible date references like *"today," "yesterday,"* or a specific date (e.g., *"paid electricity bill on July 5th"*), and the transaction is recorded with the correct date automatically.
- 📸 **Receipt Scanner** — Extract transaction data automatically using AI-powered OCR.
- 🎙️ **Voice Recognition** — Talk to the bot instead of typing — voice notes are transcribed and routed through the same AI pipeline as text, so you can log a transaction, ask for a report, or ask a financial question, all by voice.
- 🤖 **AI Intent Detection** — Automatically understands whether you're recording a transaction, requesting a report, or asking financial questions.
- 📊 **Flexible Reports** — Generate daily, weekly, monthly, or custom-date reports — including breakdowns by specific category or payment method, simply by asking.
- 💡 **AI Financial Chat** — Ask questions about your own spending habits and receive contextual answers.
- ⚡ **Fully Automated Workflow** — No manual categorization or data entry required.

## 🛠️ Tech Stack

- ⚙️ **n8n** — Orchestrates the entire automation workflow
- 🧠 **Groq API** — Intent detection, transaction parsing, and AI-powered conversations
- 📄 **OCR.Space API** — Converts receipt images into structured text using OCR
- 🎙️ **ElevenLabs API** — Transcribes voice notes into text with high accuracy
- 📊 **Google Sheets API** — Stores and manages financial data in real time
- 💬 **Telegram Bot API** — Provides a fast, intuitive chat interface for users
  
## 🏗 Workflow Architecture

> Every incoming message is normalized into a single data format before being processed by an AI-powered routing system.

```
Telegram
      │
      ▼
Normalize Input
(Text / Photo / Voice)
      │
      ▼
AI Intent Detection
      │
 ┌────┼────┬─────┐
 │    │    │     │
 ▼    ▼    ▼     ▼
Transaction Report Chat Fallback
```

![workflow](docs/workflow.png)

### How It Works

**1. Input Normalization**
Photos are sent to OCR space for OCR extraction; voice notes are transcribed by Eleven Labs model. Both outputs, along with raw text input, converge into the same `$json.text` field — so every downstream node only ever deals with plain text.

**2. Intent Detection**
A Groq-powered classification step reads the normalized text and routes it into one of four branches. Getting this reliable — especially for casual, mixed-language input — required few-shot examples in the prompt and a temperature of 0 to keep classification consistent rather than creative.

**3. Transaction Branch**
Parses amount, category, and description from natural language, then appends a structured row to Google Sheets. Output is constrained to a strict pipe-separated format so the parser never has to guess field boundaries.

**4. Report Branch**
Pulls the relevant date range from Sheets and generates a natural-language summary — daily reports run on a schedule, weekly/custom reports run on request.

**5. Chat Branch**
Uses an AI agent with tool access to the user's own Sheet, allowing free-form questions like *"what did I spend the most on this month?"* to be answered with real data instead of a canned response.

**6. Fallback Branch**
Catches anything that doesn't clearly match the other three, prompting the user for clarification instead of failing silently.


## 🚀 Getting Started

1. Clone this repository.
2. Import `workflow.json` into n8n.
3. Copy `.env.example` and configure your credentials.
4. Create a Google Sheet using the provided template.
5. Connect your Telegram Bot Token.
6. Activate the workflow.

## 📸 Demo

- 🎥 Demo Video
- 📷 Workflow Screenshot
- 📊 Example Reports

## 📚 What I Learned

Building FinBot taught me how to:

- **Design scalable AI workflows** using n8n's visual branching instead of hardcoded conditionals, making the system easier to debug and extend.
- **Combine multiple AI models based on their strengths** — routing latency-sensitive text tasks to Groq while reserving Gemini for multimodal inputs it handles natively.
- **Process text, images, and voice through a unified pipeline** by normalizing every input type to a single format before any downstream logic runs.
- **Build reliable intent-routing systems** for conversational apps, including handling misclassification on informal, mixed-language input through prompt engineering rather than more complex code.


## 🔮 Future Improvements

- Monthly analytics dashboard
- Budget tracking
- Multi-currency support
- Spending notifications
- PostgreSQL support
- Multi-user authentication

## 📄 License

Licensed under the MIT License.
