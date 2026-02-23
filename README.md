# Blimp Marketing Agency – AI Agent (Proposal / Audit / Content Calendar / Admin) | n8n Workflow

This repository contains an **n8n chat-based AI agent workflow** that automatically generates:

- ✅ Marketing **Proposal**
- ✅ Digital **Audit Report**
- ✅ 30-Day **Content Calendar**
- ✅ **Admin outputs** (emails, follow-ups, meeting agenda, onboarding checklist, simple reports)

It works by receiving a chat message, extracting key client details (name, budget, links, goal, etc.), routing to the correct task, generating content using an LLM (Groq OpenAI-compatible API), and exporting the final output as an **HTML file**.

---

## ✅ What This Workflow Does (Simple Explanation)

1. You send a message in chat (normal English/Urdu/roman Urdu or JSON).
2. Workflow detects the task:  
   - `proposal` / `audit` / `content_calendar` / `admin`
3. It extracts details like:
   - Client name, email, industry, goal, budget, links, timeline
4. It calls the AI model through **Groq API**.
5. It generates professional output in English.
6. It converts the response into a downloadable **HTML file**.

---

## 🧠 Node-by-Node Overview

### 1) When chat message received
- Starts the workflow when a chat message is received (Chat Trigger).

### 2) Code in JavaScript
- Reads the message and extracts:
  - `task`, `client_name`, `client_email`, `industry`, `goal`, `budget`, `timeline`, `services_needed`, `client_links`
- Auto-detects task type based on keywords.

### 3) Switch
- Routes the workflow into the correct path:
  - **Proposal**
  - **Audit**
  - **Content Calendar**
  - **Admin**

### 4) (Blimp Knowledge) Set Nodes
- Injects Blimp fixed details:
  - Services
  - Pricing Rules
  - Terms
  - Tone
  - Website (https://www.blimp.pk)

### 5) HTTP Request Nodes (Groq AI Call)
- Sends prompt to Groq OpenAI-compatible endpoint:
  - `/openai/v1/chat/completions`

### 6) Extract Text Nodes
- Extracts the AI response from:
  - `choices[0].message.content`

### 7) File Content Nodes
- Cleans and prepares content for exporting.

### 8) Convert to File Nodes
- Outputs HTML file:
  - `Client - proposal.html`
  - `Client - Audit.html`
  - `Client - calender.html`
  - `Client - admin.html`

---

## ⚙️ Requirements

- n8n (cloud or self-hosted)
- Groq API key (OpenAI-compatible)
- Optional: GitHub repo for versioning workflow JSON

---

## 🔒 Security (API Key Hidden / Safe Setup)

⚠️ **Never hardcode your API key inside workflow JSON.**  
Instead, store it in **n8n Credentials**.

### ✅ Recommended Safe Method (n8n Credentials)

1. In n8n go to:  
   **Credentials → New**
2. Create: **HTTP Header Auth**
3. Set:
   - Header Name: `Authorization`
   - Header Value: `Bearer YOUR_GROQ_API_KEY`
4. Attach this credential to all HTTP nodes:
   - `http_Proposal`
   - `HTTP Request (Audit)`
   - `HTTP Request (Calender)`
   - `HTTP Request (Admin)`

✅ Now your workflow JSON will not contain the key.

---

## ✅ How To Import and Run (Step-by-Step)

### 1) Import the workflow
- n8n → Workflows → Import → Paste JSON (or upload `workflow.json`)

### 2) Add credentials
- Create the Groq credential as above
- Connect it to all HTTP Request nodes

### 3) Activate workflow
- Click **Active** to enable automation

### 4) Test it
Send messages like:

**Proposal**
